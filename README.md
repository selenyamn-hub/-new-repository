<!DOCTYPE html>
<html lang="az">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BURJU KAZİNO</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body {
            background-color: #0b0e14;
            color: #fff;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        /* Əsas Oyun Paneli (Əvvəlcə gizlidir, kod təsdiqlənəndən sonra açılır) */
        #game-section {
            display: none;
            width: 100%;
            max-width: 350px;
            flex-direction: column;
            align-items: center;
        }
        .header {
            background: #161b22;
            border: 1px solid #30363d;
            border-radius: 10px;
            padding: 10px 15px;
            width: 100%;
            max-width: 350px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
            box-sizing: border-box;
        }
        .balance-title {
            font-size: 12px;
            color: #8b949e;
        }
        .balance-amount {
            font-size: 18px;
            font-weight: bold;
            color: #3fb950;
        }
        .section-title {
            font-size: 14px;
            margin-bottom: 8px;
            color: #f0883e;
            font-weight: bold;
            width: 100%;
        }
        .games-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 8px;
            margin-bottom: 12px;
            width: 100%;
        }
        .game-card {
            background: linear-gradient(145deg, #1f242c, #111418);
            border: 1px solid #30363d;
            border-radius: 10px;
            padding: 12px;
            text-align: center;
            cursor: pointer;
        }
        .game-card:active {
            border-color: #f0883e;
        }
        .game-icon {
            font-size: 24px;
            margin-bottom: 4px;
        }
        .game-name {
            font-size: 13px;
            font-weight: bold;
        }
        .slot-machine {
            background: #161b22;
            border: 1px solid #30363d;
            border-radius: 10px;
            padding: 12px;
            text-align: center;
            margin-bottom: 12px;
            width: 100%;
            box-sizing: border-box;
        }
        .slots-display {
            display: flex;
            justify-content: center;
            gap: 10px;
            font-size: 28px;
            background: #000;
            padding: 8px;
            border-radius: 6px;
            margin-bottom: 10px;
            border: 2px inset #222;
        }
        .spin-btn {
            background: linear-gradient(145deg, #238636, #2ea043);
            color: #fff;
            border: none;
            padding: 10px;
            font-size: 14px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
        }
        /* Kod Paneli (Keypad) */
        #auth-section {
            width: 100%;
            max-width: 350px;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 40px;
        }
        .keypad-container {
            background: #161b22;
            border: 1px solid #30363d;
            border-radius: 12px;
            padding: 15px;
            box-sizing: border-box;
            width: 100%;
            box-shadow: 0 5px 15px rgba(0,0,0,0.8);
        }
        .screen {
            background-color: #0d1117;
            border: 1px solid #30363d;
            border-radius: 6px;
            padding: 8px 10px;
            margin-bottom: 12px;
            text-align: center;
        }
        .screen-title {
            color: #8b949e;
            font-size: 11px;
            margin-bottom: 4px;
        }
        .screen-input {
            color: #3fb950;
            font-size: 20px;
            font-family: monospace;
            letter-spacing: 2px;
            min-height: 24px;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 8px;
        }
        .btn {
            background: #21262d;
            border: 1px solid #30363d;
            border-radius: 6px;
            color: #fff;
            font-size: 18px;
            font-weight: bold;
            padding: 12px 0;
            cursor: pointer;
        }
        .btn:active {
            background: #30363d;
        }
        .btn-red {
            background: #da3633;
            border-color: #f85149;
            font-size: 14px;
        }
        .btn-green {
            background: #238636;
            border-color: #2ea043;
            font-size: 14px;
        }
    </style>
</head>
<body>

    <!-- 1. KOD DAXİL ETMƏ HİSSƏSİ (Oyunlardan qabaq açılır) -->
    <div id="auth-section">
        <div style="text-align: center; margin-bottom: 15px;">
            <h2 style="color: #f0883e; margin: 0 0 5px 0;">BURJU KAZİNO</h2>
            <p style="color: #8b949e; font-size: 12px; margin: 0;">Botdan aldığınız 8 rəqəmli kodu daxil edin</p>
        </div>
        <div class="keypad-container">
            <div class="screen">
                <div class="screen-title">Access Code (XXXX-XXXX):</div>
                <div class="screen-input" id="display">---- ----</div>
            </div>
            <div class="grid">
                <button class="btn" onclick="press('1')">1</button>
                <button class="btn" onclick="press('2')">2</button>
                <button class="btn" onclick="press('3')">3</button>
                <button class="btn" onclick="press('4')">4</button>
                <button class="btn" onclick="press('5')">5</button>
                <button class="btn" onclick="press('6')">6</button>
                <button class="btn" onclick="press('7')">7</button>
                <button class="btn" onclick="press('8')">8</button>
                <button class="btn" onclick="press('9')">9</button>
                <button class="btn btn-red" onclick="clearCode()">⬅️ Sil</button>
                <button class="btn" onclick="press('0')">0</button>
                <button class="btn btn-green" onclick="submitCode()">OK</button>
            </div>
        </div>
    </div>

    <!-- 2. OYUN VƏ BALANS HİSSƏSİ (Kod təsdiqlənəndən sonra açılır) -->
    <div id="game-section">
        <div class="header">
            <div>
                <div class="balance-title">Balansınız:</div>
                <div class="balance-amount" id="balance">0.00 ₼</div>
            </div>
            <button style="background: #21262d; color: #f0883e; border: 1px solid #30363d; padding: 6px 10px; border-radius: 6px; cursor: pointer; font-size: 11px;" onclick="location.reload()">🔒 Çıxış</button>
        </div>

        <div class="section-title">🌟 Ulduz Oyunları</div>
        <div class="games-grid">
            <div class="game-card" onclick="playGame('Hot Sevens')">
                <div class="game-icon">🔥</div>
                <div class="game-name">Hot Sevens</div>
            </div>
            <div class="game-card" onclick="playGame('Aviatop')">
                <div class="game-icon">✈️</div>
                <div class="game-name">Aviatop</div>
            </div>
        </div>

        <div class="slot-machine">
            <div class="section-title" style="margin-top:0; font-size:12px;">🎰 Hot Sevens (Mərc: 1 ₼)</div>
            <div class="slots-display">
                <span id="s1">7️⃣</span>
                <span id="s2">🍒</span>
                <span id="s3">🍋</span>
            </div>
            <button class="spin-btn" onclick="spinSlot()">START</button>
        </div>
    </div>

    <script>
        let tg = window.Telegram.WebApp;
        tg.expand();

        let currentCode = "";

        function press(num) {
            if (currentCode.length < 8) {
                currentCode += num;
                updateDisplay();
            }
        }

        function clearCode() {
            currentCode = currentCode.slice(0, -1);
            updateDisplay();
        }

        function updateDisplay() {
            let formatted = currentCode;
            if (currentCode.length > 4) {
                formatted = currentCode.slice(0, 4) + "-" + currentCode.slice(4);
            }
            document.getElementById('display').innerText = formatted.padEnd(9, '-');
        }

        function submitCode() {
            if (currentCode.length === 8) {
                // Kodu bota göndəririk ki, balans yüklənsin
                tg.sendData(currentCode);
            } else {
                alert("Lütfən 8 rəqəmli kodu tam daxil edin!");
            }
        }

        function playGame(gameName) {
            alert(gameName + " oyununa daxil olursunuz!");
        }

        function spinSlot() {
            alert("Mərc qəbul olundu!");
        }
    </script>
</body>
</html>
