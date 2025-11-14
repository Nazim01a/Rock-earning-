<ROCK>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Rock Earn Pro — Ultimate Final</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{
    --bg1:#0b0f1a; --bg2:#0f1730; --glass: rgba(255,255,255,0.04);
    --accent1:#5d5fef; --accent2:#00ffe0; --neon:#00ffe0;
  }
  html,body{height:100%;margin:0;font-family:'Segoe UI',sans-serif;background:linear-gradient(160deg,var(--bg1),var(--bg2));color:#eef2ff;}
  /* particles */
  #particles{position:fixed;inset:0;pointer-events:none;z-index:0;overflow:hidden}
  .particle{position:absolute;background:var(--neon);border-radius:50%;opacity:.08;box-shadow:0 0 12px rgba(0,255,224,.2);}

  /* layout */
  .app {position:relative;z-index:10;display:flex;min-height:100vh;}
  .sidebar {width:220px;padding:20px;background:rgba(10,12,25,0.7);backdrop-filter:blur(8px);box-shadow:0 10px 30px rgba(0,0,0,.6);}
  .main {flex:1;padding:28px;overflow:auto;}
  .logo{font-weight:800;font-size:20px;background:linear-gradient(90deg,var(--accent1),var(--accent2));-webkit-background-clip:text;color:transparent;margin-bottom:18px}

  /* cards */
  .card {background:var(--glass);border-radius:14px;padding:16px;margin-bottom:16px;border:1px solid rgba(255,255,255,0.04);box-shadow:0 10px 30px rgba(3,7,18,0.6)}
  .plan{transition:all .28s;border-radius:14px;padding:16px;border:1px solid rgba(255,255,255,0.03);background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent)}
  .plan:hover{transform:translateY(-6px);box-shadow:0 10px 40px rgba(45,60,120,0.18),0 0 30px rgba(0,255,224,0.06);border-color:rgba(0,255,224,0.08)}

  .btn {display:inline-block;padding:10px 14px;border-radius:10px;font-weight:700;color:#001;background:linear-gradient(90deg,var(--accent1),var(--accent2));cursor:pointer;border:none}
  .btn-soft{background:transparent;color:var(--neon);border:1px solid rgba(0,255,224,0.08)}
  .muted{color:rgba(230,230,255,0.6);font-size:14px}

  /* side panel */
  #sidePanel{position:fixed;top:0;right:-420px;width:400px;height:100vh;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));backdrop-filter:blur(12px);padding:20px;transition:right .35s;z-index:60;overflow:auto;border-left:1px solid rgba(255,255,255,0.03)}
  .close-x{float:right;color:rgba(200,220,255,0.7);cursor:pointer}

  /* small */
  .top-row{display:flex;gap:12px;align-items:center;justify-content:space-between;margin-bottom:12px}
  .stats{display:flex;gap:12px}
  .stat{background:linear-gradient(90deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.03)}
  input,select{background:rgba(255,255,255,0.03);border:1px solid rgba(255,255,255,0.02);padding:10px;border-radius:10px;color:inherit;width:100%}
  .small{font-size:13px;color:rgba(230,230,255,0.8)}
  .badge{background:linear-gradient(90deg,var(--accent1),var(--accent2));color:#001;padding:4px 8px;border-radius:999px;font-weight:700;font-size:12px}

  /* responsive */
  @media(max-width:900px){.app{flex-direction:column}.sidebar{width:100%;display:flex;overflow:auto}.main{padding:16px}#sidePanel{width:100%;right:-100%}}
</style>
</head>
<body>

<!-- particles -->
<div id="particles"></div>

<div class="app">
  <!-- SIDEBAR -->
  <aside class="sidebar card" role="navigation">
    <div class="logo">🚀 Rock Earn Pro</div>
    <div class="small muted">Since 2018 • Crypto FinTech</div>
    <hr class="my-3" />
    <button class="btn w-full mb-2" onclick="showSection('plans')">Plans</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('deposit')">Deposit</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('withdraw')">Withdraw</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('profit')">Daily Profit</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('history')">Activity</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('profile')">Profile</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('company')">Company</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('share')">Share</button>
    <button class="btn-soft w-full mb-2" onclick="showSection('settings')">Settings</button>
    <div style="flex:1"></div>
    <div class="small muted text-center">Made with ❤️ — Rock Earn Pro</div>
  </aside>

  <!-- MAIN -->
  <main class="main">
    <!-- AUTH: show if not logged in -->
    <div id="authBox" class="card" style="max-width:760px;margin:auto">
      <div class="top-row">
        <div>
          <h2 class="text-2xl font-bold">Login / Sign Up</h2>
          <div class="muted small">Create account or login to access full dashboard</div>
        </div>
        <div id="authWelcome" class="small muted"></div>
      </div>

      <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px">
        <input id="authName" placeholder="Full name (for sign up)" />
        <input id="authEmail" placeholder="Email" />
        <input id="authPass" type="password" placeholder="Password" />
        <div>
          <button class="btn w-full" onclick="signupUser()">Sign Up</button>
        </div>
      </div>

      <div style="display:flex;gap:8px;margin-top:10px">
        <button class="btn" style="background:linear-gradient(90deg,#34d399,#10b981)" onclick="loginUser()">Login</button>
        <button class="btn-soft" onclick="forgotPassword()">Forgot password</button>
      </div>
    </div>

    <!-- WELCOME (after login) -->
    <div id="welcomeBox" class="card hidden" style="max-width:760px;margin:auto">
      <div style="display:flex;align-items:center;justify-content:space-between">
        <div>
          <h2 id="welcomeName" class="text-2xl font-bold">Welcome, User</h2>
          <div class="muted small">Account: <span id="accountEmail">-</span></div>
        </div>
        <div style="text-align:right">
          <div class="badge" id="balanceBadge">₨ 0</div>
          <div style="margin-top:8px">
            <button class="btn-soft" onclick="copyProfileLink()">Copy Profile</button>
            <button class="btn" style="background:linear-gradient(90deg,#ff7a7a,#ff4d4d)" onclick="logoutUser()">Logout</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Plans grid -->
    <section id="plansSection" class="card" style="max-width:1200px;margin:18px auto">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
        <div>
          <h2 class="text-2xl font-bold">Investment Plans</h2>
          <div class="muted small">Choose a plan and click Buy — deposit panel appears from right.</div>
        </div>
        <div class="stats">
          <div class="stat small"><div class="muted">Plans</div><div><b id="planCount">0</b></div></div>
          <div class="stat small"><div class="muted">Balance</div><div><b id="topBalance">₨ 0</b></div></div>
        </div>
      </div>

      <div id="plansGrid" style="display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:14px"></div>
    </section>

    <!-- Side panels area (opened via JS) -->
    <div id="sidePanel"></div>

    <!-- FOOTER small -->
    <div style="max-width:1200px;margin:10px auto;text-align:center" class="muted small">Questions? support@rockearnpro.com — Headquarters: 1234 Crypto Avenue, San Francisco, CA</div>
  </main>
</div>

<!-- sound -->
<audio id="clickSound" src="https://freesound.org/data/previews/256/256113_3263906-lq.mp3" preload="auto"></audio>

<script>
/* ---------- Utilities & Demo Data ---------- */
function playClick(){ try{document.getElementById('clickSound').currentTime=0; document.getElementById('clickSound').play()}catch(e){} }
function rand(min,max){return Math.floor(Math.random()*(max-min+1))+min}
function createParticles(n=60){
  const cont=document.getElementById('particles');
  for(let i=0;i<n;i++){
    const p=document.createElement('div'); p.className='particle';
    const size=rand(4,16); p.style.width=size+'px'; p.style.height=size+'px';
    p.style.left=rand(0,100)+'%'; p.style.top=rand(0,100)+'%'; p.style.opacity=(Math.random()*0.12).toString();
    cont.appendChild(p);
    const dur=rand(18,40); p.animate([{transform:`translateY(0) translateX(0)`},{transform:`translateY(-1200px) translateX(${rand(-200,200)}px)`}],{duration:dur*1000,iterations:Infinity});
  }
}
createParticles(80);

/* ---------- App State (localStorage) ---------- */
let users = JSON.parse(localStorage.getItem('reUsers')||'[]');         // registered users
let currentUser = JSON.parse(localStorage.getItem('reCurrent')||'null');
let balance = Number(localStorage.getItem('reBalance')||0);
let boughtPlans = JSON.parse(localStorage.getItem('reBought')||'[]');  // {name,price,profit}
let activity = JSON.parse(localStorage.getItem('reActivity')||'[]');   // latest first

/* ---------- Plans Data (13 active + 5 coming) ---------- */
const activePlans = [
  {name:"Starter Plan",price:180,profit:20,days:20},
  {name:"Basic Plan",price:500,profit:60,days:25},
  {name:"Silver Plan",price:1000,profit:130,days:30},
  {name:"Gold Plan",price:1500,profit:300,days:35},
  {name:"Pro Plan",price:2500,profit:550,days:40},
  {name:"Diamond Plan",price:4000,profit:900,days:45},
  {name:"Elite Plan",price:7000,profit:1600,days:50},
  {name:"Titan Plan",price:15000,profit:3500,days:55},
  {name:"Master Plan",price:20000,profit:4800,days:60},
  {name:"Infinity Plan",price:25000,profit:6200,days:60},
  {name:"Ultra Plan",price:30000,profit:7800,days:65},
  {name:"Super Plan",price:35000,profit:9500,days:65},
  {name:"Mega Plan",price:40000,profit:11500,days:70}
];
const comingPlans = [
  {name:"Legendary Plan",price:50000,profit:15000,days:70},
  {name:"Supreme Plan",price:70000,profit:22000,days:75},
  {name:"Infinity Plus",price:100000,profit:35000,days:80},
  {name:"Ultra Elite",price:150000,profit:50000,days:85},
  {name:"Mega Titan",price:200000,profit:75000,days:90}
];

/* ---------- Init render ---------- */
document.addEventListener('DOMContentLoaded', initApp);
function initApp(){
  // render plans
  renderPlans();
  updateUI();
  // if logged in, show welcome
  if(currentUser && currentUser.name) showWelcome();
  // auto profit loop (demo every 10s)
  setInterval(autoCreditProfit, 10000);
}

/* ---------- Render Plans ---------- */
function renderPlans(){
  const grid=document.getElementById('plansGrid'); grid.innerHTML='';
  const all = [...activePlans];
  all.forEach(p=>{
    const el=document.createElement('div'); el.className='plan';
    el.innerHTML = `
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <h3 style="font-weight:800">${p.name}</h3>
          <div class="muted small">${p.days} days • Profit: ₨ ${p.profit} total</div>
        </div>
        <div style="text-align:right">
          <div style="font-weight:800;font-size:18px">₨ ${p.price}</div>
          <div style="margin-top:8px">
            <button class="btn" onclick="buyPlan('${p.name}',${p.price},${p.profit})">Buy Now</button>
          </div>
        </div>
      </div>
    `;
    grid.appendChild(el);
  });

  // coming soon cards
  comingPlans.forEach(p=>{
    const el=document.createElement('div'); el.className='plan'; el.style.opacity=0.5;
    el.innerHTML = `
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <h3 style="font-weight:800">${p.name} <span style="font-size:12px;color:#cfefff"> (Coming Soon)</span></h3>
          <div class="muted small">${p.days} days • Profit: ₨ ${p.profit} total</div>
        </div>
        <div style="text-align:right">
          <div style="font-weight:800;font-size:18px">₨ ${p.price}</div>
          <div style="margin-top:8px">
            <button class="btn-soft" disabled>Coming</button>
          </div>
        </div>
      </div>
    `;
    grid.appendChild(el);
  });

  document.getElementById('planCount').innerText = activePlans.length + comingPlans.length;
}

/* ---------- Side Panel UI ---------- */
function openSide(html){
  const panel=document.getElementById('sidePanel'); panel.innerHTML = html; panel.style.right='0px';
}
function closeSide(){ document.getElementById('sidePanel').style.right='-420px'; }

/* ---------- Sections (sidebar) ---------- */
function showSection(key){
  playClick();
  if(key==='plans'){ window.scrollTo({top:0,behavior:'smooth'}) }
  if(key==='deposit') openDepositPanel();
  if(key==='withdraw') openWithdrawPanel();
  if(key==='profit') showProfitPanel();
  if(key==='history') showActivityPanel();
  if(key==='profile') showProfilePanel();
  if(key==='company') showCompanyPanel();
  if(key==='share') showSharePanel();
  if(key==='settings') showSettingsPanel();
}

/* ---------- BUY / DEPOSIT ---------- */
function buyPlan(name,price,profit){
  playClick();
  // store selection
  localStorage.setItem('reSelected', JSON.stringify({name,price,profit}));
  openDepositPanel();
}
function openDepositPanel(){
  const sel = JSON.parse(localStorage.getItem('reSelected')||'null') || {name:'Custom Deposit',price:0,profit:0};
  const html = `
    <div><span class="close-x" onclick="closeSide()">✕</span><h2 style="margin-top:4px">Deposit — ${sel.name}</h2></div>
    <div style="margin-top:10px" class="small muted">JazzCash / EasyPaisa — upload proof & enter txn id</div>
    <div style="margin-top:12px">
      <div class="small muted">Selected Amount</div>
      <div style="font-weight:800;font-size:20px">₨ <span id="dp_amount">${sel.price}</span></div>
      <div style="margin-top:10px"><div>JazzCash: <b id="jaz">03705519562</b> <button class="btn-soft" onclick="copyText(document.getElementById('jaz').innerText)">Copy</button></div></div>
      <div style="margin-top:8px"><div>EasyPaisa: <b id="easy">03379827882</b> <button class="btn-soft" onclick="copyText(document.getElementById('easy').innerText)">Copy</button></div></div>
      <div style="margin-top:10px"><label class="small muted">Upload Proof</label><input id="dp_proof" type="file" style="width:100%;margin-top:6px"/></div>
      <div style="margin-top:10px"><input id="dp_txn" placeholder="Transaction ID" /></div>
      <div style="margin-top:12px"><button class="btn w-full" onclick="submitDeposit()">Submit Deposit</button></div>
    </div>
  `;
  openSide(html);
}

/* deposit submit */
function submitDeposit(){
  playClick();
  const sel = JSON.parse(localStorage.getItem('reSelected')||'null') || {name:'Custom Deposit',price:0,profit:0};
  const txn = document.getElementById('dp_txn')?.value?.trim();
  const proof = document.getElementById('dp_proof')?.files?.[0];
  if(!txn || !proof){ alert('Please provide transaction id and upload proof'); return; }
  // add balance & bought plan
  balance = Number(localStorage.getItem('reBalance')||0) + Number(sel.price||0);
  localStorage.setItem('reBalance', balance);
  // save bought plan
  if(sel.name && sel.price>0){
    boughtPlans.unshift({name:sel.name,price:sel.price,profit:sel.profit,date:new Date().toISOString()});
    localStorage.setItem('reBought', JSON.stringify(boughtPlans));
    activity.unshift(`Purchased ${sel.name} ₨${sel.price} (Txn:${txn})`);
  } else {
    activity.unshift(`Deposit ₨${sel.price} (Txn:${txn})`);
  }
  localStorage.setItem('reActivity', JSON.stringify(activity));
  alert('Deposit submitted — will be verified by admin');
  updateUI();
  closeSide();
}

/* ---------- WITHDRAW ---------- */
function openWithdrawPanel(){
  const html = `
    <div><span class="close-x" onclick="closeSide()">✕</span><h2 style="margin-top:4px">Withdraw Funds</h2></div>
    <div style="margin-top:10px" class="small muted">Enter withdraw details & upload proof</div>
    <div style="margin-top:10px">
      <input id="wd_amount" type="number" placeholder="Amount" />
      <select id="wd_method" style="margin-top:8px">
        <option>JazzCash</option><option>EasyPaisa</option><option>Bank</option>
      </select>
      <input id="wd_acc" type="text" placeholder="Account Number / Mobile" style="margin-top:8px"/>
      <input id="wd_txn" type="text" placeholder="Transaction ID (if any)" style="margin-top:8px"/>
      <input id="wd_proof" type="file" style="margin-top:8px"/>
      <div style="margin-top:12px"><button class="btn w-full" onclick="submitWithdraw()">Request Withdraw</button></div>
    </div>
  `;
  openSide(html);
}
function submitWithdraw(){
  playClick();
  const amt = Number(document.getElementById('wd_amount')?.value||0);
  const method = document.getElementById('wd_method')?.value;
  const acc = document.getElementById('wd_acc')?.value?.trim();
  const txn = document.getElementById('wd_txn')?.value?.trim();
  const proof = document.getElementById('wd_proof')?.files?.[0];
  const bal = Number(localStorage.getItem('reBalance')||0);
  if(!amt || !acc || !proof){ alert('Fill amount, account and upload proof'); return; }
  if(amt>bal){ alert('Insufficient balance'); return; }
  let newBal = bal - amt;
  localStorage.setItem('reBalance', newBal);
  activity.unshift(`Withdraw ₨${amt} via ${method} (Acc:${acc})`);
  localStorage.setItem('reActivity', JSON.stringify(activity));
  alert('Withdraw request submitted');
  updateUI();
  closeSide();
}

/* ---------- PROFIT (auto credit) ---------- */
function autoCreditProfit(){
  // credit each bought plan's daily profit to balance (demo: every 10s)
  const bought = JSON.parse(localStorage.getItem('reBought')||'[]');
  if(!bought.length) return;
  let total = 0;
  bought.forEach(p => { total += Number(p.profit||0); });
  // add small fraction (demo) to balance
  let bal = Number(localStorage.getItem('reBalance')||0);
  bal += total; // in demo we add full profit each interval
  localStorage.setItem('reBalance', bal);
  activity.unshift(`Auto credit: ₨${total} (plans)`);
  localStorage.setItem('reActivity', JSON.stringify(activity));
  updateUI();
}

/* ---------- ACTIVITY ---------- */
function showActivityPanel(){ showSectionContent('Activity History', renderActivityHTML()); }
function renderActivityHTML(){
  let list = JSON.parse(localStorage.getItem('reActivity')||'[]');
  if(!list.length) return `<div class="small muted">No activity yet</div>`;
  return `<div>${list.slice(0,50).map(i=>`<div style="padding:6px;border-bottom:1px solid rgba(255,255,255,0.02)">${i}</div>`).join('')}</div>`;
}

/* ---------- PROFILE / COMPANY / SHARE / SETTINGS ---------- */
function showProfilePanel(){ showSectionContent('Profile', `
  <div class="small muted">Name: <b>${currentUser? currentUser.name : 'Guest'}</b></div>
  <div class="small muted">Email: <b>${currentUser? currentUser.email : '—'}</b></div>
  <div style="margin-top:12px"><button class="btn-soft" onclick="copyProfileLink()">Copy profile link</button></div>
`)}

function showCompanyPanel(){
  showSectionContent('Company Details', `
    <div class="muted small">Launched: <b>2018</b></div>
    <div class="muted small">Industry: <b>Cryptocurrency & FinTech</b></div>
    <div class="muted small">Partners: <b>Binance & leading platforms</b></div>
    <div class="muted small">Headquarters: <b>1234 Crypto Avenue, San Francisco, California, USA</b></div>
    <div style="margin-top:8px" class="muted small">Contact: support@rockearnpro.com</div>
  `);
}

function showSharePanel(){
  const link = window.location.href + '?ref=' + encodeURIComponent(currentUser? currentUser.email : 'guest');
  showSectionContent('Share Link', `<div class="small muted">${link}</div><div style="margin-top:10px"><button class="btn-soft" onclick="copyText('${link}')">Copy Link</button></div>`);
}

function showSettingsPanel(){
  showSectionContent('Settings', `
    <div class="small muted">Change display name / password</div>
    <input id="setName" placeholder="New name" style="margin-top:8px"/>
    <input id="setPass" placeholder="New password" style="margin-top:8px"/>
    <div style="margin-top:10px"><button class
