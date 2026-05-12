<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Yeast Fermentation Lab Game</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: #f0f4f8; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .game-container { background: white; padding: 20px; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); width: 400px; text-align: center; }
        .flask { width: 100px; height: 120px; background: #ffd966; margin: 20px auto; border-radius: 0 0 50px 50px; position: relative; border: 4px solid #333; overflow: hidden; }
        .bubbles { position: absolute; bottom: 0; width: 100%; height: 0%; background: rgba(255,255,255,0.4); transition: height 0.5s; }
        .controls { display: grid; gap: 10px; margin-top: 20px; }
        .stat { font-weight: bold; color: #2c3e50; margin: 5px 0; }
        button { padding: 10px; border: none; border-radius: 5px; background: #3498db; color: white; cursor: pointer; }
        button:hover { background: #2980b9; }
        .alert { color: #e74c3c; font-size: 0.9em; height: 20px; }
    </style>
</head>
<body>

<div class="game-container">
    <h2>🧪 Yeast Lab Game</h2>
    <p>ช่วยยีสต์หมักน้ำผลไม้ให้ได้แอลกอฮอล์เยอะๆ!</p>
    
    <div class="flask" id="flask">
        <div class="bubbles" id="bubbles"></div>
    </div>

    <div class="stat">ระดับน้ำตาล: <span id="sugar-val">50</span> g</div>
    <div class="stat">อุณหภูมิ: <span id="temp-val">25</span> °C</div>
    <div class="stat">แอลกอฮอล์ที่ได้: <span id="alc-val">0</span> %</div>
    <div id="msg" class="alert"></div>

    <div class="controls">
        <button onclick="addSugar()">+ เติมน้ำตาล</button>
        <button onclick="adjustTemp(5)">+ เพิ่มอุณหภูมิ</button>
        <button onclick="adjustTemp(-5)">- ลดอุณหภูมิ</button>
        <button onclick="resetGame()" style="background:#95a5a6;">เริ่มใหม่</button>
    </div>
</div>

<script>
    let sugar = 50;
    let temp = 25;
    let alcohol = 0;
    let gameActive = true;

    function addSugar() {
        if(!gameActive) return;
        sugar += 10;
        updateUI();
    }

    function adjustTemp(change) {
        if(!gameActive) return;
        temp += change;
        updateUI();
    }

    function updateUI() {
        document.getElementById('sugar-val').innerText = sugar;
        document.getElementById('temp-val').innerText = temp;
        document.getElementById('alc-val').innerText = alcohol.toFixed(2);
        
        // กราฟิกฟองก๊าซ CO2
        document.getElementById('bubbles').style.height = (alcohol * 5) + "%";
    }

    // Logic ของการหมัก (Game Loop)
    setInterval(() => {
        if(!gameActive) return;

        let msg = "";
        let rate = 0;

        // เงื่อนไขอุณหภูมิ (ยีสต์ชอบ 30-37 องศา)
        if (temp >= 30 && temp <= 40) {
            rate = 0.5;
        } else if (temp > 40) {
            msg = "⚠️ ร้อนเกินไป ยีสต์กำลังจะตาย!";
            rate = 0.05;
        } else {
            msg = "❄️ เย็นเกินไป ยีสต์ทำงานช้า";
            rate = 0.1;
        }

        // เงื่อนไขน้ำตาล
        if (sugar > 0) {
            let production = rate * (sugar / 100);
            alcohol += production;
            sugar -= production * 0.5;
        } else {
            msg = "🍽️ น้ำตาลหมดแล้ว! การหมักหยุดลง";
        }

        if (alcohol >= 20) {
            msg = "🏆 สำเร็จ! ได้ไวน์ผลไม้เข้มข้น";
            gameActive = false;
        }

        document.getElementById('msg').innerText = msg;
        updateUI();
    }, 1000);

    function resetGame() {
        sugar = 50; temp = 25; alcohol = 0; gameActive = true;
        updateUI();
    }
</script>

</body>
</html>
