<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple JavaScript Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #282c34;
            color: white;
            font-family: Arial, sans-serif;
        }

        canvas {
            border: 2px solid white;
            background-color: #333;
        }

        #score {
            position: absolute;
            top: 20px;
            left: 20px;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <div id="score">Score: 0</div>
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        const player = {
            x: 200,
            y: 350,
            size: 20,
            speed: 5
        };

        const enemy = {
            x: Math.random() * 380,
            y: 0,
            size: 20,
            speed: 2
        };

        let score = 0;
        let gameRunning = true;

        function drawPlayer() {
            ctx.fillStyle = 'blue';
            ctx.fillRect(player.x, player.y, player.size, player.size);
        }

        function drawEnemy() {
            ctx.fillStyle = 'red';
            ctx.fillRect(enemy.x, enemy.y, enemy.size, enemy.size);
        }

        function updateEnemy() {
            enemy.y += enemy.speed;
            if (enemy.y > canvas.height) {
                enemy.y = 0;
                enemy.x = Math.random() * (canvas.width - enemy.size);
                score++;
                document.getElementById('score').innerText = `Score: ${score}`;
                enemy.speed += 0.2; // Increase difficulty
            }
        }

        function checkCollision() {
            if (
                player.x < enemy.x + enemy.size &&
                player.x + player.size > enemy.x &&
                player.y < enemy.y + enemy.size &&
                player.y + player.size > enemy.y
            ) {
                gameRunning = false;
                alert(`Game Over! Your score: ${score}`);
                document.location.reload();
            }
        }

        function movePlayer(e) {
            if (e.key === 'ArrowLeft' && player.x > 0) {
                player.x -= player.speed;
            } else if (e.key === 'ArrowRight' && player.x < canvas.width - player.size) {
                player.x += player.speed;
            }
        }

        function gameLoop() {
            if (!gameRunning) return;

            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawPlayer();
            drawEnemy();
            updateEnemy();
            checkCollision();
            requestAnimationFrame(gameLoop);
        }

        document.addEventListener('keydown', movePlayer);
        gameLoop();
    </script>
</body>
</html>

        
