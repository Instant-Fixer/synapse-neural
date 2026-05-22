<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Synapse — Jarvis</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body { background: #000; overflow: hidden; font-family: 'Courier New', monospace; }
canvas { display: block; position: absolute; top: 0; left: 0; }

#api-modal {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.97); display: flex;
  align-items: center; justify-content: center; z-index: 200;
}
#api-box {
  border: 0.5px solid rgba(255,255,255,0.12);
  border-radius: 4px; padding: 36px 40px;
  max-width: 460px; width: 90%; text-align: center;
  background: rgba(5,5,5,0.98);
}
#api-box h2 {
  color: rgba(255,255,255,0.7); font-size: 13px;
  font-weight: 400; letter-spacing: 0.35em;
  text-transform: uppercase; margin-bottom: 4px;
}
#api-box .sub {
  color: rgba(255,255,255,0.2); font-size: 10px;
  margin-bottom: 28px; line-height: 1.9; letter-spacing: 0.05em;
}
#api-input, #airtable-input {
  width: 100%; background: rgba(255,255,255,0.03);
  border: 0.5px solid rgba(255,255,255,0.1);
  border-radius: 2px; padding: 11px 14px;
  color: rgba(255,255,255,0.7); font-size: 12px; outline: none;
  margin-bottom: 10px; font-family: 'Courier New', monospace;
}
#api-input::placeholder, #airtable-input::placeholder { color: rgba(255,255,255,0.15); }
#api-submit {
  background: transparent;
  border: 0.5px solid rgba(255,255,255,0.2);
  color: rgba(255,255,255,0.45); font-size: 11px;
  letter-spacing: 0.25em; padding: 10px 28px;
  border-radius: 2px; cursor: pointer; transition: all 0.3s;
  font-family: 'Courier New', monospace; margin-top: 4px;
}
#api-submit:hover { border-color: rgba(255,255,255,0.6); color: #fff; }

/* Wake screen */
#wake-screen {
  display: none; position: fixed; top: 0; left: 0;
  width: 100%; height: 100%; background: rgba(0,0,0,0.0);
  z-index: 150; align-items: center; justify-content: center;
  flex-direction: column; gap: 20px;
}
#wake-screen.show { display: flex; }
#wake-text {
  font-size: 11px; color: rgba(255,255,255,0.2);
  letter-spacing: 0.4em; text-transform: uppercase;
  animation: pulse-wake 3s ease infinite;
}
@keyframes pulse-wake { 0%,100%{opacity:0.2} 50%{opacity:0.6} }
#wake-input {
  background: transparent; border: none;
  border-bottom: 0.5px solid rgba(255,255,255,0.15);
  color: rgba(255,255,255,0.5); font-size: 13px;
  font-family: 'Courier New', monospace; outline: none;
  letter-spacing: 0.2em; text-align: center; width: 300px;
  padding: 8px 0;
}
#wake-input::placeholder { color: rgba(255,255,255,0.12); }

/* Topbar */
#topbar {
  position: absolute; top: 0; left: 0; width: 100%;
  padding: 13px 22px; display: flex;
  justify-content: space-between; align-items: center;
  z-index: 10; pointer-events: none;
  border-bottom: 0.5px solid rgba(255,255,255,0.04);
}
#logo { font-size: 10px; color: rgba(255,255,255,0.25); letter-spacing: 0.35em; }
#status-bar { display: flex; align-items: center; gap: 10px; }
.sdot { width: 4px; height: 4px; border-radius: 50%; background: rgba(255,255,255,0.5); animation: blink 2s infinite; }
@keyframes blink{0%,100%{opacity:1}50%{opacity:0.15}}
#stext { font-size: 9px; color: rgba(255,255,255,0.2); letter-spacing: 0.2em; }
#vdots { display:flex;gap:3px;align-items:center;opacity:0;transition:opacity 0.3s; }
#vdots span { width:2px;height:9px;background:rgba(255,255,255,0.5);border-radius:1px;animation:vbar 0.5s ease infinite alternate; }
#vdots span:nth-child(2){animation-delay:0.1s}#vdots span:nth-child(3){animation-delay:0.2s}
@keyframes vbar{from{transform:scaleY(0.3)}to{transform:scaleY(1.5)}}

/* Stats left */
#stats-bar {
  position: absolute; top: 52px; left: 22px;
  display: flex; flex-direction: column; gap: 7px;
  z-index: 10; pointer-events: none;
}
.stat { display: flex; align-items: center; gap: 10px; }
.stat-label { font-size: 9px; color: rgba(255,255,255,0.2); letter-spacing: 0.12em; width: 75px; }
.stat-bar { height: 1px; background: rgba(255,255,255,0.07); width: 70px; }
.stat-fill { height: 100%; background: rgba(255,255,255,0.4); transition: width 1.2s ease; }
.stat-num { font-size: 10px; color: rgba(255,255,255,0.4); width: 18px; }

/* Ticker right */
#ticker {
  position: absolute; right: 0; top: 42px;
  width: 270px; height: calc(100% - 90px);
  overflow: hidden; z-index: 10; pointer-events: none;
  border-left: 0.5px solid rgba(255,255,255,0.04);
  padding: 10px 14px;
}
#ticker-header { font-size: 9px; color: rgba(255,255,255,0.18); letter-spacing: 0.2em; margin-bottom: 10px; }
.tick-line { font-size: 10px; line-height: 2; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; animation: tick-in 0.3s ease; }
.tick-line.g { color: rgba(255,255,255,0.65); }
.tick-line.b { color: rgba(180,220,255,0.4); }
.tick-line.w { color: rgba(255,255,255,0.25); }
.tick-line.d { color: rgba(255,255,255,0.08); }
@keyframes tick-in{from{opacity:0;transform:translateX(6px)}to{opacity:1;transform:translateX(0)}}

/* AI bubble */
#ai-bubble {
  position: absolute; bottom: 65px; left: 22px;
  width: 300px; background: rgba(5,5,5,0.88);
  border: 0.5px solid rgba(255,255,255,0.1);
  border-radius: 4px; padding: 13px 15px;
  z-index: 10; pointer-events: none;
  opacity: 0; transition: opacity 0.5s;
}
.ai-label { font-size: 9px; color: rgba(255,255,255,0.25); letter-spacing: 0.2em; margin-bottom: 7px; }
#ai-text { font-size: 11px; color: rgba(255,255,255,0.65); line-height: 1.8; }

/* Controls */
#controls {
  position: absolute; bottom: 14px; right: 22px;
  display: flex; gap: 8px; z-index: 10;
}
.ctrl {
  background: transparent;
  border: 0.5px solid rgba(255,255,255,0.1);
  color: rgba(255,255,255,0.3); font-size: 9px;
  letter-spacing: 0.12em; padding: 6px 13px;
  border-radius: 2px; cursor: pointer; transition: all 0.3s;
  font-family: 'Courier New', monospace;
}
.ctrl:hover { border-color: rgba(255,255,255,0.4); color: rgba(255,255,255,0.7); }
.ctrl.active { border-color: rgba(255,255,255,0.35); color: rgba(255,255,255,0.6); }

/* Fullscreen overlay */
#fs-overlay {
  display: none; position: fixed; top: 0; left: 0;
  width: 100%; height: 100%; background: rgba(0,0,0,0.97);
  z-index: 100; overflow-y: auto; padding: 40px 36px;
}
#fs-overlay.show { display: block; }
#fs-header { font-size: 10px; color: rgba(255,255,255,0.25); letter-spacing: 0.3em; margin-bottom: 20px; }
.fs-lead { border: 0.5px solid rgba(255,255,255,0.07); border-radius: 2px; padding: 14px; margin-bottom: 10px; }
.fs-name { font-size: 12px; color: rgba(255,255,255,0.7); margin-bottom: 6px; }
.fs-detail { font-size: 10px; color: rgba(255,255,255,0.3); line-height: 1.9; }
.fs-outreach { font-size: 10px; color: rgba(180,220,255,0.45); margin-top: 8px; line-height: 1.7; }
#fs-close {
  position: fixed; top: 18px; right: 22px;
  background: transparent; border: 0.5px solid rgba(255,255,255,0.15);
  color: rgba(255,255,255,0.4); font-size: 9px; letter-spacing: 0.15em;
  padding: 6px 13px; border-radius: 2px; cursor: pointer;
  font-family: 'Courier New', monospace;
}
</style>
</head>
<body>
<canvas id="c"></canvas>

<!-- Credentials modal -->
<div id="api-modal">
  <div id="api-box">
    <h2>⬡ SYNAPSE JARVIS</h2>
    <p class="sub">// initialise credentials — session only, never stored<br>// say "wake up jarvis" after setup to activate</p>
    <input id="api-input" type="password" placeholder="// anthropic api key  sk-ant-..." />
    <input id="airtable-input" type="password" placeholder="// airtable token  pat..." />
    <button id="api-submit">INITIALISE()</button>
  </div>
</div>

<!-- Wake screen -->
<div id="wake-screen" class="show">
  <div id="wake-text">say or type to activate</div>
  <input id="wake-input" placeholder="wake up jarvis" autocomplete="off" spellcheck="false" />
</div>

<!-- UI -->
<div id="topbar">
  <div id="logo">SYNAPSE // JARVIS // PIPELINE MONITOR</div>
  <div id="status-bar">
    <div id="vdots"><span></span><span></span><span></span></div>
    <div class="sdot"></div>
    <div id="stext">STANDBY</div>
  </div>
</div>

<div id="stats-bar">
  <div class="stat"><div class="stat-label">TOTAL</div><div class="stat-bar"><div class="stat-fill" id="bar-total" style="width:0%"></div></div><div class="stat-num" id="num-total">0</div></div>
  <div class="stat"><div class="stat-label">CONTACTED</div><div class="stat-bar"><div class="stat-fill" id="bar-contacted" style="width:0%"></div></div><div class="stat-num" id="num-contacted">0</div></div>
  <div class="stat"><div class="stat-label">FOLLOW-UP</div><div class="stat-bar"><div class="stat-fill" id="bar-followup" style="width:0%"></div></div><div class="stat-num" id="num-followup">0</div></div>
  <div class="stat"><div class="stat-label">COLD</div><div class="stat-bar"><div class="stat-fill" id="bar-cold" style="width:0%"></div></div><div class="stat-num" id="num-cold">0</div></div>
  <div class="stat"><div class="stat-label">CLIENTS</div><div class="stat-bar"><div class="stat-fill" id="bar-clients" style="width:0%"></div></div><div class="stat-num" id="num-clients">0</div></div>
</div>

<div id="ticker">
  <div id="ticker-header">// LIVE_FEED</div>
  <div id="ticker-feed"></div>
</div>

<div id="ai-bubble">
  <div class="ai-label">// JARVIS.analyse()</div>
  <div id="ai-text"></div>
</div>

<div id="controls">
  <button class="ctrl" id="voice-btn">VOICE_ON</button>
  <button class="ctrl" id="fs-btn">FULLSCREEN</button>
  <button class="ctrl" id="reset-btn">RESET</button>
</div>

<div id="fs-overlay">
  <button id="fs-close">CLOSE</button>
  <div id="fs-header">// SYNAPSE — FULL PIPELINE</div>
  <div id="fs-content"></div>
</div>

<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
window.addEventListener('resize', () => { canvas.width = window.innerWidth; canvas.height = window.innerHeight; initParticles(); });

const W = () => canvas.width, H = () => canvas.height;
const FONT = 12;
const CHARS = 'SYNAPSE JARVIS LEADS QUALIFY OUTREACH AIRTABLE GROQ INSTAGRAM SCRAPE FILTER EMAIL SLACK INVOICE ONBOARD NAIROBI AI AUTOMATION 0110 1001 0011 1100 ><=>{}[]'.split('').filter(c=>c.trim());

// ── Irregular particles — moving in ALL directions ─────────────
let particles = [];

function initParticles() {
  particles = [];
  const count = Math.floor((W() * H()) / 3500);
  for (let i = 0; i < count; i++) spawnParticle(true);
}

function spawnParticle(random) {
  const angle = Math.random() * Math.PI * 2; // any direction
  const speed = 0.3 + Math.random() * 1.8;
  const len = 6 + Math.floor(Math.random() * 18);
  particles.push({
    x: random ? Math.random() * W() : (Math.random() < 0.5 ? -10 : W() + 10),
    y: random ? Math.random() * H() : Math.random() * H(),
    vx: Math.cos(angle) * speed,
    vy: Math.sin(angle) * speed,
    angle,
    speed,
    len,
    chars: Array.from({length: len}, () => CHARS[Math.floor(Math.random() * CHARS.length)]),
    alpha: 0.15 + Math.random() * 0.55,
    bright: Math.random() < 0.04,
    dataMode: false,
    dataText: '',
    dataTimer: 0,
    twistSpeed: (Math.random() - 0.5) * 0.04,
    wanderTimer: 60 + Math.floor(Math.random() * 120),
  });
}

initParticles();

let leads = [];

function injectLeadData() {
  if (leads.length === 0) return;
  const l = leads[Math.floor(Math.random() * leads.length)];
  const f = l.fields;
  const name = (f.Name || f['IG Handle'] || 'UNKNOWN').toUpperCase();
  const niche = (f.Niche || f['Business Type'] || '???').toUpperCase();
  const p = particles[Math.floor(Math.random() * particles.length)];
  p.dataMode = true;
  p.dataText = (name + ' ' + niche + ' SYNAPSE_AI ').repeat(3);
  p.dataTimer = 160;
  p.bright = true;
  p.alpha = 0.9;
}

setInterval(injectLeadData, 1800);
setInterval(() => {
  if (Math.random() < 0.15) spawnParticle(false);
  if (particles.length < Math.floor((W() * H()) / 3500)) spawnParticle(false);
}, 200);

function drawParticles() {
  ctx.fillStyle = 'rgba(0,0,0,0.07)';
  ctx.fillRect(0, 0, W(), H());

  particles.forEach((p, idx) => {
    // Wander — change direction occasionally
    p.wanderTimer--;
    if (p.wanderTimer <= 0) {
      p.angle += (Math.random() - 0.5) * 1.2;
      p.vx = Math.cos(p.angle) * p.speed;
      p.vy = Math.sin(p.angle) * p.speed;
      p.wanderTimer = 60 + Math.floor(Math.random() * 100);
    }

    // Twist direction slightly each frame
    p.angle += p.twistSpeed;
    p.vx = Math.cos(p.angle) * p.speed;
    p.vy = Math.sin(p.angle) * p.speed;

    // Random char shuffle
    if (Math.random() < 0.04) {
      const i = Math.floor(Math.random() * p.chars.length);
      p.chars[i] = CHARS[Math.floor(Math.random() * CHARS.length)];
    }

    // Draw trail in direction of movement
    for (let i = 0; i < p.len; i++) {
      const tx = p.x - Math.cos(p.angle) * i * FONT * 0.85;
      const ty = p.y - Math.sin(p.angle) * i * FONT * 0.85;
      if (tx < -20 || tx > W() + 20 || ty < -20 || ty > H() + 20) continue;

      let char = p.chars[i % p.chars.length];
      let alpha, color;

      if (p.dataMode && p.dataTimer > 0) {
        char = p.dataText[i % p.dataText.length] || char;
        if (i === 0) {
          alpha = 1;
          ctx.shadowColor = 'rgba(255,255,255,0.9)'; ctx.shadowBlur = 14;
          color = `rgba(255,255,255,1)`;
        } else {
          alpha = Math.max(0, 0.8 - i * 0.045);
          color = `rgba(220,235,255,${alpha})`;
          ctx.shadowBlur = 0;
        }
      } else if (p.bright) {
        if (i === 0) {
          alpha = 1; color = 'rgba(255,255,255,1)';
          ctx.shadowColor = 'rgba(255,255,255,0.7)'; ctx.shadowBlur = 10;
        } else {
          alpha = Math.max(0, 0.7 - i * 0.05);
          color = `rgba(255,255,255,${alpha})`;
          ctx.shadowBlur = 0;
        }
      } else {
        alpha = Math.max(0, p.alpha - i * 0.04);
        if (i === 0) {
          color = `rgba(255,255,255,${Math.min(alpha + 0.2, 0.8)})`;
        } else {
          color = `rgba(200,210,230,${alpha * 0.6})`;
        }
        ctx.shadowBlur = 0;
      }

      ctx.fillStyle = color;
      ctx.font = `${FONT}px 'Courier New', monospace`;
      ctx.fillText(char, tx, ty);
      ctx.shadowBlur = 0;
    }

    // Move
    p.x += p.vx;
    p.y += p.vy;

    // Wrap
    if (p.x < -30) p.x = W() + 30;
    if (p.x > W() + 30) p.x = -30;
    if (p.y < -30) p.y = H() + 30;
    if (p.y > H() + 30) p.y = -30;

    if (p.dataTimer > 0) p.dataTimer--;
    else if (p.dataMode) { p.dataMode = false; p.bright = false; }
  });
}

function animate() { drawParticles(); requestAnimationFrame(animate); }
animate();

// ── Credentials ───────────────────────────────────────────────
let anthropicKey = '', airtableToken = '', voiceEnabled = true, jarvisActive = false;

document.getElementById('api-submit').addEventListener('click', () => {
  const ak = document.getElementById('api-input').value.trim();
  const at = document.getElementById('airtable-input').value.trim();
  if (!ak || !at || ak.length < 10 || at.length < 10) {
    document.getElementById('api-input').style.borderColor = 'rgba(255,80,80,0.4)';
    return;
  }
  anthropicKey = ak; airtableToken = at;
  document.getElementById('api-modal').style.display = 'none';
  document.getElementById('wake-screen').classList.add('show');
  document.getElementById('wake-input').focus();
});

// Wake word
const wakeInput = document.getElementById('wake-input');
wakeInput.addEventListener('keydown', e => {
  if (e.key === 'Enter') checkWakeWord(wakeInput.value.trim().toLowerCase());
});
wakeInput.addEventListener('input', () => {
  if (wakeInput.value.trim().toLowerCase().includes('wake up jarvis')) {
    checkWakeWord('wake up jarvis');
  }
});

function checkWakeWord(val) {
  if (val.includes('wake up jarvis') || val.includes('jarvis') || val.includes('wake')) {
    document.getElementById('wake-screen').classList.remove('show');
    document.getElementById('stext').textContent = 'ONLINE';
    jarvisActive = true;
    initJarvis();
  }
}

// Voice recognition for wake word
if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  const rec = new SR();
  rec.continuous = true; rec.interimResults = true;
  rec.onresult = e => {
    const transcript = Array.from(e.results).map(r => r[0].transcript).join('').toLowerCase();
    if (!jarvisActive && transcript.includes('wake up jarvis')) checkWakeWord('wake up jarvis');
    if (jarvisActive && transcript.includes('jarvis')) {
      const last = transcript.split('jarvis').pop().trim();
      if (last.length > 3) handleVoiceCommand(last);
    }
  };
  rec.onerror = () => {};
  try { rec.start(); } catch(e) {}
}

async function handleVoiceCommand(cmd) {
  if (!anthropicKey) return;
  const res = await getAIComment(`User said to Jarvis: "${cmd}". Respond as Jarvis for Synapse AI Agency. Keep it under 2 sentences.`);
  if (res) { showAIBubble(res); speak(res); }
}

// Controls
document.getElementById('reset-btn').addEventListener('click', () => {
  anthropicKey = ''; airtableToken = ''; jarvisActive = false;
  document.getElementById('api-modal').style.display = 'flex';
  document.getElementById('stext').textContent = 'STANDBY';
});
document.getElementById('voice-btn').addEventListener('click', () => {
  voiceEnabled = !voiceEnabled;
  const btn = document.getElementById('voice-btn');
  btn.textContent = voiceEnabled ? 'VOICE_ON' : 'VOICE_OFF';
  btn.classList.toggle('active', voiceEnabled);
});
document.getElementById('fs-btn').addEventListener('click', () => {
  document.getElementById('fs-overlay').classList.add('show');
  renderFullscreen();
});
document.getElementById('fs-close').addEventListener('click', () => {
  document.getElementById('fs-overlay').classList.remove('show');
});

// Voice
function speak(text) {
  if (!voiceEnabled || !window.speechSynthesis) return;
  window.speechSynthesis.cancel();
  const utt = new SpeechSynthesisUtterance(text);
  utt.rate = 0.9; utt.pitch = 0.8; utt.volume = 1;
  const voices = window.speechSynthesis.getVoices();
  const preferred = voices.find(v => v.name.includes('Google UK English Male') || v.name.includes('Daniel') || v.name.includes('Alex'));
  if (preferred) utt.voice = preferred;
  utt.onstart = () => document.getElementById('vdots').style.opacity = '1';
  utt.onend = () => document.getElementById('vdots').style.opacity = '0';
  window.speechSynthesis.speak(utt);
}

// Claude
async function getAIComment(context) {
  if (!anthropicKey) return null;
  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': anthropicKey,
        'anthropic-version': '2023-06-01',
        'anthropic-dangerous-direct-browser-access': 'true'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 90,
        system: 'You are Jarvis, the AI of Synapse Agency. You work for James. Speak like a sharp, confident AI assistant. Be direct and specific. No emojis. No corporate filler. Max 2 sentences.',
        messages: [{ role: 'user', content: context }]
      })
    });
    const data = await res.json();
    return data.content?.[0]?.text || null;
  } catch (e) { return null; }
}

function showAIBubble(text) {
  document.getElementById('ai-text').textContent = text;
  document.getElementById('ai-bubble').style.opacity = '1';
  setTimeout(() => document.getElementById('ai-bubble').style.opacity = '0', 9000);
}

// Airtable
async function fetchLeads() {
  if (!airtableToken) return;
  try {
    const res = await fetch('https://api.airtable.com/v0/appi1Ty254ogelZOf/Leads%202?pageSize=50', {
      headers: { 'Authorization': 'Bearer ' + airtableToken }
    });
    const data = await res.json();
    if (data.records) { leads = data.records; updateStats(); updateTicker(); }
  } catch (e) {}
}

function updateStats() {
  const total = leads.length;
  const contacted = leads.filter(l => l.fields.status === 'Contacted').length;
  const followup = leads.filter(l => ['Follow-up 1','Follow-up 2'].includes(l.fields.status)).length;
  const cold = leads.filter(l => l.fields.status === 'Cold').length;
  const clients = leads.filter(l => ['Client','Onboarded'].includes(l.fields.status)).length;
  document.getElementById('num-total').textContent = total;
  document.getElementById('num-contacted').textContent = contacted;
  document.getElementById('num-followup').textContent = followup;
  document.getElementById('num-cold').textContent = cold;
  document.getElementById('num-clients').textContent = clients;
  const pct = n => total > 0 ? Math.min(100,(n/total)*100)+'%' : '0%';
  document.getElementById('bar-total').style.width = '100%';
  document.getElementById('bar-contacted').style.width = pct(contacted);
  document.getElementById('bar-followup').style.width = pct(followup);
  document.getElementById('bar-cold').style.width = pct(cold);
  document.getElementById('bar-clients').style.width = pct(clients);
}

function updateTicker() {
  const feed = document.getElementById('ticker-feed');
  feed.innerHTML = '';
  addTick('> FETCH() — ' + leads.length + ' records', 'g');
  addTick('──────────────────────', 'd');
  leads.slice(0, 22).forEach((l, i) => {
    const f = l.fields;
    const name = f.Name || f['IG Handle'] || 'UNKNOWN';
    const niche = f.Niche || f['Business Type'] || '???';
    const status = f.status || 'NEW';
    const outreach = (f['Outreach Msg Sent'] || '').slice(0, 42);
    addTick(`[${String(i+1).padStart(2,'0')}] ${name}`, 'g');
    addTick(`     ${niche} | ${status}`, 'b');
    if (outreach) addTick(`     "${outreach}"`, 'w');
    addTick('', 'd');
  });
}

function addTick(text, cls) {
  const div = document.createElement('div');
  div.className = 'tick-line ' + (cls||'');
  div.textContent = text;
  document.getElementById('ticker-feed').appendChild(div);
}

function renderFullscreen() {
  const c = document.getElementById('fs-content'); c.innerHTML = '';
  leads.forEach((l, i) => {
    const f = l.fields;
    const div = document.createElement('div'); div.className = 'fs-lead';
    div.innerHTML = `
      <div class="fs-name">[${String(i+1).padStart(2,'0')}] ${f.Name||f['IG Handle']||'UNKNOWN'}</div>
      <div class="fs-detail">NICHE: ${f.Niche||'—'} | STATUS: ${f.status||'NEW'}<br>PAIN: ${(f['Pain Point']||'—').slice(0,80)}<br>PROFILE: ${f.Website||'—'}</div>
      <div class="fs-outreach">> "${(f['Outreach Msg Sent']||'—').slice(0,120)}"</div>`;
    c.appendChild(div);
  });
}

async function aiLoop() {
  while (true) {
    await new Promise(r => setTimeout(r, 40000));
    if (!anthropicKey || leads.length === 0) continue;
    const total = leads.length;
    const cold = leads.filter(l => l.fields.status === 'Cold').length;
    const clients = leads.filter(l => ['Client','Onboarded'].includes(l.fields.status)).length;
    const comment = await getAIComment(`Pipeline: ${total} leads, ${cold} cold, ${clients} clients. Top niche: ${leads[0]?.fields?.Niche||'unknown'}. Analyse.`);
    if (comment) { showAIBubble(comment); speak(comment); }
  }
}

async function initJarvis() {
  speak('Jarvis online. Good to see you James. Synapse neural link established. Pulling live pipeline data now.');
  await fetchLeads();
  setInterval(fetchLeads, 30000);
  aiLoop();
  setTimeout(async () => {
    const comment = await getAIComment('Jarvis just came online. Greet James by name and give a sharp 1-2 sentence pipeline status based on the leads data you have access to.');
    if (comment) { showAIBubble(comment); speak(comment); }
  }, 5000);
}

window.speechSynthesis.getVoices();
</script>
</body>
</html>
