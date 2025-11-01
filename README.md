<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Mona’s Halloween Game — Multi-Round</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  :root{--bg:#0a0a0f;--panel:#12121a;--text:#f5f6fb;--accent:#ff2a2a;--orange:#ff7a00;--slime:#40ff8c;--line:rgba(255,255,255,.12)}
  *{box-sizing:border-box} html,body{height:100%}
  body{
    margin:0;background:
      radial-gradient(1000px 400px at 50% -10%, #191422, transparent 60%),
      radial-gradient(700px 300px at 120% 0%, #1b1330, transparent 60%),
      var(--bg);
    color:var(--text);font-family:Montserrat,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;line-height:1.5;overflow-x:hidden
  }
  /* Blood header */
  .bloodbar{position:relative;height:110px;background:linear-gradient(180deg,#7a000b,#3a0006);overflow:hidden;border-bottom:3px solid #2a0004}
  .drip{position:absolute;top:-90px;width:40px;height:220px;left:var(--x,0);background:var(--accent);border-radius:20px;filter:drop-shadow(0 4px 2px rgba(0,0,0,.5));animation:ooze var(--t,8s) linear infinite}
  .drip::after{content:"";position:absolute;bottom:-8px;left:50%;transform:translateX(-50%);width:20px;height:20px;background:var(--accent);border-radius:50%}
  @keyframes ooze{0%{transform:translateY(0)}60%{transform:translateY(55px)}100%{transform:translateY(0)}}
  .titlewrap{position:absolute;inset:0;display:grid;place-items:center;text-align:center;padding:0 10px}
  h1{margin:0;font-size:clamp(22px,4.6vw,44px);letter-spacing:.6px;text-shadow:0 2px 0 #2a0004}
  h1 span{color:var(--accent)}

  /* Atmospherics */
  .fog{position:fixed;inset:-10% -10% auto -10%;height:60vh;pointer-events:none;mix-blend-lighten;
    background:radial-gradient(60% 40% at 50% 50%, rgba(255,255,255,.06), transparent 70%),
               radial-gradient(50% 35% at 20% 60%, rgba(255,255,255,.05), transparent 70%),
               radial-gradient(55% 35% at 80% 45%, rgba(255,255,255,.05), transparent 70%);
    filter:blur(2px);animation:fogdrift 28s linear infinite}
  @keyframes fogdrift{0%{transform:translateX(0)}50%{transform:translateX(2%)}100%{transform:translateX(0)}}
  .bat{position:fixed;top:14vh;left:-12vw;width:50px;height:20px;transform:scale(.9);
    background:linear-gradient(90deg,#000 30%,transparent 0),linear-gradient(-90deg,#000 30%,transparent 0);
    background-size:50% 100%;background-repeat:no-repeat;border-radius:50%;filter:drop-shadow(0 0 6px rgba(0,0,0,.6));
    animation:fly 14s linear infinite, flap .4s ease-in-out infinite;opacity:.85}
  .bat::before,.bat::after{content:"";position:absolute;top:50%;width:14px;height:14px;background:#000;border-radius:50%;transform:translateY(-50%)}
  .bat::before{left:10px}.bat::after{right:10px}
  .bat.b2{top:26vh;animation-duration:18s;animation-delay:-4s;transform:scale(.7)}
  .bat.b3{top:38vh;animation-duration:22s;animation-delay:-7s;transform:scale(1.1)}
  @keyframes flap{0%,100%{transform:translateY(0)}50%{transform:translateY(-3px)}}
  @keyframes fly{0%{left:-12vw}100%{left:112vw}}

  .page{max-width:980px;margin:-20px auto 80px;padding:18px}
  .panel{background:linear-gradient(180deg,#151520,#0f0f16);border:1px solid var(--line);border-radius:16px;padding:18px;box-shadow:0 10px 28px rgba(0,0,0,.45)}

  /* Round selector */
  .rounds{display:flex;flex-wrap:wrap;gap:10px;margin:8px 0 12px}
  .chip{border:1px solid rgba(255,122,0,.5);color:var(--orange);border-radius:999px;padding:8px 12px;text-decoration:none;cursor:pointer}
  .chip[aria-current="true"]{background:rgba(255,122,0,.08)}

  .meta{display:flex;gap:14px;align-items:center;justify-content:space-between;flex-wrap:wrap}
  .timer{font-weight:800;color:var(--orange)}
  .hint{opacity:.9}

  .grid{display:grid;gap:14px;margin-top:14px}
  .qcard{background:#0e0e15;border:1px solid var(--line);border-radius:12px;padding:14px}
  .q{font-weight:700}
  .opts{display:grid;gap:8px;margin-top:8px}
  .opts label{display:flex;gap:10px;align-items:flex-start;background:#0b0b12;border:1px solid rgba(255,255,255,.08);padding:10px 12px;border-radius:10px;cursor:pointer}
  .opts input{margin-top:3px}
  .wrong{outline:2px solid #ff4d6d;border-radius:8px}

  .bar{display:flex;gap:10px;flex-wrap:wrap;align-items:center;margin-top:12px}
  .btn{appearance:none;border:none;border-radius:999px;padding:10px 16px;font-weight:800;cursor:pointer;color:#0b0b0f;background:linear-gradient(180deg,var(--slime), #1be173);box-shadow:0 6px 18px rgba(64,255,140,.25)}
  .btn.secondary{color:var(--orange);background:transparent;border:1px solid rgba(255,122,0,.5);box-shadow:none}
  .codebox{margin-top:16px;display:flex;gap:10px;justify-content:center;align-items:center;flex-wrap:wrap}
  .digit{width:56px;height:64px;display:grid;place-items:center;font-size:34px;font-weight:800;color:#fff;background:#0f0f15;border:2px solid rgba(255,255,255,.12);border-radius:10px;box-shadow:inset 0 0 18px rgba(0,0,0,.6)}
  .msg{margin-top:10px;font-weight:700}.success{color:var(--slime)}.fail{color:#ff4d6d}
  footer{margin-top:20px;padding-top:14px;border-top:1px solid var(--line);display:flex;justify-content:space-between;align-items:center;gap:10px}
  .sig{color:#bbb}
</style>
</head>
<body>
<!-- blood header -->
<div class="bloodbar" aria-hidden="true">
  <div class="titlewrap"><h1>Do You Want To <span>Play A Game?</span></h1></div>
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
    <div class="rounds" id="roundChips"></div>

    <div class="meta">
      <div class="hint">Answer 5/5 correctly before time runs out. Then we compute your secret 4-digit code.</div>
      <div class="timer" id="timer">⏳ 60s</div>
    </div>

    <div class="grid" id="quiz"></div>

    <div class="bar">
      <button class="btn" id="submitBtn">Check Answers & Reveal Code</button>
      <button class="btn secondary" id="resetBtn">Reset</button>
    </div>

    <div class="codebox" id="codeBox" style="display:none">
      <div class="digit" id="d1">•</div><div class="digit" id="d2">•</div><div class="digit" id="d3">•</div><div class="digit" id="d4">•</div>
    </div>
    <div class="msg" id="msg"></div>

    <footer>
      <div class="sig">Equation: <em>(Q1×Q2)(Q3×Q4) − Q5</em></div>
      <div class="sig">Mona’s Productivity Suite — Halloween Edition</div>
    </footer>
  </section>
</div>

<script>
/* =================== ROUNDS: edit movies & timers here =================== */
const ROUNDS = [
  {
    name: "Classic Horror",
    seconds: 60,
    questions: [
      { q:"In <em>Saw</em> (2004), what is the tricycle puppet’s name?", choices:["Jigsaw","Billy","Spiral","Patches"], correctIndex:1 },
      { q:"In <em>Halloween</em> (1978), Michael Myers’ mask was repainted from which character?", choices:["Jason Voorhees","William Shatner","Leatherface","Nosferatu"], correctIndex:1 },
      { q:"In <em>A Nightmare on Elm Street</em> (1984), what colors are Freddy’s sweater?", choices:["Red & green","Red & black","Green & black","Brown & red"], correctIndex:0 },
      { q:"In <em>Friday the 13th</em> (1980), who is the killer in the original film?", choices:["Jason Voorhees","Pamela Voorhees","A camp counselor","Unknown"], correctIndex:1 },
      { q:"In <em>The Shining</em> (1980), what do Jack’s pages say?", choices:["All work and no play...","REDRUM","Come play with us","Here’s Johnny!"], correctIndex:0 }
    ]
  },
  {
    name: "Modern Horror",
    seconds: 75,
    questions: [
      { q:"In <em>Get Out</em> (2017), what sends Chris to the Sunken Place?", choices:["A teacup stir","A phone flash","A doorbell chime","A metronome"], correctIndex:0 },
      { q:"In <em>It</em> (2017), what’s Pennywise’s favorite hangout?", choices:["Storm drains","Abandoned houses","Carnivals","Sewer plant"], correctIndex:0 },
      { q:"In <em>Hereditary</em> (2018), which symbol recurs throughout?", choices:["Triquetra","Tree of life","King Paimon’s sigil","Spiral"], correctIndex:2 },
      { q:"In <em>A Quiet Place</em> (2018), the monsters are most vulnerable to…", choices:["Fire","Bullets","High-frequency sound","Low temperatures"], correctIndex:2 },
      { q:"In <em>The Ring</em> (2002 US), how long after watching the tape do you die?", choices:["3 days","7 days","10 days","1 day"], correctIndex:1 }
    ]
  },
  {
    name: "Family Halloween",
    seconds: 60,
    questions: [
      { q:"In <em>Hocus Pocus</em> (1993), what’s the talking cat’s name?", choices:["Salem","Binx","Mittens","Shadow"], correctIndex:1 },
      { q:"In <em>The Nightmare Before Christmas</em>, Jack is the Pumpkin King of…", choices:["Spookyville","Halloweentown","Graveglade","Nightshade Hollow"], correctIndex:1 },
      { q:"In <em>Ghostbusters</em> (1984), what giant terrorizes New York?", choices:["Michelin Man","Stay-Puft Marshmallow Man","Candy Golem","Gumdrop Titan"], correctIndex:1 },
      { q:"In <em>Coraline</em> (2009), the Other Mother’s most notable feature is…", choices:["Button eyes","Needle teeth","Forked tongue","Hollow face"], correctIndex:0 },
      { q:"In <em>Casper</em> (1995), what’s Casper’s famous line?", choices:["I’m friendly!","Can I keep you?","Got boo?","Let’s play!"], correctIndex:1 }
    ]
  }
];

/* ======= LETTER→NUMBER MAPS (same across rounds; matches your poster) ======= */
const MAP1 = { A:3, B:5, C:1, D:9 };
const MAP2 = { A:7, B:6, C:2, D:4 };
const MAP3 = { A:3, B:8, C:6, D:1 };
const MAP4 = { A:4, B:9, C:4, D:7 }; // yes, C=4
const MAP5 = { A:5, B:8, C:9, D:2 };

const chips = document.getElementById('roundChips');
const quizEl = document.getElementById('quiz');
const timerEl = document.getElementById('timer');
const submitBtn = document.getElementById('submitBtn');
const resetBtn  = document.getElementById('resetBtn');
const msgEl     = document.getElementById('msg');
const codeBox   = document.getElementById('codeBox');
const d1 = document.getElementById('d1'), d2 = document.getElementById('d2'), d3 = document.getElementById('d3'), d4 = document.getElementById('d4');

let current = 0, time = 60, ticking = true, timerRef;

/* =================== RENDER ROUNDS + QUIZ =================== */
function renderChips(){
  chips.innerHTML = ROUNDS.map((r,i)=>`<a class="chip" data-i="${i}" ${i===current?'aria-current="true"':''}>${r.name}</a>`).join('');
  chips.querySelectorAll('.chip').forEach(ch=>{
    ch.addEventListener('click', e=>{
      e.preventDefault(); loadRound(+ch.dataset.i);
    });
  });
}
function renderQuiz(){
  const Q = ROUNDS[current].questions;
  quizEl.innerHTML = Q.map((it,qi)=>{
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
}
function resetUI(){
  msgEl.textContent = ''; msgEl.className = 'msg';
  codeBox.style.display = 'none';
  [d1.textContent,d2.textContent,d3.textContent,d4.textContent] = ['•','•','•','•'];
  document.querySelectorAll('.qcard').forEach(c=>c.classList.remove('wrong'));
  document.querySelectorAll('input[type="radio"]').forEach(r=>{ r.checked=false; r.disabled=false; });
  submitBtn.disabled = false;
}
function startTimer(){
  clearInterval(timerRef);
  time = ROUNDS[current].seconds; ticking = true;
  timerEl.textContent = `⏳ ${time}s`;
  timerRef = setInterval(()=>{
    if(!ticking) return;
    time--; timerEl.textContent = `⏳ ${time}s`;
    if(time<=0){ ticking=false; lockQuiz(); showMessage("Time’s up. Reset or pick another round.", true); }
  },1000);
}
function loadRound(i){
  current = i; renderChips(); renderQuiz(); resetUI(); startTimer();
}

/* =================== EVALUATION / CODE MATH =================== */
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
  submitBtn.disabled = true;
}
function showMessage(text, isError){
  msgEl.textContent = text; msgEl.className = 'msg ' + (isError ? 'fail' : 'success');
}

/* =================== BUTTONS =================== */
submitBtn.addEventListener('click', ()=>{
  if(!ticking){ showMessage("Time’s up. Reset or pick another round.", true); return; }
  const Q = ROUNDS[current].questions;
  document.querySelectorAll('.qcard').forEach(c=>c.classList.remove('wrong'));

  const letters = [];
  for(let i=0;i<Q.length;i++){
    const sel = getSelectedIndex(i);
    if(sel<0){ document.getElementById(`card-${i}`).classList.add('wrong'); showMessage("Answer all questions.", true); return; }
    if(sel !== Q[i].correctIndex){ document.getElementById(`card-${i}`).classList.add('wrong'); showMessage("One or more answers are incorrect.", true); return; }
    letters.push(letterFromIndex(sel));
  }
  // compute code
  const v1 = mapValue(0, letters[0]);
  const v2 = mapValue(1, letters[1]);
  const v3 = mapValue(2, letters[2]);
  const v4 = mapValue(3, letters[3]);
  const v5 = mapValue(4, letters[4]);
  const p12 = v1*v2, p34 = v3*v4, concat = parseInt(`${p12}${p34}`,10), code = concat - v5;
  const codeStr = code.toString().padStart(4,'0').slice(-4);
  [d1.textContent,d2.textContent,d3.textContent,d4.textContent] = codeStr.split('');
  codeBox.style.display='flex';
  showMessage(`Code generated. Nice work.`, false);
  ticking = false; lockQuiz(); playChime();
});

resetBtn.addEventListener('click', ()=>{ resetUI(); startTimer(); });

/* =================== WebAudio chime (no hosting) =================== */
let AC = window.AudioContext || window.webkitAudioContext, ctx=null, masterGain=null;
function ensureCtx(){ if(ctx) return; ctx = new AC(); masterGain = ctx.createGain(); masterGain.gain.value=0.9; masterGain.connect(ctx.destination); }
function onFirstGesture(){ ensureCtx(); window.removeEventListener('pointerdown', onFirstGesture); }
window.addEventListener('pointerdown', onFirstGesture, {once:true, passive:true});
function playChime(){ ensureCtx(); const t0=ctx.currentTime;
  const o1=ctx.createOscillator(), o2=ctx.createOscillator(), g=ctx.createGain();
  o1.type='sine'; o1.frequency.value=880;  o1.detune.value=-4;
  o2.type='triangle'; o2.frequency.value=1320; o2.detune.value=+3;
  g.gain.setValueAtTime(0.0001,t0); g.gain.exponentialRampToValueAtTime(0.9,t0+0.01); g.gain.exponentialRampToValueAtTime(0.001,t0+1.2);
  o1.connect(g); o2.connect(g); g.connect(masterGain); o1.start(t0); o2.start(t0); o1.stop(t0+1.3); o2.stop(t0+1.3);
}

/* =================== INIT =================== */
renderChips(); loadRound(0);
</script>
</body>
</html>