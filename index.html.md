<index.html>  
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
    align-items: center;  
    justify-content: center;  
    height: 100vh;  
    overflow: hidden;  
  }  
  
  canvas {  
    background: #111;  
    border: 2px solid #00ffcc;  
    display: none;  
  }  
  
  .menu {  
    text-align: center;  
  }  
  
  .title {  
    font-size: 32px;  
    margin-bottom: 10px;  
    color: #00ffcc;  
    animation: pulse 1.2s infinite;  
  }  
  
  @keyframes pulse {  
    0% { transform: scale(1); opacity: 1; }  
    50% { transform: scale(1.1); opacity: 0.6; }  
    100% { transform: scale(1); opacity: 1; }  
  }  
  
  button {  
    padding: 10px 16px;  
    background: #00ffcc;  
    border: none;  
    cursor: pointer;  
    font-weight: bold;  
  }  
  
  #score {  
    margin-top: 10px;  
    display: none;  
  }  
</style>  
</head>  
  
<body>  
  
<div class="menu" id="menu">  
  <div class="title">SNAKE</div>  
  <button onclick="startGame()">Press Start</button>  
</div>  
  
<canvas id="game" width="400" height="400"></canvas>  
  
<div id="score">Score: 0</div>  
  
<script>  
const canvas = document.getElementById("game");  
const ctx = canvas.getContext("2d");  
  
const grid = 20;  
let snake, food, dx, dy, score, loop, running;  
  
const AudioCtx = window.AudioContext || window.webkitAudioContext;  
const audioCtx = new AudioCtx();  
  
function beep(freq, time) {  
  const osc = audioCtx.createOscillator();  
  const gain = audioCtx.createGain();  
  
  osc.connect(gain);  
  gain.connect(audioCtx.destination);  
  
  osc.frequency.value = freq;  
  osc.type = "square";  
  
  gain.gain.setValueAtTime(0.08, audioCtx.currentTime);  
  gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + time);  
  
  osc.start();  
  osc.stop(audioCtx.currentTime + time);  
}  
  
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
  
  ctx.fillStyle = "#ff0044";  
  ctx.fillRect(food.x, food.y, grid, grid);  
  
  ctx.fillStyle = "#00ffcc";  
  snake.forEach(s => ctx.fillRect(s.x, s.y, grid, grid));  
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
    beep(600, 0.08);  
    food = spawnFood();  
  } else {  
    snake.pop();  
  }  
  
  draw();  
}  
  
function gameOver() {  
  clearInterval(loop);  
  running = false;  
  beep(120, 0.3);  
  alert("you died. score: " + score);  
}  
  
function startGame() {  
  if (running) return;  
  
  if (audioCtx.state === "suspended") {  
    audioCtx.resume();  
  }  
  
  // intro transition  
  beep(300, 0.1);  
  setTimeout(() => beep(500, 0.1), 120);  
  setTimeout(() => beep(800, 0.1), 240);  
  
  document.getElementById("menu").style.display = "none";  
  canvas.style.display = "block";  
  document.getElementById("score").style.display = "block";  
  
  reset();  
  running = true;  
  
  loop = setInterval(update, 110);  
}  
  
document.addEventListener("keydown", e => {  
  if (e.key === "ArrowUp" && dy === 0) { dx = 0; dy = -grid; }  
  else if (e.key === "ArrowDown" && dy === 0) { dx = 0; dy = grid; }  
  else if (e.key === "ArrowLeft" && dx === 0) { dx = -grid; dy = 0; }  
  else if (e.key === "ArrowRight" && dx === 0) { dx = grid; dy = 0; }  
});  
</script>  
  
</body>  
</html>  
