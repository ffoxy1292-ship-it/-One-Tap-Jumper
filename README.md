<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>One-Tap Jumper</title>
<style>
  body { margin:0; display:flex; justify-content:center; align-items:center; height:100vh; background:#f0f0f0; overflow:hidden; font-family:sans-serif; }
  #game { position:relative; width:300px; height:500px; background:#fff; border:2px solid #000; overflow:hidden; }
  #player { position:absolute; bottom:50px; left:50px; width:30px; height:30px; background:red; border-radius:50%; }
  .obstacle { position:absolute; width:30px; height:30px; background:black; bottom:50px; right:0; }
  #score { position:absolute; top:5px; left:5px; font-weight:bold; }
</style>
</head>
<body>
<div id="game">
  <div id="player"></div>
  <div id="score">0</div>
</div>

<script>
const game = document.getElementById('game');
const player = document.getElementById('player');
const scoreDisplay = document.getElementById('score');

let gravity = 2;
let jumpForce = 30;
let playerY = 50;
let velocity = 0;
let obstacles = [];
let score = 0;
let gameOver = false;

// Tap / jump
game.addEventListener('click', () => { if(!gameOver) velocity = -jumpForce; });

// Game loop
function loop() {
  if(gameOver) return;

  // Apply gravity
  velocity += gravity;
  playerY -= velocity;
  if(playerY < 0) playerY = 0;
  if(playerY > 470) { gameOver = true; alert('Game Over! Score: ' + score); return; }
  player.style.bottom = playerY + 'px';

  // Create obstacles
  if(Math.random() < 0.02) {
    const obs = document.createElement('div');
    obs.classList.add('obstacle');
    obs.style.right = '0px';
    game.appendChild(obs);
    obstacles.push(obs);
  }

  // Move obstacles
  obstacles.forEach((obs, index) => {
    let right = parseInt(obs.style.right);
    right += 5; // speed
    obs.style.right = right + 'px';

    // Check collision
    const obsX = 300 - right;
    if(obsX < 80 && obsX + 30 > 50 && playerY < 80) {
      gameOver = true;
      alert('Game Over! Score: ' + score);
    }

    // Remove off-screen
    if(right > 300) {
      obs.remove();
      obstacles.splice(index, 1);
      score++;
      scoreDisplay.textContent = score;
    }
  });

  requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html># -One-Tap-Jumper
A simple one-tap endless jumper game built with HTML, CSS, and JavaScript
