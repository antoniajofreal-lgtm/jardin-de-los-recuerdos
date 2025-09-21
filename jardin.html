<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" />
  <title>El Jardín de los Recuerdos</title>
  <style>
    html,body{height:100%;margin:0;padding:0;box-sizing:border-box;background:#f0f8f5;font-family:system-ui,Arial,sans-serif}
    /* Start screen */
    #start-screen{position:fixed;inset:0;background:url("https://cdn.pixabay.com/photo/2016/11/29/12/54/cartoon-1869538_1280.jpg") center/cover no-repeat;display:flex;flex-direction:column;align-items:center;justify-content:center;color:#fff;text-shadow:2px 2px 5px rgba(0,0,0,0.6);z-index:60}
    #start-screen h1{font-size:2.4rem;margin:0 12px 12px}
    .btn-start{padding:14px 28px;font-size:1.15rem;background:#ff80bf;border:0;border-radius:14px;color:#fff;cursor:pointer;touch-action:manipulation}
    .btn-start:active{transform:scale(.98)}
    /* Game */
    #game-container{display:none;position:relative;width:100%;height:100%;padding:16px;box-sizing:border-box;overflow:auto}
    header.appbar{display:flex;justify-content:center;padding-top:6px}
    header.appbar h2{margin:0;color:#2d572c}
    .hud{display:flex;justify-content:space-between;align-items:center;margin-top:10px;padding:0 6px}
    .score{font-weight:700;color:#155724}
    .lives{color:#7c3a3a}
    #game-board{display:grid;grid-template-columns:repeat(2,minmax(120px,150px));gap:16px;justify-content:center;margin-top:16px;margin-bottom:140px;margin-inline:auto;max-width:460px}
    .cell{width:150px;height:150px;border-radius:14px;background:#fff;display:flex;align-items:center;justify-content:center;box-shadow:0 6px 14px rgba(0,0,0,.08);cursor:pointer;touch-action:manipulation;border:2px solid rgba(0,0,0,0.02)}
    .cell img{width:86%;height:86%;object-fit:contain;pointer-events:none}
    .cell.highlight{background:#c6f6d5;box-shadow:0 10px 22px rgba(0,0,0,0.12)}
    /* gardener top-right */
    #gardener-container{position:fixed;top:12px;right:12px;display:flex;gap:8px;align-items:flex-start;z-index:65;pointer-events:none}
    #gardener-container img{width:92px;display:block}
    #bubble{max-width:240px;background:#fff;border:2px solid #333;border-radius:12px;padding:10px;font-size:0.98rem;box-shadow:0 6px 18px rgba(0,0,0,0.12);pointer-events:none}
    /* overlays */
    .overlay{position:fixed;inset:0;display:none;align-items:center;justify-content:center;z-index:70;background:rgba(255,255,255,0.95);flex-direction:column;text-align:center;padding:18px}
    .overlay h1{margin:0 0 12px;font-size:1.9rem;color:#2d572c}
    .overlay img{width:140px;margin-bottom:10px}
    .overlay .btn{padding:12px 18px;border-radius:10px;border:0;background:#77c977;color:#fff;cursor:pointer}
    @media(max-width:520px){ .cell{width:130px;height:130px} #bubble{max-width:160px;font-size:.9rem} #gardener-container img{width:78px} }
  </style>
</head>
<body>
  <!-- START -->
  <div id="start-screen" role="dialog" aria-modal="true">
    <h1>🌳 El Jardín de los Recuerdos 🌳</h1>
    <!-- inline onclick backup + id for JS fallback -->
    <button id="startMain" class="btn-start" type="button" onclick="startGameInline()">🌸 Comenzar Juego 🌸</button>
  </div>

  <!-- GAME -->
  <div id="game-container" aria-live="polite">
    <header class="appbar"><h2>El Jardín de los Recuerdos</h2></header>
    <div class="hud">
      <div class="left-hud">
        <div id="score" class="score">Puntos: 0</div>
        <div id="lives" class="lives">Vidas: 🌸🌸🌸</div>
      </div>
      <div id="roundName" class="roundName" aria-live="polite">Ronda: -</div>
    </div>

    <div id="game-board" role="grid" aria-label="Jardín interactivo"></div>
  </div>

  <!-- gardener top-right (visual only) -->
  <div id="gardener-container" aria-hidden="true">
    <div id="bubble" aria-live="polite">¡Hola! Presiona Comenzar para jugar.</div>
    <img id="gardenerImg" src="https://clker.com/cliparts/a/7/a/1/1516253069442174405cartoon-gardener-clipart.hi.png" alt="Jardinero ilustrado" onerror="this.onerror=null; this.src=generateEmojiDataURI('🧑‍🌾')">
  </div>

  <!-- WIN overlay -->
  <div id="end-win" class="overlay" role="dialog" aria-modal="true">
    <h1>🌟 ¡Gracias por jugar! 🌟</h1>
    <img src="https://clker.com/cliparts/a/7/a/1/1516253069442174405cartoon-gardener-clipart.hi.png" alt="Jardinero feliz" onerror="this.onerror=null; this.src=generateEmojiDataURI('🧑‍🌾')">
    <p>Siempre eres bienvenido en el Jardín de los Recuerdos 🌼</p>
    <div style="height:10px"></div>
    <button class="btn" onclick="restartFromEnd()">🔄 Volver a jugar</button>
  </div>

  <!-- LOSE overlay -->
  <div id="end-lose" class="overlay" role="dialog" aria-modal="true">
    <h1>💔 ¡Ánimo! 💔</h1>
    <img src="https://clker.com/cliparts/a/7/a/1/1516253069442174405cartoon-gardener-clipart.hi.png" alt="Jardinero preocupado" onerror="this.onerror=null; this.src=generateEmojiDataURI('🧑‍🌾')">
    <p>¡Inténtalo otra vez, tú puedes! 💪🌱</p>
    <div style="height:10px"></div>
    <button class="btn" onclick="restartFromEnd()">🔄 Volver a intentar</button>
  </div>

<script>
/* ==============================
   Utilidades
   ============================== */

function generateEmojiDataURI(emoji){
  // SVG with emoji centered (safe fallback)
  const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='128' height='128'><rect width='100%' height='100%' fill='%23ffffff'/><text x='50%' y='50%' dominant-baseline='middle' text-anchor='middle' font-size='72'>${emoji}</text></svg>`;
  return 'data:image/svg+xml;utf8,' + encodeURIComponent(svg);
}

/* Audio simple cache (Audio elements) */
const audioUrls = {
  waterDrop: 'https://freesound.org/data/previews/165/165206_1631209-lq.wav',
  success: 'https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg',
  error: 'https://actions.google.com/sounds/v1/cartoon/wood_plank_flicks.ogg'
};
const audioCache = {};
function playSound(name){
  const url = audioUrls[name];
  if(!url) return;
  try{
    if(!audioCache[name]) audioCache[name] = new Audio(url);
    audioCache[name].currentTime = 0;
    audioCache[name].play().catch(e=>{
      // audio can be blocked until gesture; this is expected on some browsers
      console.debug('audio play blocked or error', e);
    });
  }catch(e){ console.debug('playSound err', e); }
}

/* ==============================
   Estado / datos del juego
   ============================== */

let started = false;
let score = 0, lives = 3, currentRound = 0;
let sequence = [], playerIndex = 0, listening = false;
const sequencesPerRound = 2;
let sequenceCount = 0;

const startMainBtn = document.getElementById('startMain');
const startScreen = document.getElementById('start-screen');
const gameContainer = document.getElementById('game-container');
const board = document.getElementById('game-board');
const scoreEl = document.getElementById('score');
const livesEl = document.getElementById('lives');
const bubble = document.getElementById('bubble');
const roundNameEl = document.getElementById('roundName');
const endWin = document.getElementById('end-win');
const endLose = document.getElementById('end-lose');

/* Rondas: intento con URLs públicas + fallback emoji */
const rounds = [
  {
    name: 'Regar las Flores',
    message: '¡Hora de regar las flores! 🌸💧',
    icons: [
      'https://freesvg.org/img/apple-fruit-icon.svg',
      'https://freesvg.org/img/banana-fruit-icon.svg',
      'https://freesvg.org/img/strawberry-fruit-icon.svg',
      'https://freesvg.org/img/lemon-fruit-icon.svg'
    ],
    emojiFallbacks: ['🍎','🍌','🍓','🍋']
  },
  {
    name: 'Cosechar Frutas',
    message: '¡Vamos a cosechar frutas frescas! 🍎🍌',
    icons: [
      'https://freesvg.org/img/apple-fruit-icon.svg',
      'https://freesvg.org/img/grapes-fruit-icon.svg',
      'https://freesvg.org/img/orange-fruit-icon.svg',
      'https://freesvg.org/img/pear-fruit-icon.svg'
    ],
    emojiFallbacks: ['🍎','🍇','🍊','🍐']
  },
  {
    name: 'Juntando la Cosecha',
    message: '¡Es hora de juntar frutas y verduras! 🥕🍎',
    icons: [
      'https://freesvg.org/img/carrot-vegetable-icon.svg',
      'https://freesvg.org/img/apple-fruit-icon.svg',
      'https://freesvg.org/img/banana-fruit-icon.svg',
      'https://freesvg.org/img/corn-vegetable-icon.svg'
    ],
    emojiFallbacks: ['🥕','🍎','🍌','🌽']
  }
];

/* ==============================
   UI helpers
   ============================== */
function setBubble(text){
  bubble.textContent = text;
}
function updateScore(){ scoreEl.textContent = 'Puntos: ' + score; }
function updateLives(){ livesEl.textContent = 'Vidas: ' + '🌸'.repeat(Math.max(0,lives)); }
function setRoundName(text){ roundNameEl.textContent = 'Ronda: ' + text; }

/* ==============================
   Start handling (robusto)
   ============================== */
// inline onclick calls this wrapper
function startGameInline(){ _startHandler(); }

function _startHandler(){
  if(started) return;
  started = true;
  startMainBtn.textContent = 'Cargando...';
  startMainBtn.disabled = true;
  // start game after small delay, give time for UI update
  enterGame();
  setTimeout(()=> startRound(), 300);
}

// multi-listener for safety (pointerdown + click + touchstart)
startMainBtn.addEventListener('pointerdown', (e)=>{ e.preventDefault(); _startHandler(); });
startMainBtn.addEventListener('click', ()=> _startHandler());
startMainBtn.addEventListener('touchstart', ()=> _startHandler());

/* ==============================
   Board & sequences
   ============================== */
function enterGame(){
  startScreen.style.display = 'none';
  gameContainer.style.display = 'block';
  endWin.style.display = 'none';
  endLose.style.display = 'none';
  score = 0; lives = 3; currentRound = 0;
  updateScore(); updateLives();
  setBubble('¡Empezamos! ' + rounds[currentRound].message);
  setRoundName(rounds[currentRound].name);
}

function buildBoard(iconUrls, emojiFallbacks){
  board.innerHTML = '';
  for(let i=0;i<iconUrls.length;i++){
    const url = iconUrls[i];
    const fbEmoji = (emojiFallbacks && emojiFallbacks[i]) ? emojiFallbacks[i] : '🌼';
    const cell = document.createElement('div');
    cell.className = 'cell';
    cell.setAttribute('role','button');
    cell.setAttribute('aria-label','Elemento ' + (i+1));
    const img = document.createElement('img');
    img.src = url;
    img.alt = fbEmoji;
    // fallback if cross-origin blocked or 404
    img.onerror = function(){ this.onerror=null; this.src = generateEmojiDataURI(fbEmoji); };
    cell.appendChild(img);
    // pointerdown reduces latency on touch
    cell.addEventListener('pointerdown', (ev)=>{ ev.preventDefault(); onCellClick(i); });
    board.appendChild(cell);
  }
}

function generateSequence(len){
  const n = rounds[currentRound].icons.length;
  const seq = [];
  for(let i=0;i<len;i++) seq.push(Math.floor(Math.random()*n));
  return seq;
}

async function showSequence(seq){
  listening = false;
  setBubble('Observa la secuencia');
  playSound('waterDrop');
  const cells = board.querySelectorAll('.cell');
  for(let i=0;i<seq.length;i++){
    const idx = seq[i];
    if(cells[idx]) cells[idx].classList.add('highlight');
    // small sound per item (different per round)
    if(currentRound===0) playSound('waterDrop');
    else if(currentRound===1) playSound('success');
    else playSound('waterDrop');
    // wait
    await new Promise(r=>setTimeout(r, 900));
    if(cells[idx]) cells[idx].classList.remove('highlight');
  }
  listening = true;
  setBubble('¡Tu turno! Repite la secuencia');
}

/* ==============================
   Round flow (2 sequences per round)
   ============================== */
function startRound(){
  sequenceCount = 0;
  const r = rounds[currentRound];
  setRoundName(r.name);
  setBubble(r.message);
  buildBoard(r.icons, r.emojiFallbacks);
  startNewSequence();
}

function startNewSequence(){
  const seqLen = 2 + currentRound + sequenceCount; // grows within round
  sequence = generateSequence(seqLen);
  playerIndex = 0;
  listening = false;
  setTimeout(()=> showSequence(sequence), 600);
}

/* ==============================
   Player input
   ============================== */
function onCellClick(index){
  if(!listening) return;
  // protect out-of-range
  if(playerIndex >= sequence.length) return;
  if(index !== sequence[playerIndex]){
    // wrong
    playSound('error');
    lives = Math.max(0, lives - 1);
    updateLives();
    setBubble('¡Ups! Esa no es la correcta.');
    listening = false;
    playerIndex = 0;
    if(lives <= 0){
      setTimeout(()=> showLose(), 700);
    } else {
      setTimeout(()=> showSequence(sequence), 900);
    }
    return;
  }
  // correct
  playSound('success');
  const cells = board.querySelectorAll('.cell');
  if(cells[index]){ cells[index].classList.add('highlight'); setTimeout(()=>cells[index].classList.remove('highlight'),280); }
  score += 10; updateScore();
  playerIndex++;
  if(playerIndex >= sequence.length){
    // sequence completed
    score += 20; updateScore(); // bonus
    sequenceCount++;
    listening = false;
    if(sequenceCount < sequencesPerRound){
      setBubble('¡Muy bien! Ahora otra secuencia.');
      setTimeout(()=> startNewSequence(), 900);
    } else {
      // completed round
      currentRound++;
      if(currentRound >= rounds.length){
        setTimeout(()=> showWin(), 900);
      } else {
        setBubble('¡Ronda completada! 🎉');
        setTimeout(()=> startRound(), 1100);
      }
    }
  }
}

/* ==============================
   Overlays + restart
   ============================== */
function showWin(){
  gameContainer.style.display = 'none';
  endWin.style.display = 'flex';
  setBubble('¡Felicidades!');
}
function showLose(){
  gameContainer.style.display = 'none';
  endLose.style.display = 'flex';
  setBubble('¡Ánimo! Intenta de nuevo.');
}
function restartFromEnd(){
  endWin.style.display = 'none';
  endLose.style.display = 'none';
  // reset everything
  score = 0; lives = 3; currentRound = 0;
  updateScore(); updateLives();
  gameContainer.style.display = 'block';
  setTimeout(()=> startRound(), 300);
}

/* ==============================
   Inicialización
   ============================== */
updateScore();
updateLives();
setBubble('¡Hola! Presiona Comenzar para jugar.');

/* Small debug: print instructions to console */
console.debug('El archivo jardin.html cargó — si hay problemas abre la consola para ver errores.');

</script>
</body>
</html>
