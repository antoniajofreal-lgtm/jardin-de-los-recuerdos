<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>El Jardín de los Recuerdos — versión WebAudio</title>
<style>
  :root{
    --bg:#eef8f2;
    --card:#ffffff;
    --accent:#2b8a3e;
    --accent-2:#19703b;
    --muted:#6b7280;
    --text:#083018;
    --big:20px;
  }
  *{box-sizing:border-box}
  body{ margin:0; font-family:system-ui, -apple-system, "Segoe UI", Roboto, Arial; background:var(--bg); color:var(--text); -webkit-font-smoothing:antialiased; }
  .wrap{ max-width:980px; margin:18px auto; padding:18px; }
  header{ display:flex; gap:12px; align-items:center; }
  h1{ margin:0; font-size:26px; }
  p.lead{ margin:4px 0 0; color:var(--muted); }
  .controls{ display:flex; gap:10px; align-items:center; margin:12px 0; flex-wrap:wrap; }
  .card{ background:var(--card); border-radius:12px; padding:14px; box-shadow: 0 6px 18px rgba(8,48,24,0.06); }
  .game-area{ margin-top:12px; }
  .hud{ display:flex; justify-content:space-between; align-items:center; gap:12px; }
  .left-hud{ display:flex; gap:12px; align-items:center; }
  .score{ font-size:22px; font-weight:700; }
  .lives{ display:flex; gap:6px; align-items:center; }
  .flower{ font-size:22px; transition:opacity .28s ease, transform .28s; }
  .flower.dead{ opacity:0.28; transform:scale(.92); filter:grayscale(100%); }
  .center{ text-align:center; }
  .grid{ display:grid; grid-template-columns: repeat(3, 1fr); gap:12px; margin-top:14px; }
  .cell{ background: #f4fff7; border-radius:12px; height:110px; display:flex; align-items:center; justify-content:center; font-size:22px; font-weight:700; cursor:pointer; user-select:none; transition:transform .12s, box-shadow .12s; box-shadow: 0 6px 14px rgba(11,30,15,0.04); padding:8px; text-align:center; }
  .cell:active{ transform:scale(.98); }
  .cell.active{ background:var(--accent); color:white; box-shadow:0 8px 24px rgba(11,30,15,0.08); }
  .prompt{ margin-top:12px; font-size:18px; }
  .controls .btn{ padding:10px 14px; border-radius:10px; border:none; cursor:pointer; font-size:16px; }
  .btn.primary{ background:var(--accent); color:white; }
  .btn.secondary{ background:#9ccaa6; color:var(--text); }
  .btn.ghost{ background:transparent; border:2px solid var(--accent-2); color:var(--accent-2); }
  .small{ font-size:13px; color:var(--muted); }
  input[type=number]{ width:72px; padding:8px; border-radius:8px; border:1px solid #d1d5db; }
  input[type=text]{ padding:10px; border-radius:8px; border:1px solid #d1d5db; width:92%; max-width:520px; font-size:16px; }
  .footer{ margin-top:12px; font-size:13px; color:var(--muted); }
  .summary{ margin-top:12px; font-size:15px; }
  .sr-only{ position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); white-space:nowrap; border:0; }
  @media (max-width:640px){ .grid{ grid-template-columns:repeat(2,1fr);} .cell{ height:88px; } }
</style>
</head>
<body>
<div class="wrap card" role="application" aria-labelledby="title">
  <header>
    <div>
      <h1 id="title">El Jardín de los Recuerdos 🌳</h1>
      <p class="lead">Ayuda al jardinero recordando secuencias. Diseñado para memoria de trabajo visual en adultos mayores.</p>
    </div>
    <div style="margin-left:auto; text-align:right;">
      <div class="small">Versión autocontenida</div>
      <div id="date" class="small"></div>
    </div>
  </header>

  <section style="margin-top:12px;">
    <div class="controls">
      <label class="small">Ronda:
        <select id="roundSel" aria-label="Seleccionar ronda">
          <option value="1">Regar las flores</option>
          <option value="2">Cosechar frutas</option>
          <option value="3">Juntar la cosecha</option>
        </select>
      </label>

      <label class="small">Bloques:
        <input id="blocks" type="number" min="1" max="10" value="5" aria-label="Bloques" />
      </label>

      <label class="small">Nivel inicial:
        <input id="level" type="number" min="1" max="8" value="2" aria-label="Nivel inicial" />
      </label>

      <label class="small"><input id="soundsToggle" type="checkbox" checked /> Sonidos</label>
      <label class="small"><input id="ttsToggle" type="checkbox" /> Instrucciones habladas</label>
      <label class="small"><input id="largeUI" type="checkbox" /> UI grande</label>

      <div style="margin-left:auto; display:flex; gap:8px;">
        <button id="startBtn" class="btn primary">Iniciar sesión</button>
        <button id="practiceBtn" class="btn secondary">Práctica</button>
        <button id="stopBtn" class="btn ghost">Detener</button>
      </div>
    </div>

    <div class="card game-area">
      <div class="hud">
        <div class="left-hud">
          <div class="score" aria-live="polite">Puntos: <span id="score">0</span></div>
          <div style="margin-left:12px;" class="small">Nivel: <span id="levelShow">2</span></div>
        </div>
        <div class="lives" aria-live="polite" aria-label="Vidas">
          <div class="small">Flores:</div>
          <div id="life1" class="flower">🌸</div>
          <div id="life2" class="flower">🌸</div>
          <div id="life3" class="flower">🌸</div>
        </div>
      </div>

      <div class="center prompt" id="status">Presiona <strong>Iniciar sesión</strong> para comenzar. Usa <em>Práctica</em> para probar sin afectar puntaje.</div>

      <div id="gridWrap" class="center" aria-live="polite">
        <div id="grid" class="grid" role="grid" aria-label="Jardín interactivo"></div>
      </div>

      <div class="center" style="margin-top:12px;">
        <div><input id="textResp" type="text" placeholder="(Opcional) Escribe la secuencia, ej: 1 3 2" /></div>
        <div style="margin-top:8px;"><button id="submitResp" class="btn primary">Enviar</button></div>
      </div>

      <div id="summary" class="summary"></div>
    </div>

    <div class="footer">Este prototipo genera sonidos directamente en el navegador; no necesitas archivos de audio. Usa <strong>Práctica</strong> antes de iniciar para familiarizarte.</div>
  </section>
</div>

<script>
/* =========================
   Web Audio helper (tones)
   ========================= */
let audioCtx = null;
function ensureAudioCtx(){
  if(!audioCtx){
    try{
      audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    }catch(e){
      audioCtx = null;
    }
  }
  return audioCtx;
}

// Play a simple tone with ADSR envelope
function playTone({freq=440, type='sine', dur=0.18, vol=0.08, attack=0.01, release=0.08, detune=0} = {}){
  if(!soundsToggle.checked) return;
  const ctx = ensureAudioCtx();
  if(!ctx) return;
  // ensure resumed on user gesture (some browsers)
  if(ctx.state === 'suspended') ctx.resume();
  const o = ctx.createOscillator();
  const g = ctx.createGain();
  o.type = type;
  o.frequency.value = freq;
  o.detune.value = detune;
  g.gain.value = 0;
  o.connect(g); g.connect(ctx.destination);
  const now = ctx.currentTime;
  // envelope
  g.gain.cancelScheduledValues(now);
  g.gain.setValueAtTime(0, now);
  g.gain.linearRampToValueAtTime(vol, now + attack);
  g.gain.linearRampToValueAtTime(0.0001, now + dur - release);
  o.start(now);
  o.stop(now + dur + 0.02);
}

// Small melody: sequence of tones with slight delay
async function playMelody(tones){
  for(const t of tones){
    playTone(t);
    await new Promise(r => setTimeout(r, Math.max(50, (t.dur || 0.16)*800)));
  }
}

/* Sound mappings:
   - water (regar): gentle descending pair
   - fruit (cosechar): bright short beep
   - veg (verdura): plucky lower tone
   - success: 3 ascending chirps
   - error: single soft low tone
   - ding: small bright bell
*/
function sndWater(){
  // descending: 620 -> 480
  playTone({freq:620, type:'sine', dur:0.12, vol:0.06, attack:0.005, release:0.06});
  setTimeout(()=> playTone({freq:480, type:'sine', dur:0.12, vol:0.05, attack:0.005, release:0.06}), 120);
}
function sndFruit(){
  playTone({freq:880, type:'sine', dur:0.16, vol:0.08, attack:0.005, release:0.06});
}
function sndVeg(){
  playTone({freq:380, type:'sine', dur:0.18, vol:0.07, attack:0.002, release:0.08});
}
function sndSuccess(){
  // three ascending chirps
  playMelody([
    {freq:720, dur:0.12, vol:0.07},
    {freq:820, dur:0.12, vol:0.07},
    {freq:920, dur:0.18, vol:0.08}
  ]);
}
function sndError(){
  playTone({freq:220, type:'sine', dur:0.28, vol:0.06, attack:0.01, release:0.12});
}
function sndDing(){
  playTone({freq:1080, type:'sine', dur:0.12, vol:0.06, attack:0.001, release:0.05});
}

/* =========================
   Game UI & logic
   ========================= */
const dateEl = document.getElementById('date');
dateEl.textContent = new Date().toLocaleString();

const gridEl = document.getElementById('grid');
const statusEl = document.getElementById('status');
const startBtn = document.getElementById('startBtn');
const practiceBtn = document.getElementById('practiceBtn');
const stopBtn = document.getElementById('stopBtn');
const roundSel = document.getElementById('roundSel');
const blocksInput = document.getElementById('blocks');
const levelInput = document.getElementById('level');
const levelShow = document.getElementById('levelShow');
const scoreEl = document.getElementById('score');
const submitResp = document.getElementById('submitResp');
const textResp = document.getElementById('textResp');
const summaryEl = document.getElementById('summary');
const soundsToggle = document.getElementById('soundsToggle');
const ttsToggle = document.getElementById('ttsToggle');
const largeUI = document.getElementById('largeUI');
const lifeEls = [document.getElementById('life1'), document.getElementById('life2'), document.getElementById('life3')];

const CELL_COUNT = 9;
let cells = [];
let sequence = [];
let userResponse = [];
let waitingForResponse = false;
let listening = false;
let currentBlock = 0;
let totalBlocks = 5;
let level = 2;
let score = 0;
let lives = 3;
let isPractice = false;
let results = [];
let currentRound = 1; // 1 flowers, 2 fruits, 3 mix

// content icons
const flowers = ['🌷','🌼','🌺','🌸','🌻','💐','🌹','🥀','⚘'];
const fruits = ['🍎','🍐','🍊','🍋','🍓','🍇','🍒','🥭','🍑'];
const veggies = ['🥕','🍆','🌽','🥔','🌶️','🧄','🧅','🥦','🍄'];

function buildGrid(){
  gridEl.innerHTML = ''; cells = [];
  for(let i=0;i<CELL_COUNT;i++){
    const c = document.createElement('div');
    c.className = 'cell';
    c.dataset.idx = i;
    c.tabIndex = 0;
    c.innerHTML = cellInnerHTML(i);
    c.addEventListener('click', ()=> onCellClick(i));
    gridEl.appendChild(c);
    cells.push(c);
  }
}
function cellInnerHTML(i){
  if(currentRound === 1) return `${flowers[i % flowers.length]}<div class="small">Maceta ${i+1}</div>`;
  if(currentRound === 2) return `${fruits[i % fruits.length]}<div class="small">Árbol ${i+1}</div>`;
  // mix: alternate fruit/veg by index
  const pool = [...fruits, ...veggies];
  return `${pool[i % pool.length]}<div class="small">Parcela ${i+1}</div>`;
}

buildGrid();

largeUI.addEventListener('change', ()=>{ document.body.style.fontSize = largeUI.checked ? '18px' : ''; });

function randInt(max){ return Math.floor(Math.random()*max); }
function genRoundSequence(len, round){
  // positions (allow repeats)
  const seq = [];
  for(let i=0;i<len;i++) seq.push(randInt(CELL_COUNT));
  return seq;
}

function flashCell(index, ms=600){
  return new Promise(resolve=>{
    const el = cells[index];
    if(!el){ resolve(); return; }
    el.classList.add('active');
    sndDing();
    setTimeout(()=>{ el.classList.remove('active'); setTimeout(resolve, 160); }, ms);
  });
}
async function playSequence(seq, pace=700){
  for(const idx of seq){
    await flashCell(idx, pace);
  }
}

function onCellClick(i){
  if(!listening || !waitingForResponse) return;
  userResponse.push(i);
  const el = cells[i];
  el.classList.add('active');
  setTimeout(()=> el.classList.remove('active'), 220);
  // play action sound based on round
  if(currentRound === 1) sndWater();
  else if(currentRound === 2) sndFruit();
  else sndVeg();
}

submitResp.addEventListener('click', ()=>{ if(waitingForResponse) evaluateResponse(); });
textResp.addEventListener('keydown', (e)=>{ if(e.key === 'Enter'){ e.preventDefault(); if(waitingForResponse) evaluateResponse(); } });

async function runBlock(){
  currentBlock++;
  statusEl.textContent = `Bloque ${currentBlock} / ${totalBlocks} — Nivel ${level}`;
  speak(`Bloque ${currentBlock} de ${totalBlocks}`);
  const len = Math.max(2, level + 1);
  sequence = genRoundSequence(len, currentRound);
  statusEl.textContent = `Observa la secuencia (${len} pasos)...`;
  await playSequence(sequence, Math.max(350, 920 - level*90));
  statusEl.textContent = `Repite la secuencia: haz clic en las casillas en orden o escribe los números separados por espacios`;
  userResponse = [];
  waitingForResponse = true;
  textResp.value = '';
  textResp.focus();
  const start = Date.now();
  while(waitingForResponse && (Date.now() - start) < 35000 && listening){
    await new Promise(r=>setTimeout(r,200));
  }
  if(waitingForResponse){ waitingForResponse = false; evaluateResponse(); }
}

function evaluateResponse(){
  waitingForResponse = false;
  const txt = textResp.value.trim();
  let parsed = [];
  if(txt.length > 0){
    parsed = txt.split(/\s+/).map(x => parseInt(x)-1).filter(x => !isNaN(x) && x >=0 && x < CELL_COUNT);
  } else parsed = userResponse.slice();

  // award +10 per correct item in right position
  let correctCount = 0;
  let madeError = false;
  for(let i=0;i<sequence.length;i++){
    if(i < parsed.length && parsed[i] === sequence[i]){
      correctCount++;
    } else {
      if(i < parsed.length) madeError = true; else if(parsed.length < sequence.length) madeError = true;
    }
  }
  const gained = correctCount * 10;
  score += gained;
  if(!madeError && parsed.length === sequence.length){
    score += 20; // bonus
    sndSuccess();
    statusEl.textContent = `¡Secuencia perfecta! +${gained + 20} puntos`;
  } else {
    if(gained > 0) sndSuccess();
    else sndError();
    statusEl.textContent = `Aciertos: ${correctCount} de ${sequence.length} — +${gained} puntos`;
  }
  scoreEl.textContent = score;

  if(!isPractice && madeError){
    loseLife();
  }

  results.push({round: currentRound, level, block: currentBlock, correct: correctCount, total: sequence.length, gained, perfect: !madeError && parsed.length===sequence.length});
  adaptLevel();
}

function loseLife(){
  lives = Math.max(0, lives - 1);
  updateLivesUI();
  sndError();
  if(lives <= 0){
    endSessionDueToLives();
  } else {
    speak(`Te quedan ${lives} flores`);
  }
}

function updateLivesUI(){
  for(let i=0;i<3;i++){
    if(i < lives) lifeEls[i].classList.remove('dead');
    else lifeEls[i].classList.add('dead');
  }
}

function endSessionDueToLives(){
  listening = false;
  waitingForResponse = false;
  statusEl.textContent = 'Has perdido tus 3 flores. El jardín necesita descansar — vuelve mañana 🌼';
  speak('Has perdido tus tres flores. Sesión finalizada.');
  finalizeSession();
}

function adaptLevel(){
  if(isPractice) return;
  const recent = results.slice(-2);
  if(recent.length < 2) return;
  const avgPercent = recent.reduce((s,r)=>s + (r.correct / r.total), 0) / recent.length;
  if(avgPercent >= 0.8){
    level = Math.min(8, level + 1);
    speak('Muy bien. Aumentando dificultad.');
  } else if(avgPercent <= 0.5){
    level = Math.max(1, level - 1);
    speak('Reduciendo la dificultad.');
  }
  levelInput.value = level;
  levelShow.textContent = level;
}

async function runSession(){
  // ensure audio context exists and is resumed on user gesture
  ensureAudioCtx();
  if(audioCtx && audioCtx.state === 'suspended') await audioCtx.resume();

  totalBlocks = parseInt(blocksInput.value) || 5;
  level = parseInt(levelInput.value) || 2;
  levelShow.textContent = level;
  currentBlock = 0;
  results = [];
  isPractice = false;
  listening = true;
  currentRound = parseInt(roundSel.value);
  score = 0;
  scoreEl.textContent = 0;
  lives = 3;
  updateLivesUI();
  startBtn.disabled = true;
  practiceBtn.disabled = true;
  stopBtn.disabled = false;
  statusEl.textContent = 'Iniciando sesión...';
  sndDing();
  speak('Iniciando sesión. Mucha suerte.');
  while(currentBlock < totalBlocks && listening && lives > 0){
    await runBlock();
    await new Promise(r=>setTimeout(r,700));
  }
  listening = false;
  startBtn.disabled = false;
  practiceBtn.disabled = false;
  stopBtn.disabled = true;
  finalizeSession();
}

async function practiceBlock(){
  ensureAudioCtx();
  if(audioCtx && audioCtx.state === 'suspended') await audioCtx.resume();
  isPractice = true;
  currentRound = parseInt(roundSel.value);
  level = parseInt(levelInput.value) || 2;
  levelShow.textContent = level;
  currentBlock = 0;
  listening = true;
  startBtn.disabled = true;
  practiceBtn.disabled = true;
  stopBtn.disabled = false;
  statusEl.textContent = 'Modo práctica';
  speak('Modo práctica. Puedes probar sin perder flores.');
  await runBlock();
  listening = false;
  startBtn.disabled = false;
  practiceBtn.disabled = false;
  stopBtn.disabled = true;
}

function finalizeSession(){
  let html = '<h3>Resumen de sesión</h3>';
  const byRound = {};
  results.forEach(r=>{
    const k = 'R' + r.round;
    if(!byRound[k]) byRound[k] = {sum:0, count:0};
    byRound[k].sum += (r.correct / r.total) * 100;
    byRound[k].count++;
  });
  for(const k in byRound){
    const avg = Math.round(byRound[k].sum / byRound[k].count);
    html += `<div>${k}: promedio ${avg}% (${byRound[k].count} bloque(s))</div>`;
  }
  html += `<div style="margin-top:8px;">Puntaje total: <strong>${score}</strong></div>`;
  summaryEl.innerHTML = html;
  saveSession();
  speak('Sesión finalizada. Buen trabajo.');
}

function saveSession(){
  try{
    const key = 'jardin_sesiones_wa_v1';
    const prev = JSON.parse(localStorage.getItem(key) || '[]');
    const rec = {date: new Date().toISOString(), round: currentRound, blocks: totalBlocks, level, score, lives, results};
    prev.push(rec);
    localStorage.setItem(key, JSON.stringify(prev.slice(-200)));
  }catch(e){}
}

/* UI wiring */
startBtn.addEventListener('click', ()=>{
  buildGrid();
  runSession();
});
practiceBtn.addEventListener('click', ()=>{
  buildGrid();
  practiceBlock();
});
stopBtn.addEventListener('click', ()=>{
  listening = false; waitingForResponse = false;
  startBtn.disabled = false; practiceBtn.disabled = false; stopBtn.disabled = true;
  statusEl.textContent = 'Detenido';
  speak('Sesión detenida.');
});
roundSel.addEventListener('change', ()=>{
  currentRound = parseInt(roundSel.value);
  buildGrid();
  statusEl.textContent = currentRound===1 ? 'Ronda: Regar las flores' : (currentRound===2 ? 'Ronda: Cosechar frutas' : 'Ronda: Juntar la cosecha');
});
document.addEventListener('keydown', (e)=>{ if(e.key === 'p') practiceBtn.click(); if(e.key === 's') startBtn.click(); });

levelShow.textContent = level;
updateLivesUI();

/* Accessibility tip: allow TTS for instructions */
function speak(text){
  if(!ttsToggle.checked) return;
  if('speechSynthesis' in window){
    const u = new SpeechSynthesisUtterance(text);
    u.rate = 0.95; speechSynthesis.cancel(); speechSynthesis.speak(u);
  }
}

/* Prevent accidental text selection while clicking many cells */
document.addEventListener('mousedown', e => e.preventDefault(), {passive:false});

</script>
</body>
</html>
