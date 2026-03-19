# opticstreams
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Sahil's GameZone | Pro Gaming</title>
    <style>
        :root { --neon-blue: #00f2ff; --neon-purple: #bc13fe; --bg: #0a0a0a; }
        body { background: var(--bg); color: white; font-family: 'Courier New', monospace; text-align: center; margin: 0; }
        
        /* HEADER */
        header { padding: 20px; border-bottom: 2px solid var(--neon-blue); text-shadow: 0 0 10px var(--neon-blue); }
        h1 { margin: 0; font-size: 35px; }

        /* GAME AREA */
        .container { display: flex; flex-wrap: wrap; justify-content: center; gap: 30px; padding: 40px; }
        .game-card { background: #111; border: 2px solid #333; border-radius: 15px; padding: 20px; width: 320px; transition: 0.3s; }
        .game-card:hover { border-color: var(--neon-purple); box-shadow: 0 0 20px var(--neon-purple); transform: translateY(-10px); }

        /* SNAKE GAME CANVAS */
        canvas { background: #000; display: block; margin: 10px auto; border: 1px solid #444; }
        
        /* BUTTONS */
        .btn { background: transparent; border: 2px solid var(--neon-blue); color: var(--neon-blue); padding: 10px 20px; cursor: pointer; font-weight: bold; margin-top: 10px; }
        .btn:hover { background: var(--neon-blue); color: black; }

        footer { margin-top: 50px; color: #555; padding: 20px; border-top: 1px solid #222; }
    </style>
</head>
<body>

<header>
    <h1>SAHIL'S GAMEZONE</h1>
    <p>Optimized for Lenovo G580 High-Performance</p>
</header>

<div class="container">
    <div class="game-card">
        <h2>🐍 Neon Snake</h2>
        <canvas id="snakeGame" width="300" height="300"></canvas>
        <p>Score: <span id="score">0</span></p>
        <button class="btn" onclick="resetSnake()">Restart Game</button>
    </div>

    <div class="game-card">
        <h2>⚡ Click Master</h2>
        <p>How many clicks in 10 seconds?</p>
        <h1 id="timer">10s</h1>
        <h2 id="clickCount">0</h2>
        <button class="btn" id="clickBtn" onclick="countClick()">CLICK ME!</button>
        <button class="btn" onclick="resetClickTest()">Reset</button>
    </div>
</div>

<footer>
    <h3>Developed by Sahil Shelke</h3>
</footer>

<script>
    // --- SNAKE GAME LOGIC ---
    const canvas = document.getElementById("snakeGame");
    const ctx = canvas.getContext("2d");
    let box = 15;
    let snake = [{x: 10 * box, y: 10 * box}];
    let food = { x: Math.floor(Math.random() * 19 + 1) * box, y: Math.floor(Math.random() * 19 + 1) * box };
    let score = 0;
    let d;

    document.addEventListener("keydown", direction);
    function direction(event) {
        if(event.keyCode == 37 && d != "RIGHT") d = "LEFT";
        else if(event.keyCode == 38 && d != "DOWN") d = "UP";
        else if(event.keyCode == 39 && d != "LEFT") d = "RIGHT";
        else if(event.keyCode == 40 && d != "UP") d = "DOWN";
    }

    function drawSnake() {
        ctx.fillStyle = "black";
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        
        for(let i = 0; i < snake.length; i++) {
            ctx.fillStyle = (i == 0) ? "#00f2ff" : "#bc13fe";
            ctx.fillRect(snake[i].x, snake[i].y, box, box);
        }
        
        ctx.fillStyle = "red";
        ctx.fillRect(food.x, food.y, box, box);

        let snakeX = snake[0].x;
        let snakeY = snake[0].y;

        if( d == "LEFT") snakeX -= box;
        if( d == "UP") snakeY -= box;
        if( d == "RIGHT") snakeX += box;
        if( d == "DOWN") snakeY += box;

        if(snakeX == food.x && snakeY == food.y) {
            score++;
            document.getElementById("score").innerHTML = score;
            food = { x: Math.floor(Math.random() * 19 + 1) * box, y: Math.floor(Math.random() * 19 + 1) * box };
        } else {
            snake.pop();
        }

        let newHead = { x: snakeX, y: snakeY };

        if(snakeX < 0 || snakeX >= canvas.width || snakeY < 0 || snakeY >= canvas.height || collision(newHead, snake)) {
            clearInterval(game);
            alert("GAME OVER! Score: " + score);
        }
        snake.unshift(newHead);
    }

    function collision(head, array) {
        for(let i = 0; i < array.length; i++) { if(head.x == array[i].x && head.y == array[i].y) return true; }
        return false;
    }
    let game = setInterval(drawSnake, 100);
    function resetSnake() { location.reload(); }

    // --- CLICK TEST LOGIC ---
    let clicks = 0;
    let time = 10;
    let started = false;
    let timerId;

    function countClick() {
        if(!started) {
            started = true;
            timerId = setInterval(() => {
                time--;
                document.getElementById("timer").innerHTML = time + "s";
                if(time == 0) {
                    clearInterval(timerId);
                    document.getElementById("clickBtn").disabled = true;
                    alert("Time up! Your CPS is " + (clicks/10));
                }
            }, 1000);
        }
        clicks++;
        document.getElementById("clickCount").innerHTML = clicks;
    }
    function resetClickTest() { location.reload(); }
</script>

</body>
</html>
