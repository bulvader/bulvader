<!DOCTYPE html>
<html lang="hu">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Játék</title>
    <style>
        body { display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        canvas { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const gridSize = 20;
        let snake = [{ x: 200, y: 200 }];
        let food = { x: Math.floor(Math.random() * 20) * gridSize, y: Math.floor(Math.random() * 20) * gridSize };
        let direction = { x: 0, y: 0 };
        let score = 0;

        function gameLoop() {
            // Mozgatás
            const head = { x: snake[0].x + direction.x * gridSize, y: snake[0].y + direction.y * gridSize };
            snake.unshift(head);

            // Étkezés
            if (head.x === food.x && head.y === food.y) {
                score++;
                food = { x: Math.floor(Math.random() * 20) * gridSize, y: Math.floor(Math.random() * 20) * gridSize };
            } else {
                snake.pop();
            }

            // Falak és önmagába harapás ellenőrzése
            if (head.x < 0 || head.x >= canvas.width || head.y < 0 || head.y >= canvas.height || 
                snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)) {
                alert(`Játék vége! Pontszám: ${score}`);
                snake = [{ x: 200, y: 200 }];
                direction = { x: 0, y: 0 };
                score = 0;
            }

            // Rajzolás
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = "green";
            snake.forEach(segment => ctx.fillRect(segment.x, segment.y, gridSize, gridSize));

            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, gridSize, gridSize);
        }

        function changeDirection(event) {
            switch (event.key) {
                case "ArrowUp":
                    if (direction.y === 0) direction = { x: 0, y: -1 };
                    break;
                case "ArrowDown":
                    if (direction.y === 0) direction = { x: 0, y: 1 };
                    break;
                case "ArrowLeft":
                    if (direction.x === 0) direction = { x: -1, y: 0 };
                    break;
                case "ArrowRight":
                    if (direction.x === 0) direction = { x: 1, y: 0 };
                    break;
            }
        }

        document.addEventListener("keydown", changeDirection);
        setInterval(gameLoop, 100);
    </script>
</body>
</html>
