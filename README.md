<!DOCTYPE html>
<html lang="az">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WİN_WİN</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: sans-serif; }
        body { background: #121212; color: #fff; display: flex; flex-direction: column; height: 100vh; }
        header { background: #1f1f1f; padding: 15px; text-align: center; font-size: 20px; font-weight: bold; border-bottom: 1px solid #333; }
        .bal-container { text-align: center; padding: 10px; background: #262626; font-size: 14px; }
        .main-container { flex: 1; display: flex; flex-direction: column; overflow: hidden; position: relative; }
        .video-feed { flex: 1; overflow-y: scroll; scroll-snap-type: y mandatory; display: flex; flex-direction: column; }
        .video-item { min-height: 100%; scroll-snap-align: start; background: #000; display: flex; align-items: center; justify-content: center; position: relative; }
        .video-item video { width: 100%; height: 100%; object-fit: cover; }
        .sidebar-controls { position: absolute; right: 15px; bottom: 80px; display: flex; flex-direction: column; gap: 15px; align-items: center; z-index: 10; }
        .control-btn { background: rgba(0,0,0,0.6); border: none; color: white; padding: 10px; border-radius: 50%; cursor: pointer; font-size: 18px; width: 45px; height: 45px; display: flex; align-items: center; justify-content: center; }
        .chat-overlay { position: absolute; bottom: 10px; left: 10px; right: 70px; max-height: 150px; overflow-y: auto; background: rgba(0,0,0,0.4); padding: 5px; border-radius: 5px; font-size: 13px; z-index: 10; }
        .chat-input-area { display: flex; padding: 10px; background: #1f1f1f; gap: 5px; }
        .chat-input-area input { flex: 1; padding: 10px; border-radius: 20px; border: none; background: #333; color: white; outline: none; }
        .chat-input-area button { padding: 10px 15px; border-radius: 20px; border: none; background: #fe2c55; color: white; cursor: pointer; font-weight: bold; }
        .nav-bar { display: flex; background: #1f1f1f; border-top: 1px solid #333; }
        .nav-btn { flex: 1; padding: 15px; text-align: center; background: none; border: none; color: #888; font-size: 14px; cursor: pointer; }
        .nav-btn.active { color: #fff; font-weight: bold; }
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 100; justify-content: center; align-items: center; }
        .modal-content { background: #1f1f1f; width: 90%; max-width: 400px; padding: 20px; border-radius: 10px; max-height: 80vh; overflow-y: auto; }
        .shop-item { display: flex; justify-content: space-between; align-items: center; background: #2a2a2a; padding: 10px; margin-bottom: 10px; border-radius: 5px; }
        .shop-item button { background: #fe2c55; border: none; color: white; padding: 5px 10px; border-radius: 5px; cursor: pointer; }
        .gift-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 10px; margin-top: 15px; }
        .gift-btn { background: #333; border: none; font-size: 20px; padding: 10px; border-radius: 5px; cursor: pointer; text-align: center; }
        .floating-gift { position: absolute; font-size: 30px; animation: flyUp 2s ease-out forwards; z-index: 20; }
        @keyframes flyUp { 0% { bottom: 80px; opacity: 1; transform: scale(1); } 100% { bottom: 300px; opacity: 0; transform: scale(1.5); } }
    </style>
</head>
<body>

    <header>WİN_WİN</header>
    <div class="bal-container">Balın: <span id="userBal">100</span> 🪙 | Nik Rəngi: <span id="currentNickColor" style="color:white">Normal</span></div>

    <div class="main-container">
        <!-- VİDEO LENTA -->
        <div class="video-feed" id="videoFeed">
            <div class="video-item">
                <video src="https://www.w3schools.com/html/mov_bbb.mp4" loop autoplay muted></video>
                <div class="chat-overlay" id="chatOverlay">
                    <div><b>İstifadəçi1:</b> Salam!</div>
                </div>
                <div class="sidebar-controls">
                    <button class="control-btn" onclick="openShopModal()">🛒</button>
                    <button class="control-btn" onclick="openGiftModal()">🎁</button>
                    <button class="control-btn" onclick="earnBal()">💰</button>
                </div>
            </div>
        </div>

        <!-- MESAJ YAZMA PANELİ -->
        <div class="chat-input-area">
            <input type="text" id="messageInput" placeholder="Yaz...">
            <button onclick="sendMessage()">Göndər</button>
        </div>
    </div>

    <!-- ALT MENYU -->
    <div class="nav-bar">
        <button class="nav-btn active">Əsas</button>
        <button class="nav-btn" onclick="openShopModal()">Mağaza</button>
    </div>

    <!-- MAĞAZA MODALI -->
    <div class="modal" id="shopModal">
        <div class="modal-content">
            <h3>Mağaza</h3>
            <p style="margin: 10px 0; font-size: 13px; color: #aaa;">Rəngli Niklər (30 Bal)</p>
            <div class="shop-item"><span>Yaşıl Nik</span> <button onclick="buyItem('nick', 'green', 30)">Al</button></div>
            <div class="shop-item"><span>Sarı Nik</span> <button onclick="buyItem('nick', 'yellow', 30)">Al</button></div>
            <div class="shop-item"><span>Qırmızı Nik</span> <button onclick="buyItem('nick', 'red', 30)">Al</button></div>
            <div class="shop-item"><span>Bənövşəyi Nik</span> <button onclick="buyItem('nick', 'purple', 30)">Al</button></div>
            <div class="shop-item"><span>Göy Nik</span> <button onclick="buyItem('nick', 'blue', 30)">Al</button></div>

            <p style="margin: 15px 0 10px 0; font-size: 13px; color: #aaa;">Rəngli Mesajlar (30 Bal)</p>
            <div class="shop-item"><span>Yaşıl Mesaj</span> <button onclick="buyItem('msg', 'green', 30)">Al</button></div>
            <div class="shop-item"><span>Sarı Mesaj</span> <button onclick="buyItem('msg', 'yellow', 30)">Al</button></div>
            <div class="shop-item"><span>Qırmızı Mesaj</span> <button onclick="buyItem('msg', 'red', 30)">Al</button></div>
            <div class="shop-item"><span>Göy Mesaj</span> <button onclick="buyItem('msg', 'blue', 30)">Al</button></div>
            <div class="shop-item"><span>Bənövşəyi Mesaj</span> <button onclick="buyItem('msg', 'purple', 30)">Al</button></div>
            
            <button onclick="closeShopModal()" style="width:100%; margin-top:15px; padding:10px; background:#444; border:none; color:white; border-radius:5px; cursor:pointer;">Bağla</button>
        </div>
    </div>

    <!-- HƏDİYYƏ MODALI -->
    <div class="modal" id="giftModal">
        <div class="modal-content">
            <h3>Hədiyyə At (Hər biri 25 Bal)</h3>
            <div class="gift-grid" id="giftGrid"></div>
            <button onclick="closeGiftModal()" style="width:100%; margin-top:15px; padding:10px; background:#444; border:none; color:white; border-radius:5px; cursor:pointer;">Bağla</button>
        </div>
    </div>
