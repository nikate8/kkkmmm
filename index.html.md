<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="UTF-8" />  
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>  
<title>Snake Game</title>  
  
<style>  
  body {  
    margin: 0;  
    background: #0f0f0f;  
    color: white;  
    font-family: Arial;  
    display: flex;  
    flex-direction: column;  
    align-items: center;  
    justify-content: center;  
    height: 100vh;  
  }  
  
  canvas {  
    background: #111;  
    border: 2px solid #00ffcc;  
  }  
  
  .ui {  
    margin-top: 10px;  
    text-align: center;  
  }  
  
  button {  
    padding: 8px 12px;  
    background: #00ffcc;  
    border: none;  
    cursor: pointer;  
    font-weight: bold;  
  }  
</style>  
</head>  
  
<body>  
  
<canvas id="game" width="400" height="400"></canvas>  
  
<div class="ui">  
  <button onclick="startGame()">Start</button>  
  <div id="score">Score: 0</div>  
</div>  
  
<script>  
const canvas = document.getElementById("game");  
const ctx = canvas.getContext("2d");  
  
const grid = 20;  
let snake, food, dx, dy, score, gameLoop, running;  
  
function reset() {  
  snake = [{x: 200, y: 200}];  
  dx = grid;  
  dy = 0;  
  score = 0;  
  food = spawnFood();  
  document.getElementById("score").innerText = "Score: 0";  
}  
  
function spawnFood() {  
  return {  
    x: Math.floor(Math.random() * (canvas.width / grid)) * grid,  
    y: Math.floor(Math.random() * (canvas.height / grid)) * grid  
  };  
}  
  
function draw() {  
  ctx.fillStyle = "#111";  
  ctx.fillRect(0,0,canvas.width,canvas.height);  
  
  // food  
  ctx.fillStyle = "#ff0044";  
  ctx.fillRect(food.x, food.y, grid, grid);  
  
  // snake  
  ctx.fillStyle = "#00ffcc";  
  snake.forEach((s, i) => {  
    ctx.fillRect(s.x, s.y, grid, grid);  
  });  
}  
  
function update() {  
  const head = {x: snake[0].x + dx, y: snake[0].y + dy};  
  
  // wall collision  
  if (  
    head.x < 0 || head.y < 0 ||  
    head.x >= canvas.width ||  
    head.y >= canvas.height  
  ) {  
    return gameOver();  
  }  
  
  // self collision  
  for (let part of snake) {  
    if (head.x === part.x && head.y === part.y) {  
      return gameOver();  
    }  
  }  
  
  snake.unshift(head);  
  
  if (head.x === food.x && head.y === food.y) {  
    score++;  
    document.getElementById("score").innerText = "Score: " + score;  
    food = spawnFood();  
  } else {  
    snake.pop();  
  }  
  
  draw();  
}  
  
function gameOver() {  
  clearInterval(gameLoop);  
  running = false;  
  alert("you died. score: " + score);  
}  
  
function startGame() {  
  if (running) return;  
  
  reset();  
  running = true;  
  
  gameLoop = setInterval(update, 120);  
}  
  
// controls  
document.addEventListener("keydown", e => {  
  if (e.key === "ArrowUp" && dy === 0) {  
    dx = 0; dy = -grid;  
  } else if (e.key === "ArrowDown" && dy === 0) {  
    dx = 0; dy = grid;  
  } else if (e.key === "ArrowLeft" && dx === 0) {  
    dx = -grid; dy = 0;  
  } else if (e.key === "ArrowRight" && dx === 0) {  
    dx = grid; dy = 0;  
  }  
});  
</script>  
  
</body>  
</html>  
