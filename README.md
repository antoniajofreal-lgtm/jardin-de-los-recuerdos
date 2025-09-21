<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" />
  <title>El Jardín de los Recuerdos</title>
  <style>
    /* --- Global --- */
    html,body{height:100%; margin:0; padding:0; box-sizing:border-box; -webkit-font-smoothing:antialiased;}
    body{
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background:#f0f8f5;
      overflow:hidden;
      -webkit-user-select:none; user-select:none;
      -webkit-touch-callout:none;
    }

    /* --- Start screen (portada) --- */
    #start-screen{
      position:fixed; inset:0;
      background: url("https://cdn.pixabay.com/photo/2016/11/29/12/54/cartoon-1869538_1280.jpg") center/cover no-repeat;
      display:flex; flex-direction:column; align-items:center; justify-content:center;
      color:#fff; text-align:center; z-index:30;
    }
    #start-screen h1{ font-size:2.6rem; margin:0 16px 12px; text-shadow: 2px 2px 8px rgba(0,0,0,0.6); }
    #start-screen .hero-img{ width:180px; max-width:40%; margin-bottom:12px; }
    #start-screen .btn{
      background:#ff80bf; color:#fff; border:0; padding:14px 28px; border-radius:18px; font-size:1.15rem;
      box-shadow:0 6px 16px rgba(0,0,0,0.25); cursor:pointer; touch-action: manipulation;
    }
    #start-screen .btn:active{ transform:scale(.98); }

    /* --- Game container --- */
    #game-container{ display:none; position:relative; width:100%; height:100%; padding:18px; box-sizing:border-box; overflow:auto; }

    header.appbar{ display:flex; align-items:center; gap:12px; }
    header.appbar h2{ margin:0; font-size:1.4rem; color:#2d572c; }
    .hud{ display:flex; gap:16px; align-items:center; margin-top:12px; flex-wrap:wrap; justify-content:space-between; }
    .left-hud{ display:flex; gap:12px; align-items:center; }

    .score{ font-weight:700; font-size:1.25rem; color:#155724; }
    .lives{ font-size:1.2rem; color:#7c3a3a; display:flex; gap:6px; align-items:center; }
    .flower{ font-size:1.6rem; transition:opacity .25s ease, transform .25s; }
    .flower.dead{ opacity:.28; transform:scale(.9); filter:grayscale(100%); }

    /* board */
    #boardWrap{ margin-top:14px; display:flex; justify-content:center; }
    #board{
      display:grid;
      grid-template-columns: repeat(2, minmax(120px,160px));
      gap:22px;
      justify-content:center;
      padding:12px;
    }
    .cell{
      width:160px; height:160px; border-radius:18px; background:#fff; display:flex; align-items:center; justify-content:center;
      box-shadow:0 6px 18px rgba(0,0,0,.08); border: 2px solid rgba(0,0,0,0.02);
      touch-action: manipulation; cursor:pointer;
    }
    .cell img{ width:86%; height:86%; object-fit:contain; user-drag:none; -webkit-user-drag:none; }

    .cell:active{ transform:scale(.96); }
    .cell.highlight{ background:#c6f6d5; }

    /* gardener & speech bubble */
    #gardener{
      position:fixed; left:12px; bottom:12px; display:flex; gap:10px; align-items:flex-end; z-index:20;
      pointer-events:none; /* gardener is visual only */
    }
    #gardener img{ width:110px; height:auto; display:block; border-radius:8px; }
    #bubble{
      max-width:260px; background:#fff; border:2px solid #333; border-radius:12px; padding:10px; font-size:1rem;
      box-shadow:0 6px 18px rgba(0,0,0,0.12); color:#0a0a0a;
      position:relative;
    }
    #bubble:after{
      content:""; position:absolute; left:28px; bottom:-18px; width:0;height:0;
      border-left:14px solid transparent; border-right:14px solid transparent; border-top:18px solid #fff;
    }
    #bubble[aria-live]{ outline: none; }

    /* controls */
    #controls{ display:flex; gap:10px; justify-content:center; margin-top:14px; flex-wrap:wrap; }
    .bigBtn{ padding:12px 22px; font-size:1.05rem; border-radius:12px; border:0; cursor:pointer; box-shadow:0 6px 18px rgba(0,0,0,0.10); touch-action:manipulation; }
    .primary{ background:#4caf50; color:white; }
    .secondary{ background:#f06292; color:white; }

    /* end screens overlay */
    .end-overlay{
      position:fixed; inset:0; display:none; align-items:center; justify-content:center; z-index:40;
      background: linear-gradient(0deg, rgba(255,255,255,0.95), rgba(255,255,255,0.95));
      flex-direction:column; text-align:center; padding:18px;
    }
    .end-overlay h1{ margin:0 0 10px; font-size:2rem; color:#2d572c; }
    .end-overlay img{ width:160px; margin-bottom:10px; }
    .end-overlay .btn{ padding:12px 20px; font-size:1.05rem; border-radius:10px; border:0; background:#77c977; color:#fff; cursor:pointer; }

    /* responsive adjustments */
    @media (max-width:520px){
      .cell{ width:130px; height:130px; }
      #board{ grid-template-columns: repeat(2, minmax(110px,1fr)); gap:16px; }
      #gardener img{ width:92px; }
      #bubble{ max-width:200px; font-size:.95rem; }
    }
  </style>
</head>
<body>
  <!-- START -->
  <div id="start-screen" role="dialog" aria-modal="true">
    <h1>🌳 El Jardín de los Recuerdos 🌳</h1>
    <img class="hero-img" src="https://cdn.pixabay.com/photo/2016/03/31/19/56/gardener-1297390_1280.png" alt="Jardinero ilustrado">
    <div style="height:8px"></div>
    <button id="startMain" class="btn" aria-label="Comenzar juego">🌸 Comenzar Juego 🌸</button>
  </div>

  <!-- GAME -->
  <div id="game-container" aria-live="polite">
    <header class="appbar">
      <h2>El Jardín de los Recuerdos</h2>
    </header>

    <div class="hud">
      <div class="left-hud">
        <div class="score" id="score">Puntos: 0</div>
        <div class="lives" id="lives" aria-live="polite">Flores:
          <span class="flower" id="life1">🌸</span>
          <span class="flower" id="life2">🌸</span>
          <span class="flower" id="life3">🌸</span>
        </div>
      </div>
      <div id="roundName" class="small">Ronda: -</div>
    </div>

    <div id="boardWrap">
      <div id="board" role="grid" aria-label="Jardín interactivo"></div>
    </div>

    <div id="controls">
      <button id="playBtn" class="bigBtn primary">Iniciar partida</button>
      <button id="menuBtn" class="bigBtn secondary">Volver al menú</button>
    </div>
  </div>

  <!-- gardener + bubble -->
  <div id="gardener" aria-hidden="false">
    <img id="gardenerImg" src="https://cdn.pixabay.com/photo/2017/01/31/17/44/gardener-2025449_1280.png" alt="Jardinero">
    <div id="bubble" aria-live="polite">¡Hola! Presiona Iniciar para comenzar.</div>
  </div>

  <!-- WIN overlay -->
  <div id="end-win" class="end-overlay" role="dialog" aria-modal="true">
    <h1>🌟 ¡Gracias por jugar! 🌟</h1>
    <img src="https://cdn.pixabay.com/photo/2017/01/31/17/44/gardener-2025449_1280.png" alt="Jardinero feliz">
    <p>Siempre eres bienvenido en el Jardín de los Recuerdos 🌼</p>
    <div style="height:10px"></div>
    <button class="btn" onclick="restartFromEnd(true)">🔄 Volver a jugar</button>
  </div>

  <!-- LOSE overlay -->
  <div id="end-lose" class="end-overlay" role="dialog" aria-modal="true">
    <h1>💔 ¡Ánimo! 💔</h1>
    <img src="https://cdn.pixabay.com/photo/2014/04/02/14/12/man-306695_1280.png" alt="Jardinero preocupado">
    <p>¡Inténtalo otra vez, tú puedes! 💪🌱</p>
    <div style="height:10px"></div>
    <button class="btn" onclick="restartFromEnd(false)">🔄 Volver a intentar</button>
  </div>

<script>
/* ========= Juego (revisado y corregido) ========= */

/* --- Audio: WebAudio helper (resume on gesture) --- */
let audioCtx = null;
function ensureAudioCtx(){
  if(!audioCtx){
    try{ audioCtx = new (window.AudioContext || window.webkitAudioContext)(); }
    catch(e){ audioCtx = null; }
  }
  return audioCtx;
}
function playTone({freq=440, type='sine', dur=0.18, vol=0.07, attack=0.006, release=0.06} = {}){
  if(!ensureAudioCtx()) return;
  if(audioCtx.state === 'suspended') audioCtx.resume();
  const o = audioCtx.createOscillator();
  const g = audioCtx.createGain();
  o.type = type;
  o.frequency.value = freq;
  o.connect(g); g.connect(audioCtx.destination);
  const now = audioCtx.currentTime;
  g.gain.setValueAtTime(0, now);
  g.gain.linearRampToValueAtTime(vol, now + attack);
  g.gain.linearRampToValueAtTime(0.0001, now + dur - release);
  o.start(now); o.stop(now + dur + 0.02);
}
function playSuccess(){
  playTone({freq:720, dur:0.12, vol:0.07}); setTimeout(()=>playTone({freq:880, dur:0.12, vol:0.07}),110);
  setTimeout(()=>playTone({freq:980, dur:0.16, vol:0.08}),230);
}
function playError(){ playTone({freq:220, dur:0.3, vol:0.06}); }
function playRoundDing(){ playTone({freq:1200, dur:0.12, vol:0.06}); }

/* --- Elementos DOM --- */
const startScreen = document.getElementById('start-screen');
const startMain = document.getElementById('startMain');
const gameContainer = document.getElementById('game-container');
const board = document.getElementById('board');
const scoreEl = document.getElementById('score');
const lifeEls = [document.getElementById('life1'), document.getElementById('life2'), document.getElementById('life3')];
const bubble = document.getElementById('bubble');
const roundName = document.getElementById('roundName');
const playBtn = document.getElementById('playBtn');
const menuBtn = document.getElementById('menuBtn');
const endWin = document.getElementById('end-win');
const endLose = document.getElementById('end-lose');

/* --- Juego: datos y estado --- */
const rounds = [
  {
    name: 'Regar las Flores',
    message:'¡Hora de regar las flores! 🌸💧',
    icons:[
      'https://img.icons8.com/color/96/flower.png',
      'https://img.icons8.com/color/96/sunflower.png',
      'https://img.icons8.com/color/96/hibiscus.png',
      'https://img.icons8.com/color/96/cherry-blossom.png'
    ]
  },
  {
    name:'Cosechar Frutas',
    message:'¡Vamos a cosechar frutas frescas! 🍎🍌',
    icons:[
      'https://img.icons8.com/color/96/apple.png',
      'https://img.icons8.com/color/96/banana.png',
      'https://img.icons8.com/color/96/pear.png',
      'https://img.icons8.com/color/96/grapes.png'
    ]
  },
  {
    name:'Juntando la Cosecha',
    message:'¡Es hora de juntar frutas y verduras! 🥕🍎',
    icons:[
      'https://img.icons8.com/color/96/apple.png',
      'https://img.icons8.com/color/96/carrot.png',
      'https://img.icons8.com/color/96/banana.png',
      'https://img.icons8.com/color/96/corn.png'
    ]
  }
];

let currentRound = 0;
let sequence = [];
let playerIndex = 0;
let listening = false;
let score = 0;
let lives = 3;

/* --- Utilidades --- */
function setBubble(text){
  bubble.textContent = text;
}
function updateScore(){
  scoreEl.textContent = 'Puntos: ' + score;
}
function updateLivesUI(){
  for(let i=0;i<3;i++){
    if(i < lives) lifeEls[i].classList.remove('dead');
    else lifeEls[i].classList.add('dead');
  }
}

/* --- Flow: mostrar/ocultar pantallas --- */
function showStart(){
  startScreen.style.display = 'flex';
  gameContainer.style.display = 'none';
  endWin.style.display = 'none';
  endLose.style.display = 'none';
  setBubble('¡Hola! Presiona Comenzar para jugar.');
}
function enterGame(){
  startScreen.style.display = 'none';
  gameContainer.style.display = 'block';
  endWin.style.display = 'none';
  endLose.style.display = 'none';
}
function showWinOverlay(){
  endWin.style.display = 'flex';
}
function showLoseOverlay(){
  endLose.style.display = 'flex';
}

/* --- Generar y mostrar tablero --- */
function buildBoard(icons){
  board.innerHTML = '';
  icons.forEach((url, idx) => {
    const cell = document.createElement('div');
    cell.className = 'cell';
    cell.setAttribute('role','button');
    cell.setAttribute('aria-label','Elemento ' + (idx+1));
    const img = document.createElement('img');
    img.src = url;
    img.alt = '';
    cell.appendChild(img);
    cell.addEventListener('click', ()=> onCellClick(idx));
    board.appendChild(cell);
  });
}

/* --- Secuencia --- */
function generateSequence(len){
  const iconsCount = rounds[currentRound].icons.length;
  const seq = [];
  for(let i=0;i<len;i++) seq.push(Math.floor(Math.random()*iconsCount));
  return seq;
}
function flashIndex(idx){
  const cells = board.querySelectorAll('.cell');
  if(!cells[idx]) return;
  cells[idx].classList.add('highlight');
  setTimeout(()=> cells[idx].classList.remove('highlight'), 500);
}
async function showSequence(seq){
  listening = false;
  playRoundDing();
  for(let i=0;i<seq.length;i++){
    flashIndex(seq[i]);
    // sound per round
    if(currentRound===0) playTone({freq:620, dur:0.12, vol:0.06});
    else if(currentRound===1) playTone({freq:880, dur:0.12, vol:0.07});
    else playTone({freq:420, dur:0.12, vol:0.06});
    await new Promise(r=>setTimeout(r, 900));
  }
  listening = true;
  setBubble('¡Tu turno! Repite la secuencia');
}

/* --- Interacciones --- */
function onCellClick(index){
  if(!listening) return;
  // check
  if(index !== sequence[playerIndex]){
    // error
    playError();
    lives = Math.max(0, lives-1);
    updateLivesUI();
    setBubble('¡Ups! Te equivocaste.');
    listening = false;
    playerIndex = 0;
    // if no lives -> lose
    if(lives <= 0){
      setTimeout(()=> showLoseOverlay(), 700);
    } else {
      // replay current sequence after short pause
      setTimeout(()=> showSequence(sequence), 900);
    }
    return;
  }

  // correct step
  // provide brief feedback
  const cells = board.querySelectorAll('.cell');
  if(cells[index]){ cells[index].classList.add('highlight'); setTimeout(()=>cells[index].classList.remove('highlight'),240); }
  // award points per correct item
  score += 10;
  updateScore();

  playerIndex++;
  if(playerIndex >= sequence.length){
    // completed sequence for this round
    playSuccess();
    setBubble('¡Muy bien! 😊');
    listening = false;
    playerIndex = 0;
    // bonus for full sequence
    score += 20;
    updateScore();
    // advance to next round after small pause
    currentRound++;
    if(currentRound >= rounds.length){
      // win
      setTimeout(()=> showWinOverlay(), 900);
    } else {
      setTimeout(()=> startRound(), 900);
    }
  }
}

/* --- Start / Round logic --- */
function startRound(){
  // show current round info
  const rdata = rounds[currentRound];
  roundName.textContent = 'Ronda: ' + rdata.name;
  setBubble(rdata.message);
  // Build board with icons
  buildBoard(rdata.icons);
  // Create sequence with length depending on round (2,3,4)
  const seqLen = 2 + currentRound; // 2,3,4
  sequence = generateSequence(seqLen);
  playerIndex = 0;
  // small delay then show sequence
  setTimeout(()=> showSequence(sequence), 800);
}

/* --- Public controls --- */
startMain.addEventListener('click', ()=>{
  // resume audio on gesture
  if(ensureAudioCtx() && audioCtx.state === 'suspended') audioCtx.resume();
  // reset
  score = 0; lives = 3; currentRound = 0; updateScore(); updateLivesUI();
  enterGame();
  setTimeout(()=> startRound(), 300);
});
playBtn.addEventListener('click', ()=>{
  // in-game start: restart from currentRound (useful)
  score = 0; lives = 3; currentRound = 0; updateScore(); updateLivesUI(); startRound();
});
menuBtn.addEventListener('click', ()=>{
  showStart();
});

/* --- End screens restart --- */
function restartFromEnd(won){
  endWin.style.display = 'none';
  endLose.style.display = 'none';
  // choose: go back to menu or restart immediately in game
  score = 0; lives = 3; currentRound = 0; updateScore(); updateLivesUI();
  // if you prefer to return to menu instead uncomment next line:
  // return showStart();
  // else: restart the play directly
  enterGame();
  setTimeout(()=> startRound(), 300);
}

/* --- Init UI --- */
updateScore();
updateLivesUI();
showStart();

</script>
</body>
</html>
