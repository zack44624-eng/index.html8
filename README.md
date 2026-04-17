# index.html8
小恐龍遊戲
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Dino Mini</title>
    <style>
        body { margin: 0; background: #222; display: flex; justify-content: center; align-items: center; height: 100vh; }
        canvas { background: #87CEEB; border: 2px solid #fff; }
    </style>
</head>
<body>
    <canvas id="g"></canvas>

<script>
    const canvas = document.getElementById('g');
    const ctx = canvas.getContext('2d');
    canvas.width = 800; canvas.height = 200;

    // 1. 直接定義圖片路徑 (簡化成直接賦值)
    const imgDino = new Image(); imgDino.src = 'dino.png';
    const imgCactus = new Image(); imgCactus.src = 'cactus.png';
    const imgBird = new Image(); imgBird.src = 'bird.png';

    let score = 0, gameSpeed = 5, isDead = false;
    let dino = { x: 50, y: 150, w: 40, h: 40, dy: 0, jump: -12 };
    let obstacles = [];

    // 2. 簡易輸入控制
    window.onclick = window.onkeydown = () => {
        if (isDead) location.reload();
        if (dino.y === 150) dino.dy = dino.jump;
    };

    function loop() {
        if (isDead) return;
        ctx.clearRect(0, 0, 800, 200);

        // 分數與速度
        score++;
        gameSpeed += 0.001;

        // 恐龍物理
        dino.dy += 0.6; // 重力
        dino.y += dino.dy;
        if (dino.y > 150) { dino.y = 150; dino.dy = 0; }

        // 繪製恐龍
        ctx.drawImage(imgDino, dino.x, dino.y, dino.w, dino.h);

        // 障礙物邏輯
        if (Math.random() < 0.02) {
            let isBird = Math.random() > 0.8;
            obstacles.push({ x: 800, y: isBird ? 80 : 160, w: 30, h: 30, img: isBird ? imgBird : imgCactus });
        }

        obstacles.forEach((o, i) => {
            o.x -= gameSpeed;
            ctx.drawImage(o.img, o.x, o.y, o.w, o.h);

            // 簡易碰撞判斷
            if (o.x < dino.x + dino.w && o.x + o.w > dino.x && o.y < dino.y + dino.h && o.y + o.h > dino.y) {
                isDead = true;
                alert("Game Over! Score: " + score);
            }
            if (o.x < -50) obstacles.splice(i, 1);
        });

        ctx.fillStyle = "black";
        ctx.fillText("Score: " + score, 10, 20);
        requestAnimationFrame(loop);
    }

    loop();
</script>
</body>
</html>
