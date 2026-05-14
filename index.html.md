<index.html>  
<html lang="en">  
<head>  
<meta charset="UTF-8" />  
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>  
<title>Neon Snake</title>  
  
<style>  
  body {  
    margin: 0;  
    background: #050505;  
    color: #00ffcc;  
    font-family: Arial;  
    display: flex;  
    flex-direction: column;  
    align-items: center;  
    justify-content: center;  
    height: 100vh;  
    overflow: hidden;  
  }  
  
  canvas {  
    background: #000;  
    border: 2px solid #00ffcc;  
    box-shadow: 0 0 20px #00ffcc, 0 0 40px #00ffcc;  
    display: none;  
  }  
  
  .menu {  
    text-align: center;  
  }  
  
  .title {  
    font-size: 34px;  
    color: #00ffcc;  
    text-shadow: 0 0 10px #00ffcc, 0 0 20px #00ffcc;  
    animation: glow 1.5s infinite alternate;  
  }  
  
  @keyframes glow {  
    from { opacity: 0.6; transform: scale(1); }  
    to { opacity: 1; transform: scale(1.08); }  
  }  
  
  button {  
    padding: 10px 16px;  
    background: #00ffcc;  
    border: none;  
    font-weight: bold;  
    cursor: pointer;  
    margin-top: 10px;  
    box-shadow: 0 0 10px #00ffcc;  
  }  
  
  #score {  
    margin-top: 10px;  
    display: none;  
    text-shadow: 0 0 10px #00ffcc;  
  }  
  
  #controls {  
    position: absolute;  
    bottom: 25px;  
    display: none;  
    text-align: center;  
  }  
  
  #controls button {  
    font-size: 22px;  
    margin: 5px;  
    padding: 12px 16px;  
    border-radius: 10px;  
    background: #00ffcc;  
    box-shadow: 0 0 10px #00ffcc, 0 0 20px #00ffcc;  
    border: none;  
  }  
</style>  
</head>  
  
<body>  
  
<div class="menu" id="menu">  
  <div class="title">NEON SNAKE</div>  
  <button onclick="startGame()">START</button>  
</div>  
  
<canvas id="game" width="400" height="400"></canvas>  
  
<div id="score">Score: 0</div>  
  
<div id="controls">  
  <button onclick="setDir('up')">⬆️</button><br/>  
  <button onclick="setDir('left')">⬅️</button>  
  <button onclick="setDir('down')">⬇️</button>  
  <button onclick="setDir('right')">➡️</button>  
</div>  
  
<script>  
const canvas = document.getElementById("game");  
const ctx = canvas.getContext("2d");  
  
const grid = 20;  
let snake, food, dx, dy, score, loop, running;  
  
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
  ctx.fillStyle = "#000";  
  ctx.fillRect(0,0,canvas.width,canvas.height);  
  
  // neon food  
  ctx.shadowBlur = 20;  
  ctx.shadowColor = "#ff0044";  
  ctx.fillStyle = "#ff0044";  
  ctx.fillRect(food.x, food.y, grid, grid);  
  
  // neon snake  
  ctx.shadowBlur = 25;  
  ctx.shadowColor = "#00ffcc";  
  ctx.fillStyle = "#00ffcc";  
  
  snake.forEach(s => {  
    ctx.fillRect(s.x, s.y, grid, grid);  
  });  
  
  ctx.shadowBlur = 0;  
}  
  
function update() {  
  const head = {x: snake[0].x + dx, y: snake[0].y + dy};  
  
  if (  
    head.x < 0 || head.y < 0 ||  
    head.x >= canvas.width ||  
    head.y >= canvas.height  
  ) return gameOver();  
  
  for (let p of snake) {  
    if (p.x === head.x && p.y === head.y) return gameOver();  
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
  clearInterval(loop);  
  running = false;  
  alert("you died. score: " + score);  
}  
  
function startGame() {  
  if (running) return;  
  
  document.getElementById("menu").style.display = "none";  
  canvas.style.display = "block";  
  document.getElementById("score").style.display = "block";  
  document.getElementById("controls").style.display = "block";  
  
  reset();  
  running = true;  
  loop = setInterval(update, 110);  
}  
  
/* keyboard */  
document.addEventListener("keydown", e => {  
  if (e.key === "ArrowUp" && dy === 0) { dx = 0; dy = -grid; }  
  else if (e.key === "ArrowDown" && dy === 0) { dx = 0; dy = grid; }  
  else if (e.key === "ArrowLeft" && dx === 0) { dx = -grid; dy = 0; }  
  else if (e.key === "ArrowRight" && dx === 0) { dx = grid; dy = 0; }  
});  
  
/* mobile buttons */  
function setDir(dir) {  
  if (dir === "up" && dy === 0) { dx = 0; dy = -grid; }  
  else if (dir === "down" && dy === 0) { dx = 0; dy = grid; }  
  else if (dir === "left" && dx === 0) { dx = -grid; dy = 0; }  
  else if (dir === "right" && dx === 0) { dx = grid; dy = 0; }  
}  
</script>  
  
</body>  
</html>  
