<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" />
  <title>El Jardín de los Recuerdos</title>
  <style>
    html, body { height:100%; margin:0; padding:0; box-sizing:border-box; background:#f0f8f5; font-family: system-ui, Arial, sans-serif; overflow:hidden; }
    #start-screen {
      position:absolute; inset:0;
      background: url("https://cdn.pixabay.com/photo/2016/11/29/12/54/cartoon-1869538_1280.jpg") center/cover no-repeat;
      display:flex; flex-direction:column; align-items:center; justify-content:center; color:#fff; text-shadow:2px 2px 5px rgba(0,0,0,0.6); z-index:30;
    }
    #start-screen h1 { font-size:2.6rem; margin-bottom:20px; }
    #start-screen .btn { padding:15px 30px; font-size:1.3rem; background:#ff80bf; border:none; border-radius:16px; cursor:pointer; touch-action: manipulation; }
    #start-screen .btn:active { transform:scale(0.98); }

    #game-container { display:none; position:relative; width:100%; height:100%; padding:20px; box-sizing:border-box; overflow:auto; }
    #score, #lives { font-size:1.2rem; margin:10px; }
    #game-board { display:grid; grid-template-columns: repeat(2, minmax(120px,150px)); gap:18px; justify-content:center; margin-top:20px; margin-bottom:140px; }

    .cell { width:150px; height:150px; border-radius:15px; background:#fff; display:flex; align-items:center; justify-content:center; box-shadow:0 5px 15px rgba(0,0,0,0.1); cursor:pointer; touch-action: manipulation; }
    .cell img { width:90%; height:90%; object-fit:contain; pointer-events:none; }
    .highlight { background:#c6f6d5 !important; }

    #gardener-container { position:absolute; bottom:10px; left:20px; display:flex; align-items:flex-end; gap:10px; pointer-events:none; }
    #gardener-container img { width:110px; }
    #speech-bubble {
      max-width:250px; background:#fff; border:2px solid #333; border-radius:14px; padding:10px;
      font-size:1rem; position:relative; box-shadow:0 5px 18px rgba(0,0,0,0.12);
    }
    #speech-bubble:after { content:''; position:absolute; bottom:-20px; left:30px; border-left:20px solid transparent; border-right:20px solid transparent; border-top:20px solid #fff; }

    .end-screen { display:none; position:absolute; inset:0; background:rgba(255,255,255,0.95); flex-direction:column; justify-content:center; align-items:center; text-align:center; z-index:40; }
    .end-screen h2 { font-size:2rem; margin-bottom:20px; color:#2d572c; }
    .end-screen img { width:160px; margin-bottom:15px; }
    .end-screen button { padding:12px 22px; font-size:1.15rem; background:#77c977; border:none; border-radius:12px; color:#fff; cursor:pointer; touch-action: manipulation; }

  </style>
</head>
<body>
  <div id="start-screen">
    <h1>🌳 El Jardín de los Recuerdos 🌳</h1>
    <button id="startMain" class="btn">🌸 Comenzar Juego 🌸</button>
  </div>

  <div id="game-container">
    <div id="score">Puntos: 0</div>
    <div id="lives">Vidas: 🌸🌸🌸</div>
    <div id="game-board"></div>

    <div id="gardener-container">
      <img id="gardener-img" src="https://clker.com/cliparts/a/7/a/1/1516253069442174405cartoon-gardener-clipart.hi.png" alt="Jardinero dominio público">
      <div id="speech-bubble">¡Bienvenido al jardín! 🌼</div>
    </div>
  </div>

  <div id="end-screen-win" class="end-screen">
    <h2>🌟 ¡Gracias por jugar! 🌟</h2>
    <img src="https://clker.com/cliparts/a/7/a/1/1516253069442174405cartoon-gardener-clipart.hi.png" alt="Jardinero feliz">
    <p>Siempre eres bienvenido en el Jardín de los Recuerdos 🌼</p>
    <button onclick="restartGame()">🔄 Volver a jugar</button>
  </div>

  <div id="end-screen-lose" class="end-screen">
    <h2>💔 ¡Ánimo! 💔</h2>
    <img src="https://clker.com/cliparts/a/7/a/1/1516253069442174405cartoon-gardener-clipart.hi.png" alt="Jardinero preocupado">
    <p>¡Inténtalo otra vez, tú puedes! 💪🌱</p>
    <button onclick="restartGame()">🔄 Volver a intentar</button>
  </div>

  <script>
    const startMain = document.getElementById("startMain");
    const startScreen = document.getElementById("start-screen");
    const gameContainer = document.getElementById("game-container");
    const board = document.getElementById("game-board");
    const scoreDisplay = document.getElementById("score");
    const livesDisplay = document.getElementById("lives");
    const speechBubble = document.getElementById("speech-bubble");
    const endWin = document.getElementById("end-screen-win");
    const endLose = document.getElementById("end-screen-lose");

    let score=0, lives=3, currentRound=0, listening=false, playerIndex=0;
    const sequencesPerRound = 2;
    let sequenceCount = 0;
    let sequence = [];

    const rounds = [
      {
        name: "Regar las Flores",
        message: "¡Hora de regar las flores! 🌸💧",
        icons: [
          "https://freesvg.org/img/apple-fruit-icon.svg",
          "https://freesvg.org/img/banana-fruit-icon.svg",
          "https://freesvg.org/img/strawberry-fruit-icon.svg",
          "https://freesvg.org/img/lemon-fruit-icon.svg"
        ]
      },
      {
        name: "Cosechar Frutas",
        message: "¡Vamos a cosechar frutas frescas! 🍎🍌",
        icons: [
          "https://freesvg.org/img/apple-fruit-icon.svg",
          "https://freesvg.org/img/grapes-fruit-icon.svg",
          "https://freesvg.org/img/orange-fruit-icon.svg",
          "https://freesvg.org/img/pear-fruit-icon.svg"
        ]
      },
      {
        name: "Juntando la Cosecha",
        message: "¡Frutas y verduras unidas! 🥕🍎",
        icons: [
          "https://freesvg.org/img/carrot-vegetable-icon.svg",
          "https://freesvg.org/img/apple-fruit-icon.svg",
          "https://freesvg.org/img/banana-fruit-icon.svg",
          "https://freesvg.org/img/corn-vegetable-icon.svg"
        ]
      }
    ];

    // Sonidos
    const soundUrls = {
      waterDrop: "https://freesound.org/data/previews/165/165206_1631209-lq.wav",
      success: "https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg",
      error: "https://actions.google.com/sounds/v1/cartoon/wood_plank_flicks.ogg"
    };
    const audioCache = {};

    function playSound(name){
      let url = soundUrls[name];
      if(!url) return;
      if(!audioCache[name]){
        let a = new Audio(url);
        audioCache[name] = a;
      }
      audioCache[name].currentTime = 0;
      audioCache[name].play().catch(e=>{});
    }

    function startGame(){
      startScreen.style.display = "none";
      gameContainer.style.display = "block";
      score = 0; lives=3; currentRound=0;
      updateScore(); updateLives();
      startRound();
    }

    function startRound(){
      speechBubble.textContent = rounds[currentRound].message;
      buildBoard(rounds[currentRound].icons);
      sequenceCount = 0;
      startNewSequence();
    }

    function buildBoard(iconUrls){
      board.innerHTML = "";
      iconUrls.forEach((url, idx)=>{
        const btn = document.createElement("button");
        btn.classList.add("cell");
        const img = document.createElement("img");
        img.src = url;
        img.onerror = ()=>{ img.src = "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='96' height='96'><rect width='100%' height='100%' fill='%23fff'/><text x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' font-size='48'>❓</text></svg>";};
        img.alt="";
        btn.appendChild(img);
        btn.addEventListener("click", ()=>onCellClick(idx));
        board.appendChild(btn);
      });
    }

    function startNewSequence(){
      const len = 2 + currentRound + sequenceCount;
      sequence = generateSequence(len);
      playerIndex=0;
      listening = false;
      setTimeout(()=> showSequence(sequence), 700);
    }

    function generateSequence(len){
      const arr=[];
      for(let i=0;i<len;i++){
        arr.push(Math.floor(Math.random()*rounds[currentRound].icons.length));
      }
      return arr;
    }

    function showSequence(seq){
      const cells = board.querySelectorAll(".cell");
      let i=0;
      const interval = setInterval(()=>{
        if(cells[seq[i]]){
          cells[seq[i]].classList.add("highlight");
          setTimeout(()=>cells[seq[i]].classList.remove("highlight"),500);
          playSound("waterDrop");
        }
        i++;
        if(i>=seq.length){
          clearInterval(interval);
          setTimeout(()=>{ listening = true; speechBubble.textContent = "¡Tu turno! Repite la secuencia"; }, 500);
        }
      }, 900);
    }

    function onCellClick(index){
      if(!listening) return;
      if(index !== sequence[playerIndex]){
        playSound("error");
        lives--;
        updateLives();
        speechBubble.textContent = "¡Ups! Esa no es la correcta.";
        listening = false;
        if(lives <= 0){
          setTimeout(()=> showLose(), 800);
        } else {
          setTimeout(()=> showSequence(sequence), 900);
        }
        return;
      }
      playSound("success");
      playerIndex++;
      score += 10;
      updateScore();
      let cells = board.querySelectorAll(".cell");
      if(cells[index]){
        cells[index].classList.add("highlight");
        setTimeout(()=>cells[index].classList.remove("highlight"),300);
      }
      if(playerIndex >= sequence.length){
        sequenceCount++;
        score += 20;
        updateScore();
        if(sequenceCount < sequencesPerRound){
          speechBubble.textContent = "¡Muy bien! Ahora otra secuencia.";
          listening = false;
          setTimeout(()=> startNewSequence(), 1000);
        } else {
          currentRound++;
          if(currentRound >= rounds.length){
            setTimeout(()=> showWin(), 900);
          } else {
            speechBubble.textContent = "¡Ronda completada! 🎉";
            listening = false;
            setTimeout(()=> startRound(), 1200);
          }
        }
      }
    }

    function updateScore(){
      scoreDisplay.textContent = "Puntos: " + score;
    }
    function updateLives(){
      livesDisplay.textContent = "Vidas: " + "🌸".repeat(lives);
    }
    function showWin(){
      gameContainer.style.display = "none";
      endWin.style.display = "flex";
    }
    function showLose(){
      gameContainer.style.display = "none";
      endLose.style.display = "flex";
    }
    function restartGame(){
      endWin.style.display = "none";
      endLose.style.display = "none";
      gameContainer.style.display = "block";
      score = 0; lives=3; currentRound=0;
      updateScore(); updateLives();
      startRound();
    }

    // 🔥 Conectar botón de inicio
    startMain.addEventListener("click", startGame);
  </script>
</body>
</html>
