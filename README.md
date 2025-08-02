<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Time Ninja Ultimate</title>
  <style>
    body {
      margin: 0;
      font-family: 'Arial', sans-serif;
      background: linear-gradient(to right, #0f2027, #203a43, #2c5364);
      color: white;
      text-align: center;
    }
    .game-screen { display: none; padding: 20px; }
    .start-screen, .game-over-screen { padding: 40px; }
    button {
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      border: none;
      border-radius: 10px;
      background: #4caf50;
      color: white;
      margin: 10px;
    }
    .stats, .hearts, .power-ups { margin-top: 10px; }
    .hearts span { color: red; font-size: 24px; }
    .arrows { margin-top: 30px; font-size: 40px; }
    .night { background: linear-gradient(to right, #000428, #004e92); }
  </style>
</head>
<body>
  <div class="start-screen">
    <h1>Time Ninja Ultimate</h1>
    <button onclick="startGame()">Start Game</button>
    <br>
    <button onclick="toggleTheme()">Toggle Day/Night</button>
  </div>

  <div class="game-screen">
    <h2>Level <span id="level">1</span></h2>
    <div class="stats">
      <div>Score: <span id="score">0</span></div>
      <div>Time Left: <span id="timer">60</span>s</div>
      <div class="hearts" id="health">❤️❤️❤️</div>
    </div>
    <div class="arrows">⬅️ ⬆️ ⬇️ ➡️</div>
    <div class="power-ups" id="powerup">Power-up: None</div>
    <br>
    <button onclick="simulateHit()">Simulate Wrong Hit</button>
  </div>

  <div class="game-over-screen">
    <h1>Game Over!</h1>
    <p>Your Score: <span id="final-score">0</span></p>
    <button onclick="restartGame()">Restart</button>
  </div>

  <script>
    let score = 0, level = 1, timeLeft = 60, lives = 3, theme = 'day';
    const scoreDisplay = document.getElementById("score");
    const levelDisplay = document.getElementById("level");
    const timerDisplay = document.getElementById("timer");
    const healthDisplay = document.getElementById("health");
    const powerupDisplay = document.getElementById("powerup");
    const finalScoreDisplay = document.getElementById("final-score");

    function startGame() {
      document.querySelector(".start-screen").style.display = "none";
      document.querySelector(".game-over-screen").style.display = "none";
      document.querySelector(".game-screen").style.display = "block";
      score = 0; level = 1; timeLeft = 60; lives = 3;
      updateStats();
      countdown();
      activatePowerUps();
    }

    function updateStats() {
      scoreDisplay.textContent = score;
      levelDisplay.textContent = level;
      timerDisplay.textContent = timeLeft;
      healthDisplay.innerHTML = '❤️'.repeat(lives);
    }

    function countdown() {
      let timer = setInterval(() => {
        timeLeft--;
        timerDisplay.textContent = timeLeft;
        if (timeLeft <= 0 || lives <= 0) {
          clearInterval(timer);
          endGame();
        }
      }, 1000);
    }

    function simulateHit() {
      lives--;
      if (lives <= 0) endGame();
      updateStats();
    }

    function endGame() {
      document.querySelector(".game-screen").style.display = "none";
      document.querySelector(".game-over-screen").style.display = "block";
      finalScoreDisplay.textContent = score;
    }

    function restartGame() {
      document.querySelector(".game-over-screen").style.display = "none";
      startGame();
    }

    function toggleTheme() {
      if (theme === 'day') {
        document.body.classList.add('night');
        theme = 'night';
      } else {
        document.body.classList.remove('night');
        theme = 'day';
      }
    }

    function activatePowerUps() {
      setInterval(() => {
        const powers = ["Slow Time", "Double Score", "Invincibility", "None"];
        const selected = powers[Math.floor(Math.random() * powers.length)];
        powerupDisplay.textContent = "Power-up: " + selected;
        if (selected === "Double Score") {
          score += 10;
        } else if (selected === "Invincibility" && lives < 3) {
          lives++;
        } else if (selected === "Slow Time") {
          timeLeft += 5;
        }
        updateStats();
      }, 10000); // every 10 seconds
    }
  </script>
</body>
</html>
