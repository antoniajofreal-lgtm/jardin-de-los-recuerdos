<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1.0" />
  <title>El Jardín de los Recuerdos</title>
  <style>
    :root {
      --bubble-w: 260px;
      --bottom-space: 180px; /* espacio para jardinero + burbuja */
    }
    html,body { height:100%; margin:0; font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial; background:#e8f6ea; }
    /* --- START SCREEN --- */
    #start-screen {
      position:fixed; inset:0; display:flex; flex-direction:column; align-items:center; justify-content:center;
      background: linear-gradient(180deg,#dff3e6,#bfead0);
      z-index: 400;
      padding:20px; box-sizing:border-box;
    }
    #start-screen h1 { margin:0 0 12px; font-size:2.1rem; text-align:center; }
    .btn { padding:12px 20px; border-radius:12px; border:0; cursor:pointer; background:#57b86a; color:#fff; font-size:1rem; }
    .btn:active { transform: scale(.98); }

    /* --- GAME --- */
    #game-container { display:none; padding:18px; box-sizing:border-box; min-height:100vh; }
    header.appbar { display:flex; justify-content:center; align-items:center; margin-bottom:8px; }
    header.appbar h2 { margin:0; color:#2b6a36; }

    .hud { display:flex; justify-content:space-between; align-items:center; gap:12px; margin-bottom:8px; }
    .score { font-weight:700; color:#1e6b2a; }
    .lives { color:#9b4b4b; }

    #elements { display:flex; flex-wrap:wrap; justify-content:center; gap:18px; padding-bottom: var(--bottom-space); }

    .cell {
      width:140px; height:140px; border-radius:14px; background:#fff; display:flex; align-items:center; justify-content:center;
      font-size:56px; box-shadow:0 6px 18px rgba(0,0,0,0.08);
      touch-action: manipulation; user-select:none; -webkit-user-select:none;
    }
    .cell.highlight { background:#c6f6d5; transform: scale(1.03); transition: transform .12s; }

    /* --- Jardinero + burbuja (anclado, no cubrirá elementos porque reservamos espacio) --- */
    #gardener-wrapper {
      position:fixed; right:12px; bottom:12px; display:flex; flex-direction:column; align-items:center; gap:10px;
      z-index: 900; pointer-events:none;
    }
    #speech-bubble {
      width: var(--bubble-w); max-width: 85vw; background: rgba(255,255,255,0.96);
      border:2px solid #3f3f3f; border-radius:14px; padding:10px; font-size:18px; text-align:center;
      box-shadow:0 8px 20px rgba(0,0,0,0.12);
    }
    #gardener-img { width:110px; height:auto; display:block; }

    /* --- overlays finales --- */
    .overlay { position:fixed; inset:0; display:none; align-items:center; justify-content:center; z-index:800; background: rgba(255,255,255,0.95); }
    .overlay h2 { margin:0 0 8px; color:#2b6a36; }

    /* responsive */
    @media (max-width:520px){
      .cell { width:120px; height:120px; font-size:48px; }
      #speech-bubble { font-size:16px; width: 200px; }
      #gardener-img { width:90px; }
      :root { --bottom-space: 150px; }
    }
  </style>
</head>
<body>

  <!-- START -->
  <div id="start-screen" role="dialog" aria-modal="true">
    <h1>🌳 El Jardín de los Recuerdos 🌳</h1>
    <p style="max-width:560px;text-align:center;">Ayuda al jardinero a cuidar su jardín recordando y repitiendo secuencias. (2 secuencias por ronda)</p>
    <div style="margin-top:14px;">
      <button id="startBtn" class="btn">🌸 Comenzar Juego</button>
      <button id="musicToggleStart" class="btn" style="background:#6ca3f5;margin-left:10px;">🔊 Música</button>
    </div>
  </div>

  <!-- GAME -->
  <div id="game-container" aria-live="polite">
    <header class="appbar"><h2>El Jardín de los Recuerdos</h2></header>
    <div class="hud">
      <div class="score" id="scoreDisplay">Puntos: 0</div>
      <div class="lives" id="livesDisplay">Vidas: 🌸🌸🌸</div>
    </div>

    <main id="elements" role="application" aria-label="Jardín interactivo"></main>

    <div style="margin-top:12px; display:flex; justify-content:center; gap:10px;">
      <button id="restartBtn" class="btn" style="background:#f6a45d;">🔄 Reiniciar</button>
      <button id="musicToggleGame" class="btn" style="background:#6ca3f5;">🔊 Música</button>
    </div>
  </div>

  <!-- Gardener + Bubble (anchored) -->
  <div id="gardener-wrapper" aria-hidden="true">
    <div id="speech-bubble" role="status" aria-live="polite">¡Bienvenido al Jardín!</div>
    <img id="gardener-img" src="https://cdn.pixabay.com/photo/2014/04/02/10/57/gardener-303491_1280.png" alt="Jardinero" />
  </div>

  <!-- Overlays -->
  <div id="winOverlay" class="overlay">
    <div>
      <h2>🌟 ¡Gracias por jugar! 🌟</h2>
      <p>Siempre eres bienvenido en el Jardín de los Recuerdos 🌼</p>
      <div style="margin-top:12px;"><button id="winRestart" class="btn">Volver a jugar</button></div>
    </div>
  </div>

  <div id="loseOverlay" class="overlay">
    <div>
      <h2>💪 ¡Ánimo! 💪</h2>
      <p>Inténtalo otra vez, el jardín te espera.</p>
      <div style="margin-top:12px;"><button id="loseRestart" class="btn">Reintentar</button></div>
    </div>
  </div>

  <!-- Audio -->
  <audio id="bgMusic" loop src="https://files.freemusicarchive.org/storage-freemusicarchive-org/music/ccCommunity/Jahzzar/Traveller/Jahzzar_-_05_-_Siesta.mp3"></audio>
  <audio id="sndSequence" src="https://actions.google.com/sounds/v1/cartoon/wood_plank_flicks.ogg"></audio>
  <audio id="sndCorrect" src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg"></audio>
  <audio id="sndError" src="https://actions.google.com/sounds/v1/cartoon/metal_thud_and_clang.ogg"></audio>

<script>
/* ---------- Utilidades ---------- */
function emojiDataURI(emoji, size=128){
  const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='${size}' height='${size}'><rect width='100%' height='100%' fill='%23ffffff'/><text x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' font-size='72'>${emoji}</text></svg>`;
  return 'data:image/svg+xml;utf8,' + encodeURIComponent(svg);
}

/* ---------- Elementos DOM ---------- */
const startScreen = document.getElementById('start-screen');
const startBtn = document.getElementById('startBtn');
const musicToggleStart = document.getElementById('musicToggleStart');
const gameContainer = document.getElementById('game-container');
const elementsWrap = document.getElementById('elements');
const scoreDisplay = document.getElementById('scoreDisplay');
const livesDisplay = document.getElementById('livesDisplay');
const restartBtn = document.getElementById('restartBtn');
const musicToggleGame = document.getElementById('musicToggleGame');
const speechBubble = document.getElementById('speech-bubble');
const gardenerImg = document.getElementById('gardener-img');
const winOverlay = document.getElementById('winOverlay');
const loseOverlay = document.getElementById('loseOverlay');
const winRestart = document.getElementById('winRestart');
const loseRestart = document.getElementById('loseRestart');

const bgMusic = document.getElementById('bgMusic');
const sndSequence = document.getElementById('sndSequence');
const sndCorrect = document.getElementById('sndCorrect');
const sndError = document.getElementById('sndError');

/* ---------- Estado del juego ---------- */
let started = false;
let score = 0;
let lives = 3;
let currentRound = 0; // 1..3
const sequencesPerRound = 2;
let sequenceCount = 0;

/* sequence will be array of indices (0..N-1) referencing the current pool cells */
let sequence = [];
let playerIndex = 0;
let listening = false;

/* Pools (unique items in pool) */
const pools = [
  ['🌸','🌻','🌷','🌼'], // round 1 (flowers)
  ['🍎','🍌','🍐','🍇'], // round 2 (fruits)
  ['🥕','🌽','🍅','🍆']  // round 3 (mixed)
];

/* ---------- Helpers UI ---------- */
function setBubble(text, minMs=3000){
  speechBubble.textContent = text;
  // keep visible — we don't replace before minMs in code paths
}
function updateScoreUI(){ scoreDisplay.textContent = 'Puntos: ' + score; }
function updateLivesUI(){ livesDisplay.textContent = 'Vidas: ' + '🌸'.repeat(Math.max(0,lives)); }

/* Image fallback: if gardener fails to load, replace with emoji svg */
gardenerImg.onerror = function(){
  this.onerror=null;
  this.src = emojiDataURI('🧑‍🌾', 160);
};

/* ---------- Audio safe play (returns Promise) ---------- */
async function safePlay(audioEl){
  if(!audioEl) return Promise.resolve();
  try{
    audioEl.currentTime = 0;
    await audioEl.play();
  }catch(e){
    // play may be blocked until user gesture; ignore
    console.debug('audio play blocked', e);
  }
}

/* Toggle BG music (with promise handling) */
let musicOn = false;
async function toggleMusic(){
  if(musicOn){
    try{ bgMusic.pause(); }catch(e){ console.debug(e); }
    musicOn = false;
    musicToggleStart.textContent = '🔊 Música';
    musicToggleGame.textContent = '🔊 Música';
  } else {
    await safePlay(bgMusic);
    musicOn = !bgMusic.paused;
    musicToggleStart.textContent = musicOn ? '🔇 Música' : '🔊 Música';
    musicToggleGame.textContent = musicOn ? '🔇 Música' : '🔊 Música';
  }
}

/* Attach music toggles */
musicToggleStart.addEventListener('click', toggleMusic);
musicToggleGame.addEventListener('click', toggleMusic);

/* ---------- Start / Restart ---------- */
function resetState(){
  score = 0;
  lives = 3;
  currentRound = 0;
  sequenceCount = 0;
  sequence = [];
  playerIndex = 0;
  listening = false;
  updateScoreUI();
  updateLivesUI();
  winOverlay.style.display = 'none';
  loseOverlay.style.display = 'none';
}

async function startGame(){
  if(started) return;
  started = true;
  // try to start music (mobile may require a gesture)
  await toggleMusic().catch(()=>{ /* ignore */ });
  startScreen.style.display = 'none';
  gameContainer.style.display = 'block';
  resetState();
  startRound();
}

/* restart handlers */
restartBtn.addEventListener('click', ()=>{
  resetState();
  startScreen.style.display = 'none';
  gameContainer.style.display = 'block';
  startRound();
});
winRestart.addEventListener('click', ()=>{
  winOverlay.style.display = 'none';
  resetState();
  startRound();
});
loseRestart.addEventListener('click', ()=>{
  loseOverlay.style.display = 'none';
  resetState();
  startRound();
});

/* Bind start button (pointer + click fallback) */
startBtn.addEventListener('pointerdown', (e)=>{ e.preventDefault(); startGame(); });
startBtn.addEventListener('click', startGame);

/* ---------- Board building and sequence logic (indices) ---------- */
function buildBoardForRound(roundIndex){
  elementsWrap.innerHTML = '';
  const pool = pools[roundIndex - 1];
  // create a cell per pool item; index corresponds to pool index
  pool.forEach((symbol, idx) => {
    const div = document.createElement('div');
    div.className = 'cell';
    div.setAttribute('role','button');
    div.dataset.idx = String(idx);
    div.textContent = symbol;
    // pointerdown reduces mobile latency
    div.addEventListener('pointerdown', (ev)=>{ ev.preventDefault(); onCellPressed(idx); });
    elementsWrap.appendChild(div);
  });
}

/* Generate a sequence of indices (length len) for currentRound */
function generateSequence(len){
  const poolLen = pools[currentRound - 1].length;
  const seq = [];
  for(let i=0;i<len;i++){
    seq.push(Math.floor(Math.random() * poolLen));
  }
  return seq;
}

/* Show sequence by highlighting cells by index */
async function showSequence(seq){
  listening = false;
  setBubble('Observa la secuencia...', 2800);
  await new Promise(r=>setTimeout(r, 2800)); // give time to read
  const cells = Array.from(elementsWrap.querySelectorAll('.cell'));
  for(let i=0;i<seq.length;i++){
    const idx = seq[i];
    const cell = cells[idx];
    if(cell){
      cell.classList.add('highlight');
      // sound
      safePlay(sndSequence);
      await new Promise(r=>setTimeout(r, 800));
      cell.classList.remove('highlight');
    } else {
      // if a cell is missing (shouldn't happen), wait anyway
      await new Promise(r=>setTimeout(r, 800));
    }
    await new Promise(r=>setTimeout(r, 200));
  }
  listening = true;
  setBubble('¡Tu turno! Repite la secuencia', 2800);
}

/* ---------- Rounds flow (2 sequences per round) ---------- */
function startRound(){
  currentRound++;
  if(currentRound > pools.length){
    // finished all rounds: win
    showWin();
    return;
  }
  sequenceCount = 0;
  setBubble('Ronda ' + currentRound + ' comienza...', 2400);
  // build UI board for this round
  buildBoardForRound(currentRound);
  // reserve space already with CSS bottom padding
  // start first sequence after brief delay (bubble display)
  setTimeout(()=> startNewSequence(), 2400);
}

/* Start/generate a new sequence inside the same round */
function startNewSequence(){
  const seqLen = 2 + (currentRound - 1) + sequenceCount; // e.g., round1: 2, then 3
  sequence = generateSequence(seqLen);
  playerIndex = 0;
  listening = false;
  showSequence(sequence).catch(e=>console.debug(e));
}

/* ---------- Input handling ---------- */
function onCellPressed(cellIdx){
  if(!listening) return;
  // compare index directly
  if(cellIdx !== sequence[playerIndex]){
    // incorrect
    safePlay(sndError);
    lives = Math.max(0, lives - 1);
    updateLivesUI();
    setBubble('¡Ups! Esa no es la correcta.', 2400);
    listening = false;
    playerIndex = 0;
    if(lives <= 0){
      // lose
      setTimeout(()=> showLose(), 700);
    } else {
      // replay same sequence after short delay
      setTimeout(()=> showSequence(sequence), 900);
    }
    return;
  }

  // correct step
  safePlay(sndCorrect);
  // visual feedback
  const cells = Array.from(elementsWrap.querySelectorAll('.cell'));
  const cell = cells[cellIdx];
  if(cell){ cell.classList.add('highlight'); setTimeout(()=>cell.classList.remove('highlight'),240); }
  playerIndex++;
  score += 10;
  updateScoreUI();

  if(playerIndex >= sequence.length){
    // completed this sequence
    score += 20; updateScoreUI();
    sequenceCount++;
    listening = false;
    if(sequenceCount < sequencesPerRound){
      setBubble('¡Muy bien! Ahora otra secuencia.', 2000);
      setTimeout(()=> startNewSequence(), 1200);
    } else {
      // finished round: go to next
      setBubble('¡Ronda completada! 🎉', 2000);
      setTimeout(()=> startRound(), 1200);
    }
  }
}

/* ---------- Win / Lose ---------- */
function showWin(){
  gameContainer.style.display = 'none';
  winOverlay.style.display = 'flex';
  setBubble('¡Tu jardín florece! 🌸', 3000);
  safePlay(sndCorrect);
}

function showLose(){
  gameContainer.style.display = 'none';
  loseOverlay.style.display = 'flex';
  setBubble('¡Ánimo! Intenta otra vez.', 3000);
  safePlay(sndError);
}

/* ---------- Initialization ---------- */
updateScoreUI();
updateLivesUI();
setBubble('¡Hola! Presiona "Comenzar Juego".');

/* Debug helper (console) */
console.debug('Jardin de los Recuerdos - script cargado');

</script>
</body>
</html>
