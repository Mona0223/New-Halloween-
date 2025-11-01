<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Do You Want To Play A Game?</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0a0a0f; --panel:#12121a; --text:#f5f6fb;
    --accent:#ff2a2a;       /* blood red */
    --orange:#ff7a00;       /* candle orange */
    --slime:#40ff8c;        /* success green */
    --line:rgba(255,255,255,.12);
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0; background:
      radial-gradient(1000px 400px at 50% -10%, #191422, transparent 60%),
      radial-gradient(700px 300px at 120% 0%, #1b1330, transparent 60%),
      var(--bg);
    font-family:Montserrat,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;
    color:var(--text); line-height:1.5; overflow-x:hidden;
  }
  /* Blood header */
  .bloodbar{
    position:relative; height:110px; background:linear-gradient(180deg, #7a000b, #3a0006);
    overflow:hidden; border-bottom:3px solid #2a0004;
  }
  .drip{
    position:absolute; top:-90px; width:40px; height:220px; left:var(--x,0);
    background:var(--accent); border-radius:20px;
    filter:drop-shadow(0 4px 2px rgba(0,0,0,.5));
    animation:ooze var(--t,8s) linear infinite;
  }
  .drip::after{
    content:""; position:absolute; bottom:-8px; left:50%; transform:translateX(-50%);
    width:20px; height:20px; background:var(--accent); border-radius:50%;
  }
  @keyframes ooze{
    0%{transform:translateY(0)}
    60%{transform:translateY(55px)}
    100%{transform:translateY(0)}
  }
  .titlewrap{position:absolute; inset:0; display:grid; place-items:center; text-align:center; padding:0 10px}
  h1{margin:0; font-size:clamp(22px,4.8vw,44px); letter-spacing:.6px; text-shadow:0 2px 0 #2a0004}
  h1 span{color:var(--accent)}

  /* Atmospherics */
  .fog{position:fixed; inset:-10% -10% auto -10%; height:60vh; pointer-events:none; mix-blend-lighten;
    background:radial-gradient(60% 40% at 50% 50%, rgba(255,255,255,.06), transparent 70%),
               radial-gradient(50% 35% at 20% 60%, rgba(255,255,255,.05), transparent 70%),
               radial-gradient(55% 35% at 80% 45%, rgba(255,255,255,.05), transparent 70%);
    filter:blur(2px); animation:fogdrift 28s linear infinite;}
  @keyframes fogdrift{0%{transform:translateX(0)}50%{transform:translateX(2%)}100%{transform:translateX(0)}}
  .bat{
    position:fixed; top:14vh; left:-12vw; width:50px; height:20px; transform:scale(.9);
    background:linear-gradient(90deg,#000 30%,transparent 0),linear-gradient(-90deg,#000 30%,transparent 0);
    background-size:50% 100%; background-repeat:no-repeat; border-radius:50%;
    filter:drop-shadow(0 0 6px rgba(0,0,0,.6));
    animation:fly 14s linear infinite, flap .4s ease-in-out infinite; opacity:.85;
  }
  .bat::before,.bat::after{content:""; position:absolute; top:50%; width:14px; height:14px; background:#000; border-radius:50%; transform:translateY(-50%)}
  .bat::before{left:10px} .bat::after{right:10px}
  .bat.b2{top:26vh; animation-duration:18s; animation-delay:-4s; transform:scale(.7)}
  .bat.b3{top:38vh; animation-duration:22s; animation-delay:-7s; transform:scale(1.1)}
  @keyframes flap{0%,100%{transform:translateY(0)}50%{transform:translateY(-3px)}}
  @keyframes fly{0%{left:-12vw}100%{left:112vw}}

  .page{max-width:960px; margin:-20px auto 80px; padding:18px}
  .panel{
    background:linear-gradient(180deg,#151520,#0f0f16); border:1px solid var(--line);
    border-radius:16px; padding:18px; box-shadow:0 10px 28px rgba(0,0,0,.45);
  }
  .meta{display:flex; gap:14px; align-items:center; justify-content:space-between; flex-wrap:wrap}
  .timer{font-weight:800; color:var(--orange)}
  .hint{opacity:.9}
  .grid{display:grid; gap:14px; margin-top:14px}

  .qcard{background:#0e0e15; border:1px solid var(--line); border-radius:12px; padding:14px}
  .q{font-weight:700}
  .opts{display:grid; gap:8px; margin-top:8px}
  .opts label{
    display:flex; gap:10px; align-items:flex-start; background:#0b0b12;
    border:1px solid rgba(255,255,255,.08); padding:10px 12px; border-radius:10px; cursor:pointer;
  }
  .opts input{margin-top:3px}
  .wrong{outline:2px solid #ff4d6d; border-radius:8px}

  .bar{display:flex; gap:10px; flex-wrap:wrap; align-items:center; margin-top:12px}
  .btn{
    appearance:none; border:none; border-radius:999px; padding:10px 16px; font-weight:800; cursor:pointer;
    color:#0b0b0f; background:linear-gradient(180deg,var(--slime), #1be173); box-shadow:0 6px 18px rgba(64,255,140,.25);
  }
  .btn.secondary{color:var(--orange); background:transparent; border:1px solid rgba(255,122,0,.5); box-shadow:none}

  .codebox{
    margin-top:16px; display:flex; gap:10px; justify-content:center; align-items:center; flex-wrap:wrap;
  }
  .digit{
    width:56px; height:64px; display:grid; place-items:center; font-size:34px; font-weight:800; color:#fff;
    background:#0f0f15; border:2px solid rgba(255,255,255,.12); border-radius:10px; box-shadow:inset 0 0 18px rgba(0,0,0,.6);
  }
  .msg{margin-top:10px; font-weight:700}
  .success{color:var(--slime)}
  .fail{color:#ff4d6d}

  footer{margin-top:20px; padding-top:14px; border-top:1px solid var(--line); display:flex; justify-content:space-between; align-items:center; gap:10px}
  .sig{color:#bbb}
</style>
</head>
<body>
<!-- blood header -->
<div class="bloodbar" aria-hidden="true">
  <div class="titlewrap"><h1>Do You Want To <span>Play A Game?</span></h1></div>
  <!-- a few drips -->
  <div class="drip" style="--x:6%; --t:7.5s"></div>
  <div class="drip" style="--x:18%; --t:9s"></div>
  <div class="drip" style="--x:33%; --t:8.2s"></div>
  <div class="drip" style="--x:61%; --t:8.6s"></div>
  <div class="drip" style="--x:79%; --t:7.9s"></div>
</div>

<!-- atmospherics -->
<div class="fog" aria-hidden="true"></div>
<div class="bat" aria-hidden="true"></div>
<div class="bat b2" aria-hidden="true"></div>
<div class="bat b3" aria-hidden="true"></div>

<div class="page">
  <section class="panel">
    <div class="meta">
      <div class="hint">You have <strong>60 seconds</strong> to answer 5 questions correctly. Then we’ll compute your secret 4-digit code.</div>
      <div class="timer" id="timer">⏳ 60s</div>
    </div>

    <div class="grid" id="quiz"></div>

    <div class="bar">
      <button class="btn" id="submitBtn">Check Answers & Reveal Code</button>
      <button class="btn secondary" id="resetBtn">Reset</button>
    </div>

    <div class="codebox" id="codeBox" style="display:none">
      <div class="digit" id="d1">•</div>
      <div class="digit" id="d2">•</div>
      <div class="digit" id="d3">•</div>
      <div class="digit" id="d4">•</div>
    </div>
    <div class="msg" id="msg"></div>

    <footer>
      <div class="sig">Equation: <em>(Q1×Q2)(Q3×Q4) − Q5</em></div>
      <div class="sig">Mona’s Productivity Suite — Halloween Edition</div>
    </footer>
  </section>
</div>

<script>
/* ===================== QUESTIONS (edit text if you like) ===================== */
const QUESTIONS = [
  {
    q: "In <em>Halloween</em> (1978), what was Michael Myers’ mask originally made from?",
    choices: ["A blank mannequin mold","A William Shatner mask","A clown mask","A custom latex cast"],
    correctIndex: 1  // B
  },
  {
    q: "In <em>The Shining</em> (1980), what reads on Jack’s pages?",
    choices: ["“All work and no play makes Jack a dull boy”","“REDRUM” repeated","“Come play with us”","“Here’s Johnny!” on loop"],
    correctIndex: 0  // A
  },
  {
    q: "In <em>The Exorcist</em> (1973), what’s the possessed child’s name?",
    choices: ["Carrie","Regan","Carol Anne","Samara"],
    correctIndex: 1  // B
  },
  {
    q: "In <em>Scream</em> (1996), the caller asks about which movie?",
    choices: ["A Nightmare on Elm Street","Friday the 13th","Halloween","Psycho"],
    correctIndex: 0  // A
  },
  {
    q: "In <em>The Nightmare Before Christmas</em> (1993), Jack is the Pumpkin King of…",
    choices: ["Spookyville","Halloweentown","Graveglade","Nightshade Hollow"],
    correctIndex: 1  // B
  }
];

/* ============= LETTER→NUMBER MAPS (must match your poster) ============= */
/* Q1 */ const MAP1 = { A:3, B:5, C:1, D:9 };
/* Q2 */ const MAP2 = { A:7, B:6, C:2, D:4 };
/* Q3 */ const MAP3 = { A:3, B:8, C:6, D:1 };
/* Q4 */ const MAP4 = { A:4, B:9, C:4, D:7 };  // yes, C=4 as shown
/* Q5 */ const MAP5 = { A:5, B:8, C:9, D:2 };

/* ============================ RENDER QUIZ ============================ */
const quizEl = document.getElementById('quiz');
quizEl.innerHTML = QUESTIONS.map((it,qi)=>{
  const letters = ['A','B','C','D'];
  const opts = it.choices.map((txt,i)=>`
    <label>
      <input type="radio" name="q${qi}" value="${i}">
      <span><strong>${letters[i]}.</strong> ${txt}</span>
    </label>
  `).join('');
  return `
    <div class="qcard" id="card-${qi}">
      <div class="q">${qi+1}) ${it.q}</div>
      <div class="opts" id="opts-${qi}">${opts}</div>
    </div>`;
}).join('');

/* ============================ TIMER ============================ */
const timerEl = document.getElementById('timer');
let time = 60, ticking = true;
const t = setInterval(()=>{
  if(!ticking) return;
  time--; timerEl.textContent = `⏳ ${time}s`;
  if(time<=0){ ticking=false; lockQuiz(); showMessage("Time’s up. Reset and try again.", true); }
}, 1000);

/* ============================ HELPERS ============================ */
function getSelectedIndex(qi){
  const sel = document.querySelector(`input[name="q${qi}"]:checked`);
  return sel ? +sel.value : -1;
}
function letterFromIndex(i){ return ['A','B','C','D'][i] || '?'; }
function mapValue(qIndex, letter){
  switch(qIndex){
    case 0: return MAP1[letter];
    case 1: return MAP2[letter];
    case 2: return MAP3[letter];
    case 3: return MAP4[letter];
    case 4: return MAP5[letter];
    default: return 0;
  }
}
function lockQuiz(){
  document.querySelectorAll('input[type="radio"]').forEach(r=>r.disabled=true);
  document.getElementById('submitBtn').disabled = true;
}
function unlockQuiz(){
  document.querySelectorAll('input[type="radio"]').forEach(r=>r.disabled=false);
  document.getElementById('submitBtn').disabled = false;
}

/* ============================ EVALUATION ============================ */
const submitBtn = document.getElementById('submitBtn');
const resetBtn  = document.getElementById('resetBtn');
const msgEl     = document.getElementById('msg');
const codeBox   = document.getElementById('codeBox');
const d1 = document.getElementById('d1');
const d2 = document.getElementById('d2');
const d3 = document.getElementById('d3');
const d4 = document.getElementById('d4');

submitBtn.addEventListener('click', ()=>{
  if(!ticking){ showMessage("Time’s up. Reset to try again.", true); return; }

  // clear old highlights
  document.querySelectorAll('.qcard').forEach(c=>c.classList.remove('wrong'));

  // Check correctness
  const letters = [];
  for(let i=0;i<QUESTIONS.length;i++){
    const sel = getSelectedIndex(i);
    if(sel<0){ highlight(i); showMessage("Answer all questions.", true); return; }
    const isCorrect = (sel === QUESTIONS[i].correctIndex);
    if(!isCorrect){ highlight(i); showMessage("One or more answers are incorrect.", true); return; }
    letters.push(letterFromIndex(sel));
  }

  // All correct → compute code
  const v1 = mapValue(0, letters[0]);
  const v2 = mapValue(1, letters[1]);
  const v3 = mapValue(2, letters[2]);
  const v4 = mapValue(3, letters[3]);
  const v5 = mapValue(4, letters[4]);

  const p12 = v1 * v2;              // first product
  const p34 = v3 * v4;              // second product
  const concat = parseInt(`${p12}${p34}`, 10);
  const code = concat - v5;         // final 4-digit code

  const codeStr = code.toString().padStart(4,'0').slice(-4);
  [d1.textContent, d2.textContent, d3.textContent, d4.textContent] = codeStr.split('');
  codeBox.style.display = 'flex';
  showMessage(`Code generated. Nice work.`, false);
  playChime(); // success sfx
  ticking=false; // stop the clock once solved
  lockQuiz();
});

resetBtn.addEventListener('click', ()=>{
  // reset UI
  document.querySelectorAll('input[type="radio"]').forEach(r=>{ r.checked=false; r.disabled=false; });
  document.querySelectorAll('.qcard').forEach(c=>c.classList.remove('wrong'));
  msgEl.textContent = ''; codeBox.style.display='none';
  [d1.textContent,d2.textContent,d3.textContent,d4.textContent] = ['•','•','•','•'];
  // reset timer
  time = 60; ticking = true; timerEl.textContent = `⏳ ${time}s`;
  submitBtn.disabled = false;
});

function highlight(i){
  document.getElementById(`card-${i}`).classList.add('wrong');
}
function showMessage(text, isError){
  msgEl.textContent = text;
  msgEl.className = 'msg ' + (isError ? 'fail' : 'success');
}

/* ============================ WebAudio CHIME ============================ */
let AC = window.AudioContext || window.webkitAudioContext, ctx=null, masterGain=null;
function ensureCtx(){
  if(ctx) return; ctx = new AC();
  masterGain = ctx.createGain(); masterGain.gain.value = 0.9; masterGain.connect(ctx.destination);
}
function onFirstGesture(){ ensureCtx(); window.removeEventListener('pointerdown', onFirstGesture); }
window.addEventListener('pointerdown', onFirstGesture, {once:true, passive:true});

function playChime(){
  ensureCtx(); const t0 = ctx.currentTime;
  const o1 = ctx.createOscillator(), o2 = ctx.createOscillator(), g = ctx.createGain();
  o1.type='sine'; o1.frequency.value=880;  o1.detune.value=-4;
  o2.type='triangle'; o2.frequency.value=1320; o2.detune.value=+3;
  g.gain.setValueAtTime(0.0001, t0);
  g.gain.exponentialRampToValueAtTime(0.9, t0+0.01);
  g.gain.exponentialRampToValueAtTime(0.001, t0+1.2);
  o1.connect(g); o2.connect(g); g.connect(masterGain);
  o1.start(t0); o2.start(t0); o1.stop(t0+1.3); o2.stop(t0+1.3);
}
</script>
</body>
</html>