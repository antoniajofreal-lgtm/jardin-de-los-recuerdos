<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>El Jardín de los Recuerdos</title>
  <style>
    body {
      margin: 0;
      font-family: "Comic Sans MS", cursive, sans-serif;
      background-color: #f0f8f5;
      overflow: hidden;
    }

    /* Pantalla de inicio */
    #start-screen {
      position: absolute;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: url("https://cdn.pixabay.com/photo/2016/11/29/12/54/cartoon-1869538_1280.jpg") no-repeat center center/cover;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      z-index: 2;
    }
    #start-screen h1 {
      font-size: 3em;
      color: #fff;
      text-shadow: 2px 2px 4px #000;
      margin-bottom: 20px;
    }
    #start-screen button {
      font-size: 1.5em;
      padding: 15px 30px;
      border: none;
      border-radius: 12px;
      background-color: #ff77aa;
      color: white;
      cursor: pointer;
      transition: 0.3s;
    }
    #start-screen button:hover {
      background-color: #ff4f88;
    }

    /* Contenedor del juego */
    #game-container {
      display: none;
      padding: 20px;
      text-align: center;
      position: relative;
      height: 100vh;
      box-sizing: border-box;
    }

    #score, #lives {
      font-size: 1.2em;
      margin: 10px;
    }

    #game-board {
      display: grid;
      grid-template-columns: repeat(2, 150px);
      grid-gap: 20px;
      justify-content: center;
      margin-top: 20px;
      margin-bottom: 160px; /* espacio para el jardinero */
    }

    button.cell {
      width: 150px;
      height: 150px;
      border: none;
      border-radius: 15px;
      background-color: #fff;
      box-shadow: 2px 2px 5px rgba(0,0,0,0.2);
      cursor: pointer;
      touch-action: manipulation;
    }
    button.cell img {
      width: 100%;
      height: 100%;
      object-fit: contain;
      pointer-events: none;
    }
    .highlight {
      background-color: #c6f6d5 !important;
    }

    /* Jardinero */
    #gardener-container {
      position: absolute;
      bottom: 10px;
      left: 20px;
      display: flex;
      align-items: flex-end;
      gap: 10px;
    }
    #gardener-container img {
      width: 120px;
    }
    #speech-bubble {
      max-width: 250px;
      background: #fff;
      border: 2px solid #333;
      border-radius: 15px;
      padding: 10px;
      font-size: 1em;
      position: relative;
    }
    #speech-bubble:after {
      content: '';
      position: absolute;
      bottom: -20px; left: 30px;
      border-width: 20px 20px 0;
      border-style: solid;
      border-color: #fff transparent;
      display: block;
      width: 0;
    }

    /* Pantallas finales */
    .end-screen {
      display: none;
      position: absolute;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background-color: rgba(255,255,255,0.95);
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      z-index: 3;
    }
    .end-screen h2 {
      font-size: 2.5em;
      color: #333;
      margin-bottom: 20px;
    }
    .end-screen img {
      width: 180px;
      margin-bottom: 20px;
    }
    .end-screen button {
      font-size: 1.2em;
      padding: 10px 20px;
      border: none;
      border-radius: 10px;
      background-color: #77c977;
      color: white;
      cursor: pointer;
      transition: 0.3s;
    }
    .end-screen button:hover {
      background-color: #55aa55;
    }
  </style>
</head>
<body>
  <!-- Pantalla de inicio -->
  <div id="start-screen">
    <h1>🌳 El Jardín de los Recuerdos 🌳</h1>
    <button onclick="startGame()">🌸 Comenzar Juego 🌸</button>
  </div>

  <!-- Contenedor del juego -->
  <div id="game-container">
    <div id="score">Puntos: 0</div>
    <div id="lives">Vidas: 🌸🌸🌸</div>
    <div id="game-board"></div>

    <div id="gardener-container">
      <img id="gardener-img" src="https://cdn.pixabay.com/photo/2017/01/31/17/44/gardener-2025449_1280.png" alt="Jardinero">
      <div id="speech-bubble">¡Bienvenido al jardín! 🌼</div>
    </div>
  </div>

  <!-- Final feliz -->
  <div id="end-screen-win" class="end-screen">
    <h2>🌟 ¡Gracias por jugar! 🌟</h2>
    <img src="https://cdn.pixabay.com/photo/2017/01/31/17/44/gardener-2025449_1280.png" alt="Jardinero feliz">
    <p>Siempre eres bienvenido en el Jardín de los Recuerdos 🌼</p>
    <button onclick="restartGame()">🔄 Volver a jugar</button>
  </div>

  <!-- Final de ánimo -->
  <div id="end-screen-lose" class="end-screen">
    <h2>💔 ¡Ánimo! 💔</h2>
    <img src="https://cdn.pixabay.com/photo/2014/04/02/14/12/man-306695_1280.png" alt="Jardinero preocupado">
    <p>¡Inténtalo otra vez, tú puedes! 💪🌱</p>
    <button onclick="restartGame()">🔄 Volver a intentar</button>
  </div>

  <script>
    // Elementos principales
    const startScreen = document.getElementById("start-screen");
    const gameContainer = document.getElementById("game-container");
    const board = document.getElementById("game-board");
    const scoreDisplay = document.getElementById("score");
    const livesDisplay = document.getElementById("lives");
    const speechBubble = document.getElementById("speech-bubble");
    const endScreenWin = document.getElementById("end-screen-win");
    const endScreenLose = document.getElementById("end-screen-lose");

    let sequence = [];
    let playerIndex = 0;
    let score = 0;
    let lives = 3;
    let listening = false;

    const sequencesPerRound = 2; // 🔥 2 secuencias por ronda
    let sequenceCount = 0;

    const rounds = [
      {
        name: "Regar las Flores",
        message: "¡Hora de regar las flores! 🌸💧",
        icons: [
          "https://img.icons8.com/color/96/flower.png",
          "https://img.icons8.com/color/96/sunflower.png",
          "https://img.icons8.com/color/96/hibiscus.png",
          "https://img.icons8.com/color/96/cherry-blossom.png"
        ]
      },
      {
        name: "Cosechar Frutas",
        message: "¡Vamos a cosechar frutas frescas! 🍎🍌",
        icons: [
          "https://img.icons8.com/color/96/apple.png",
          "https://img.icons8.com/color/96/banana.png",
          "https://img.icons8.com/color/96/pear.png",
          "https://img.icons8.com/color/96/grapes.png"
        ]
      },
      {
        name: "Juntando la Cosecha",
        message: "¡Es hora de juntar frutas y verduras! 🥕🍎",
        icons: [
          "https://img.icons8.com/color/96/apple.png",
          "https://img.icons8.com/color/96/carrot.png",
          "https://img.icons8.com/color/96/banana.png",
          "https://img.icons8.com/color/96/corn.png"
        ]
      }
    ];
    let currentRound = 0;

    function startGame() {
      startScreen.style.display = "none";
      gameContainer.style.display = "block";
      startRound();
    }

    function startRound() {
      speechBubble.textContent = rounds[currentRound].message;
      buildBoard(rounds[currentRound].icons);
      sequenceCount = 0;
      startNewSequence();
    }

    function buildBoard(icons) {
      board.innerHTML = "";
      icons.forEach((icon, index) => {
        const btn = document.createElement("button");
        btn.classList.add("cell");
        const img = document.createElement("img");
        img.src = icon;
        img.alt = "Elemento";
        btn.appendChild(img);
        btn.addEventListener("click", () => onCellClick(index));
        board.appendChild(btn);
      });
    }

    function startNewSequence() {
      const seqLen = 2 + currentRound + sequenceCount;
      sequence = generateSequence(seqLen);
      playerIndex = 0;
      setTimeout(() => showSequence(sequence), 700);
    }

    function generateSequence(len) {
      const seq = [];
      for (let i = 0; i < len; i++) {
        seq.push(Math.floor(Math.random() * rounds[currentRound].icons.length));
      }
      return seq;
    }

    function showSequence(seq) {
      const cells = board.querySelectorAll(".cell");
      let i = 0;
      listening = false;
      const interval = setInterval(() => {
        const cell = cells[seq[i]];
        cell.classList.add("highlight");
        setTimeout(() => cell.classList.remove("highlight"), 500);
        i++;
        if (i >= seq.length) {
          clearInterval(interval);
          setTimeout(() => { listening = true; }, 500);
        }
      }, 1000);
    }

    function onCellClick(index) {
      if (!listening) return;
      if (index !== sequence[playerIndex]) {
        lives--;
        updateLives();
        speechBubble.textContent = "¡Ups! Te has equivocado. 😅";
        listening = false;
        if (lives === 0) {
          setTimeout(() => showLose(), 700);
        } else {
          playerIndex = 0;
          setTimeout(() => showSequence(sequence), 900);
        }
        return;
      }

      playerIndex++;
      score += 10;
      scoreDisplay.textContent = "Puntos: " + score;

      if (playerIndex >= sequence.length) {
        score += 20;
        scoreDisplay.textContent = "Puntos: " + score;
        speechBubble.textContent = "¡Muy bien! 🌟";
        listening = false;
        sequenceCount++;
        if (sequenceCount < sequencesPerRound) {
          setTimeout(() => startNewSequence(), 1000);
        } else {
          currentRound++;
          if (currentRound >= rounds.length) {
            setTimeout(() => showWin(), 900);
          } else {
            speechBubble.textContent = "¡Ronda completada! 🎉";
            setTimeout(() => startRound(), 1200);
          }
        }
      }
    }

    function updateLives() {
      livesDisplay.textContent = "Vidas: " + "🌸".repeat(lives);
    }

    function showWin() {
      gameContainer.style.display = "none";
      endScreenWin.style.display = "flex";
    }

    function showLose() {
      gameContainer.style.display = "none";
      endScreenLose.style.display = "flex";
    }

    function restartGame() {
      endScreenWin.style.display = "none";
      endScreenLose.style.display = "none";
      gameContainer.style.display = "block";
      score = 0;
      lives = 3;
      currentRound = 0;
      scoreDisplay.textContent = "Puntos: 0";
      updateLives();
      startRound();
    }
  </script>
</body>
</html>
