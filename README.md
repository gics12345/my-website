# my-website
1234
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Simple Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin-top: 50px;
    }
    .calculator {
      display: inline-block;
      padding: 20px;
      border: 2px solid #ccc;
      border-radius: 10px;
    }
    input[type="text"] {
      width: 200px;
      font-size: 1.2em;
      margin-bottom: 10px;
      padding: 5px;
      text-align: right;
    }
    .buttons button {
      width: 45px;
      height: 45px;
      font-size: 1.2em;
      margin: 5px;
    }
  </style>
</head>
<body>
  <div class="calculator">
    <input type="text" id="display" disabled>
    <div class="buttons">
      <br>
      <button onclick="append('7')">7</button>
      <button onclick="append('8')">8</button>
      <button onclick="append('9')">9</button>
      <button onclick="append('/')">/</button>
      <br>
      <button onclick="append('4')">4</button>
      <button onclick="append('5')">5</button>
      <button onclick="append('6')">6</button>
      <button onclick="append('*')">*</button>
      <br>
      <button onclick="append('1')">1</button>
      <button onclick="append('2')">2</button>
      <button onclick="append('3')">3</button>
      <button onclick="append('-')">-</button>
      <br>
      <button onclick="append('0')">0</button>
      <button onclick="append('.')">.</button>
      <button onclick="calculate()">=</button>
      <button onclick="append('+')">+</button>
      <br>
      <button onclick="clearDisplay()">C</button>
    </div>
  </div>

  <script>
    function append(char) {
      document.getElementById('display').value += char;
    }

    function clearDisplay() {
      document.getElementById('display').value = '';
    }

    function calculate() {
      try {
        const result = eval(document.getElementById('display').value);
        document.getElementById('display').value = result;
      } catch {
        document.getElementById('display').value = 'Error';
      }
    }
  </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Snake Game</title>
  <style>
    body {
      background-color: #222;
      color: #fff;
      text-align: center;
      font-family: Arial, sans-serif;
    }

    canvas {
      background-color: #111;
      display: block;
      margin: 30px auto;
      border: 2px solid #444;
    }

    h1 {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>🐍 Snake Game</h1>
  <canvas id="gameCanvas" width="400" height="400"></canvas>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    const box = 20;
    const canvasSize = 400;
    const rows = canvasSize / box;
    const columns = canvasSize / box;

    let snake = [{ x: 10 * box, y: 10 * box }];
    let direction = null;
    let food = {
      x: Math.floor(Math.random() * columns) * box,
      y: Math.floor(Math.random() * rows) * box
    };

    let score = 0;

    function draw() {
      // Clear
      ctx.clearRect(0, 0, canvasSize, canvasSize);

      // Draw food
      ctx.fillStyle = "red";
      ctx.fillRect(food.x, food.y, box, box);

      // Draw snake
      for (let i = 0; i < snake.length; i++) {
        ctx.fillStyle = i === 0 ? "lime" : "green";
        ctx.fillRect(snake[i].x, snake[i].y, box, box);
      }

      // Move snake
      let headX = snake[0].x;
      let headY = snake[0].y;

      if (direction === "LEFT") headX -= box;
      if (direction === "RIGHT") headX += box;
      if (direction === "UP") headY -= box;
      if (direction === "DOWN") headY += box;

      // Check collision with walls
      if (
        headX < 0 ||
        headY < 0 ||
        headX >= canvasSize ||
        headY >= canvasSize ||
        collision({ x: headX, y: headY }, snake)
      ) {
        clearInterval(game);
        alert("Game Over! Score: " + score);
        return;
      }

      let newHead = { x: headX, y: headY };

      // Eat food
      if (headX === food.x && headY === food.y) {
        score++;
        food = {
          x: Math.floor(Math.random() * columns) * box,
          y: Math.floor(Math.random() * rows) * box
        };
      } else {
        snake.pop();
      }

      snake.unshift(newHead);
    }

    function collision(head, array) {
      for (let i = 0; i < array.length; i++) {
        if (head.x === array[i].x && head.y === array[i].y) {
          return true;
        }
      }
      return false;
    }

    document.addEventListener("keydown", changeDirection);

    function changeDirection(event) {
      if (event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
      else if (event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
      else if (event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
      else if (event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
    }

    let game = setInterval(draw, 100);
  </script>
</body>
</html>
