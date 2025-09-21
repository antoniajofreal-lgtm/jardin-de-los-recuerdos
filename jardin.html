<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>🌸 El Jardín de los Recuerdos 🌸</title>
  <style>
    body {
      font-family: "Comic Sans MS", cursive;
      background: #e6f5e9;
      text-align: center;
      margin: 0;
      padding: 0;
    }
    #gameContainer { display: none; padding: 20px; }
    #elements { margin-top: 20px; }
    .item {
      display: inline-block;
      margin: 10px;
      font-size: 60px;
      cursor: pointer;
      transition: transform 0.2s;
    }
    .item:active { transform: scale(1.2); }
    #scoreBoard { font-size: 20px; margin: 10px; }
    #lives span { font-size: 25px; margin: 0 2px; }
    
    /* Contenedor del jardinero y su diálogo */
    #gardenerContainer {
      position: fixed;
      right: 10px;
      bottom: 10px;
      display: flex;
      flex-direction: column;
      align-items: center;
      z-index: 5;
    }
    #gardener {
      width: 120px;
    }
    #speechBubble {
      background: rgba(255,255,255,0.9);
      border: 2px solid #555;
      border-radius: 15px;
      padding: 10px;
      max-width: 240px;
      font-size: 20px;
      margin-bottom: 10px;
    }

    #startScreen, #endWin, #endLose {
      padding: 40px;
    }
    button {
      padding: 10px 20px;
      font-size: 18px;
      border-radius: 10px;
      border: none;
      cursor: pointer;
      background: #7ec850;
      color: white;
      margin: 5px;
    }
    button:hover { background: #6ab042; }
  </style>
</head>
<body>
  <div id="startScreen">
    <h1>🌸 El Jardín de los Recuerdos 🌸</h1>
    <p>Ayuda al jardinero a cuidar su jardín recordando secuencias.</p>
    <button id="startMain">Comenzar Juego</button>
    <button id="toggleMusic">🔇 Música</button>
  </div>

  <div id="gameContainer">
    <div id="scoreBoard">
      Puntos: <span id="score">0</span> &nbsp;&nbsp; Vidas:
      <span id="lives">🌸🌸🌸</span>
      <button id="toggleMusicGame">🔇 Música</button>
    </div>
    <div id="elements"></div>
  </div>

  <div id="endWin" style="display:none;">
    <h2>🎉 ¡Felicidades! 🎉</h2>
    <p>El jardín florece gracias a tu memoria.</p>
    <button onclick="restartGame()">Jugar otra vez</button>
  </div>

  <div id="endLose" style="display:none;">
    <h2>🌱 ¡Ánimo! 🌱</h2>
    <p>Inténtalo de nuevo, el jardín espera por ti.</p>
    <button onclick="restartGame()">Reintentar</button>
  </div>

  <!-- Jardinero con globo -->
  <div id="gardenerContainer">
    <div id="speechBubble">¡Bienvenido al jardín!</div>
    <img id="gardener" src="https://cdn.pixabay.com/photo/2014/04/02/10/57/gardener-303491_1280.png" alt="Jardinero">
  </div>

  <!-- Música de fondo -->
  <audio id="bg-music" src="https://files.freemusicarchive.org/storage-freemusicarchive-org/music/ccCommunity/Jahzzar/Traveller/Jahzzar_-_05_-_Siesta.mp3" loop></audio>

  <!-- Sonidos -->
  <audio id="sound-sequence" src="https://actions.google.com/sounds/v1/cartoon/wood_plank_flicks.ogg"></audio>
  <audio id="sound-correct" src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg"></audio>
  <audio id="sound-error" src="https://actions.google.com/sounds/v1/cartoon/metal_thud_and_clang.ogg"></audio>

  <script>
    const flowers = ['🌹','🌻','🌷','🌼'];
    const fruits = ['🍎','🍐','🍊','🍇'];
    const veggies = ['🥕','🍅','🌽','🍆'];
    const gardenerBubble = document.getElementById("speechBubble");

    let sequence = [];
    let playerSequence = [];
    let score = 0;
    let lives = 3;
    let currentRound = 0;
    let sequencesPerRound = 2;
    let sequenceCount = 0;
    let listening = false;

    const elementsDiv = document.getElementById("elements");
    const scoreSpan = document.getElementById("score");
    const livesSpan = document.getElementById("lives");
    const startScreen = document.getElementById("startScreen");
    const gameContainer = document.getElementById("gameContainer");
    const endWin = document.getElementById("endWin");
    const endLose = document.getElementById("endLose");

    const bgMusic = document.getElementById("bg-music");
    let musicPlaying = false;

    function toggleMusic(){
      if(musicPlaying){
        bgMusic.pause();
        musicPlaying = false;
      } else {
        bgMusic.play().then(()=>{
          musicPlaying = true;
        }).catch(err=>{
          console.log("Error al reproducir música:", err);
        });
      }
    }

    document.getElementById("toggleMusic").addEventListener("click", toggleMusic);
    document.getElementById("toggleMusicGame").addEventListener("click", toggleMusic);

    function setBubble(text, duration=3500){
      gardenerBubble.innerText = text;
    }

    function playSound(id){
      const sound = document.getElementById(id);
      if(sound){
        sound.currentTime = 0;
        sound.play();
      }
    }

    function updateScore(){ scoreSpan.textContent = score; }
    function updateLives(){
      livesSpan.textContent = "🌸".repeat(lives);
    }

    function startGame(){
      startScreen.style.display = "none";
      gameContainer.style.display = "block";
      score = 0; lives = 3; currentRound = 0; sequenceCount = 0;
      updateScore(); updateLives();
      if(!musicPlaying){ toggleMusic(); }
      startRound();
    }

    function restartGame(){
      endWin.style.display = "none";
      endLose.style.display = "none";
      gameContainer.style.display = "block";
      score = 0; lives=3; currentRound=0; sequenceCount=0;
      updateScore(); updateLives();
      startRound();
    }

    function startRound(){
      currentRound++;
      sequenceCount = 0;
      setBubble("Ronda "+currentRound+" comienza...", 3500);
      setTimeout(()=> playSequence(), 3500);
    }

    function playSequence(){
      sequence = [];
      playerSequence = [];
      let pool = currentRound===1 ? flowers : (currentRound===2 ? fruits : veggies.concat(fruits));
      let length = currentRound + 1;
      for(let i=0; i<length; i++){
        sequence.push(pool[Math.floor(Math.random()*pool.length)]);
      }
      elementsDiv.innerHTML="";
      pool.forEach((item, idx)=>{
        let div = document.createElement("div");
        div.textContent = item;
        div.className="item";
        div.dataset.value = item;
        div.onclick = ()=> handleClick(item, idx);
        elementsDiv.appendChild(div);
      });
      showSequence();
    }

    async function showSequence(){
      setBubble("Observa la secuencia...", 3500);
      await new Promise(r=>setTimeout(r, 3500));
      for(let i=0; i<sequence.length; i++){
        const el = Array.from(elementsDiv.children).find(c=>c.textContent===sequence[i]);
        if(el){
          el.style.transform="scale(1.5)";
          playSound("sound-sequence");
          await new Promise(r=>setTimeout(r, 800));
          el.style.transform="scale(1)";
          await new Promise(r=>setTimeout(r, 400));
        }
      }
      setTimeout(()=>{
        listening=true;
        setBubble("¡Tu turno! Repite la secuencia", 3500);
      }, 1000);
    }

    function handleClick(item){
      if(!listening) return;
      playerSequence.push(item);
      let idx = playerSequence.length-1;
      if(playerSequence[idx]!==sequence[idx]){
        lives--;
        updateLives();
        setBubble("¡Oh! Te equivocaste.", 3500);
        playSound("sound-error");
        listening=false;
        if(lives<=0){ endLoseGame(); return; }
        else { setTimeout(()=> playSequence(), 3500); }
        return;
      }
      if(playerSequence.length===sequence.length){
        score+=10;
        updateScore();
        setBubble("¡Muy bien! 🌟", 3500);
        playSound("sound-correct");
        listening=false;
        sequenceCount++;
        if(sequenceCount < sequencesPerRound){
          setTimeout(()=> playSequence(), 3500);
        } else {
          if(currentRound>=3){ endWinGame(); }
          else { setTimeout(()=> startRound(), 3500); }
        }
      }
    }

    function endWinGame(){
      gameContainer.style.display="none";
      endWin.style.display="block";
      setBubble("¡Tu jardín florece! 🌸", 4000);
      playSound("sound-correct");
    }

    function endLoseGame(){
      gameContainer.style.display="none";
      endLose.style.display="block";
      setBubble("El jardín te espera para intentarlo de nuevo 🌱", 4000);
      playSound("sound-error");
    }

    document.getElementById("startMain").addEventListener("click", startGame);
  </script>
</body>
</html>
