<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Gerbera — I love you</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Inter:wght@300;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg1:#fff2f6;
    --bg2:#ffeef2;
    --gerbera:#ff6f98;
    --center:#ffcede;
    --leaf:#9acb8f;
    --text:#5b2b38;
  }
  *{box-sizing:border-box}
  html,body{height:100%}
  body{
    margin:0;
    font-family:Inter,system-ui,Arial,sans-serif;
    background: radial-gradient(600px 300px at 10% 10%, #fff6fb 0%, var(--bg1) 20%), linear-gradient(180deg,var(--bg2), #fff);
    overflow:hidden;
    color:var(--text);
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
  }

  /* Container */
  .stage{
    position:relative;
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    gap:20px;
    padding:48px 20px;
    flex-direction:column;
  }

  h1{font-family:"Playfair Display",serif;margin:0;font-size:32px;color:#7a2a40}
  p.lead{margin:8px 0 24px;color:#7a5166;max-width:760px;text-align:center}

  /* Gerbera (SVG wrapper) */
  .flower-wrap{
    width:240px;height:240px;display:grid;place-items:center;
    cursor:pointer;filter:drop-shadow(0 12px 28px rgba(139,69,93,0.12));
    transition:transform .18s ease;
  }
  .flower-wrap:active{transform:scale(.98)}
  .flower{
    width:220px;height:220px;display:block;
  }

  /* controls */
  .controls{display:flex;gap:10px;align-items:center}
  .btn{
    background:linear-gradient(180deg,var(--gerbera),#ff4d7a);
    border:none;color:white;padding:10px 14px;border-radius:12px;font-weight:700;cursor:pointer;
    box-shadow:0 8px 20px rgba(255,80,125,0.12);
  }
  .btn.secondary{background:transparent;color:var(--gerbera);border:2px solid rgba(255,111,163,0.12)}

  /* floating "I love you" messages */
  .floating {
    position:fixed;left:50%;top:50%;pointer-events:none;white-space:nowrap;
    transform:translate(-50%,-50%) scale(1);
    font-family:"Playfair Display",serif;
    color:var(--gerbera);
    text-shadow:0 2px 8px rgba(255,111,163,0.14);
    will-change: transform, opacity;
  }

  /* Styles for different sizes/weights */
  .f-small{font-size:14px;opacity:.95}
  .f-medium{font-size:20px;opacity:0.98}
  .f-large{font-size:34px;opacity:1;font-weight:700}
  .f-italic{font-style:italic;color:#ff3b6b}

  /* petal particle */
  .petal {
    position:fixed;left:0;top:0;pointer-events:none;z-index:1;
    width:18px;height:28px;opacity:0.95;will-change:transform,opacity;
    transform-origin:center top;
    filter:drop-shadow(0 8px 18px rgba(255,50,110,0.06));
  }

  /* subtle ground vignette */
  .vignette{
    position:fixed;left:0;right:0;bottom:0;height:28vh;background:linear-gradient(180deg,transparent,rgba(255,210,220,0.35) 50%,rgba(255,211,226,0.6));
    pointer-events:none;
  }

  /* small footer */
  .credits{position:fixed;left:12px;bottom:10px;font-size:13px;color:#9e6b7a}

  /* Responsive */
  @media (max-width:520px){
    .flower-wrap{width:160px;height:160px}
    .flower{width:140px;height:140px}
    h1{font-size:22px}
  }
</style>
</head>
<body>
  <div class="stage" id="stage">
    <h1>Gerbera Garden of Love</h1>
    <p class="lead">Click the gerbera or "Bloom more" to send floating "I love you" notes and let petals fall — a gentle shower of love.</p>

    <div class="flower-wrap" id="flower" title="Click to bloom!">
      <!-- Gerbera SVG (stylized) -->
      <svg class="flower" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
        <defs>
          <radialGradient id="petalGrad" cx="30%" cy="30%">
            <stop offset="0%" stop-color="#ffd7e6"/>
            <stop offset="40%" stop-color="#ff9ac0"/>
            <stop offset="100%" stop-color="#ff6f98"/>
          </radialGradient>
          <radialGradient id="centerGrad">
            <stop offset="0%" stop-color="#fff2e6"/>
            <stop offset="60%" stop-color="#ffdfd8"/>
            <stop offset="100%" stop-color="#ffb6c1"/>
          </radialGradient>
        </defs>

        <!-- Multiple petals rotated -->
        <g transform="translate(100,100)">
          <!-- generate 16 petals -->
          <g id="petals">
            <!-- Creating petals with path -->
            <!-- We'll create petals via JS for variation; keep a base petal to copy -->
            <path id="basePetal" d="M0,-10 C20,-40 60,-40 80,-10 C60,10 20,10 0,-10 Z" fill="url(#petalGrad)" transform="scale(.9) rotate(0) translate(-40,0)" opacity="0.98"/>
          </g>

          <!-- center -->
          <circle cx="0" cy="0" r="22" fill="url(#centerGrad)" stroke="#ff9aa8" stroke-width="2"/>
          <circle cx="0" cy="0" r="9" fill="#ff4d7a"/>
        </g>
      </svg>
    </div>

    <div class="controls">
      <button class="btn" id="bloomBtn">Bloom more</button>
      <button class="btn secondary" id="stopBtn">Stop petals</button>
    </div>
  </div>

  <div class="vignette"></div>
  <div class="credits">Click flower • Bloom more • Enjoy</div>

<script>
/* Gerbera + Petals + "I love you" floating messages
   - Clicking the flower or "Bloom more" spawns petals and messages.
   - Messages float upward and fade; petals fall with random rotation.
*/

// utilities
function rand(min, max){ return Math.random()*(max-min)+min; }
function pick(arr){ return arr[Math.floor(Math.random()*arr.length)]; }

// grab elements
const stage = document.getElementById('stage');
const flower = document.getElementById('flower');
const bloomBtn = document.getElementById('bloomBtn');
const stopBtn = document.getElementById('stopBtn');

let petalsInterval = null;
let messagesInterval = null;
let petalsEnabled = true;

// list of "i love you" variants (different casing, languages, hearts)
const lovePhrases = [
  "I love you", "i love you", "I ♥ you", "I ❤️ you", "I lоve you", // some visual variants
  "Te amo", "Je t'aime", "Ich liebe dich", "Ti amo", "愛してる",
  "I love you so much", "ILY", "Love you", "I love you 💖", "i love you 💕"
];

// create a falling petal element (SVG)
function createPetal(xStart){
  const ns = "http://www.w3.org/2000/svg";
  const svg = document.createElementNS(ns, "svg");
  svg.setAttribute("class","petal");
  svg.setAttribute("width","24");
  svg.setAttribute("height","28");
  svg.setAttribute("viewBox","0 0 24 28");
  svg.style.left = xStart + "px";
  svg.style.top = "-40px";
  svg.style.zIndex = 1;

  // petal path shape
  const path = document.createElementNS(ns, "path");
  path.setAttribute("d", "M12 1 C18 1 22 8 18 14 C14 20 12 26 12 26 C12 26 10 20 6 14 C2 8 6 1 12 1 Z");
  path.setAttribute("fill", `rgba(255, ${Math.floor(rand(90,160))}, ${Math.floor(rand(120,170))}, ${rand(0.85,1)})`);
  path.setAttribute("transform", `rotate(${rand(-30,30)} 12 14)`);

  svg.appendChild(path);
  document.body.appendChild(svg);

  // animate using JS + CSS transforms
  const duration = rand(4200,9000);
  const endX = xStart + rand(-120,120);
  const rotate = rand(200,1200) * (Math.random()<0.5 ? -1 : 1);
  const sway = rand(40,140);

  const start = performance.now();
  function anim(now){
    const t = (now - start)/duration;
    if(t >= 1){
      svg.remove();
      return;
    }
    // y goes from -40 to window.innerHeight + 60
    const y = -40 + (window.innerHeight + 100) * t;
    const x = xStart + Math.sin(t*Math.PI*2) * sway + (endX - xStart) * t;
    const rot = rotate * t;
    const scale = 0.9 + 0.2*Math.sin(t*Math.PI);
    svg.style.transform = `translate(${x}px, ${y}px) rotate(${rot}deg) scale(${scale})`;
    svg.style.opacity = String(1 - Math.pow(t, 1.6));
    requestAnimationFrame(anim);
  }
  requestAnimationFrame(anim);
}

// spawn a floating "I love you" message near center/top
function spawnLove(x=null,y=null){
  const el = document.createElement('div');
  el.className = 'floating ' + pick(['f-small','f-medium','f-large','f-italic']);
  const phrase = pick(lovePhrases);
  el.textContent = phrase;
  // initial position: random near center or provided coords
  const cx = x === null ? (window.innerWidth/2 + rand(-120,120)) : x;
  const cy = y === null ? (window.innerHeight/2 + rand(-40,40)) : y;
  el.style.left = cx + 'px';
  el.style.top = cy + 'px';
  el.style.transform = `translate(-50%,-50%) scale(${rand(0.9,1.2)})`;
  document.body.appendChild(el);

  // animate upward + fade
  const duration = rand(2800,5200);
  const start = performance.now();
  const driftX = rand(-200,200);
  function step(now){
    const t = (now - start)/duration;
    if(t >= 1){
      el.remove();
      return;
    }
    const ease = 1 - Math.pow(1-t, 3); // ease-out
    const newY = cy - (200 + rand(20,160)) * ease;
    const newX = cx + driftX * ease;
    el.style.left = newX + 'px';
    el.style.top = newY + 'px';
    el.style.opacity = String(1 - ease);
    el.style.transform = `translate(-50%,-50%) scale(${1 - 0.12*ease})`;
    requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}

// bloom action: spawn multiple petals and messages
function bloom(x=null,y=null,count=10){
  // spawn petals across width near center
  for(let i=0;i<count;i++){
    const startX = (x === null) ? (window.innerWidth/2 + rand(-200,200)) : x + rand(-40,40);
    if(petalsEnabled) createPetal(startX);
  }
  // spawn love messages
  const loveCount = Math.max(2, Math.floor(count/3));
  for(let j=0;j<loveCount;j++){
    const px = (x === null) ? (window.innerWidth/2 + rand(-80,80)) : x + rand(-20,20);
    const py = (y === null) ? (window.innerHeight/2 + rand(-40,40)) : y + rand(-10,10);
    spawnLove(px,py);
  }
}

// continuous gentle petals (background)
function startPetalRain(){
  if(petalsInterval) return;
  petalsInterval = setInterval(()=>{
    const x = rand(0, window.innerWidth);
    if(petalsEnabled) createPetal(x);
    // small chance of a single love message
    if(Math.random() < 0.12) spawnLove(rand(0,window.innerWidth), rand(0,window.innerHeight/2));
  }, 420);
}
function stopPetalRain(){
  clearInterval(petalsInterval); petalsInterval = null;
}

// click handlers
flower.addEventListener('click', (e)=>{
  // get center position of flower for localized spawn
  const rect = flower.getBoundingClientRect();
  const cx = rect.left + rect.width/2;
  const cy = rect.top + rect.height/2;
  // big bloom
  bloom(cx, cy, 18);
  // playful pulse animation on SVG
  flower.animate([{transform:'scale(1)'},{transform:'scale(1.06)'},{transform:'scale(1)'}], {duration:420, easing:'cubic-bezier(.2,.9,.3,1)'});
});

bloomBtn.addEventListener('click', ()=> bloom(null,null,14));
stopBtn.addEventListener('click', ()=>{
  petalsEnabled = !petalsEnabled;
  stopBtn.textContent = petalsEnabled ? 'Stop petals' : 'Resume petals';
  if(petalsEnabled) startPetalRain(); else stopPetalRain();
});

// start gentle rain by default
startPetalRain();

// accessibility: tap anywhere on stage to also bloom a little
stage.addEventListener('click', (e)=>{
  if(e.target.closest('.flower-wrap') || e.target.closest('.controls')) return;
  bloom(e.clientX, e.clientY, 8);
});

// cleanup on resize to avoid stray elements building too many
window.addEventListener('resize', ()=> {
  // no heavy cleanup; existing particles will naturally finish
});

// optional: pre-populate a gentle wall of "I love you"
for(let i=0;i<6;i++){
  setTimeout(()=> spawnLove(rand(window.innerWidth*0.25, window.innerWidth*0.75), rand(window.innerHeight*0.25, window.innerHeight*0.6)), i*420);
}
</script>
</body>
</html>
