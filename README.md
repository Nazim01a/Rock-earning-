<ROCK EARNING>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Rock Earning LLC — Final</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{--a1:#5b21b6;--a2:#06b6d4;}
  body{font-family:Inter,system-ui,Arial;margin:0;background:linear-gradient(180deg,#f8fafc,#eef2ff);}
  .btn-grad{background:linear-gradient(90deg,var(--a1),var(--a2));}
  .card-hover:hover{transform:translateY(-6px);box-shadow:0 12px 30px rgba(2,6,23,0.08);transition:0.22s;}
  .hidden{display:none;}
  .copy-box{background:#f1f5f9;padding:10px;border-radius:8px;text-align:center;font-weight:700;}
</style>
</head>
<body>

<header class="w-full shadow-sm">
  <div class="max-w-6xl mx-auto flex items-center justify-between p-4">
    <div class="flex items-center gap-3">
      <div class="w-12 h-12 rounded-xl flex items-center justify-center" style="background:linear-gradient(135deg,var(--a1),var(--a2));color:white;font-weight:800">RE</div>
      <div>
        <div class="text-lg font-extrabold">🪨 Rock Earning LLC — California (Established 2018)</div>
        <div class="text-xs text-slate-500">Professional demo dashboard</div>
      </div>
    </div>

    <div id="headerRight" class="flex items-center gap-2">
      <button id="btnHome" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="home">Home</button>
      <button id="btnPlans" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="plans">Plans</button>
      <button id="btnDeposit" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="deposit">Deposit</button>
      <button id="btnWithdraw" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="withdraw">Withdraw</button>
      <button id="btnActivity" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="activity">Activity</button>
      <button id="btnCompany" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="company">Company</button>

      <button id="profileBtn" class="hidden text-sm px-3 py-1 rounded-md border">Profile</button>
      <button id="shareBtn" class="hidden text-sm px-3 py-1 rounded-md border">Share</button>
      <button id="logoutBtn" class="hidden text-sm px-3 py-1 rounded-md bg-red-500 text-white">Logout</button>

      <button id="openAuth" class="px-3 py-1 rounded-md btn-grad text-white">Login / Sign Up</button>
    </div>
  </div>
</header>

<main class="max-w-6xl mx-auto p-6">

  <!-- Landing (simple form with email / phone / password) -->
  <section id="landingSection" class="min-h-[360px] flex items-center justify-center">
    <div class="w-full max-w-md bg-white rounded-xl p-6 shadow card-hover">
      <h2 class="text-2xl font-bold mb-2 text-center">Welcome to Rock Earning</h2>
      <p class="text-sm text-slate-500 mb-4 text-center">Use Gmail, phone and password to Login or Sign Up</p>

      <div class="flex flex-col gap-3">
        <input id="inputEmail" type="email" placeholder="Gmail / Email" class="p-3 rounded border" />
        <input id="inputPhone" type="text" placeholder="Phone (03xxxxxxxxx)" class="p-3 rounded border" />
        <input id="inputPass" type="password" placeholder="Password" class="p-3 rounded border" />
        <div class="flex gap-3 justify-center mt-2">
          <button id="btnLogin" class="px-4 py-2 rounded-md btn-grad text-white">Login</button>
          <button id="btnSignup" class="px-4 py-2 rounded-md border">Sign Up</button>
        </div>
        <div id="landingError" class="text-sm text-red-500 text-center hidden"></div>
      </div>
    </div>
  </section>

  <!-- App Area (hidden until login) -->
  <section id="appArea" class="hidden">

    <!-- HOME -->
    <div id="tab-home" class="tab hidden">
      <div class="grid md:grid-cols-3 gap-4 mb-6">
        <div class="col-span-2 p-6 rounded-xl bg-white shadow">
          <div class="flex justify-between items-start">
            <div>
              <h3 class="text-2xl font-bold">Welcome, <span id="displayName">User</span></h3>
              <p class="text-sm text-slate-500">Overview</p>
            </div>
            <div class="text-right">
              <div class="text-xs text-slate-400">Account Balance</div>
              <div id="displayBalance" class="text-2xl font-extrabold">PKR 0</div>
            </div>
          </div>
          <div class="mt-4 text-sm text-slate-500">Select a plan, then deposit proof to start. Daily profit will be added automatically (simulation).</div>
        </div>

        <div class="p-6 rounded-xl bg-white shadow">
          <div class="text-sm text-slate-600">Quick Actions</div>
          <div class="mt-4 flex flex-col gap-3">
            <button id="gotoPlans" class="px-3 py-2 rounded-md btn-grad text-white">Browse Plans</button>
            <button id="gotoDeposit" class="px-3 py-2 rounded-md bg-green-500 text-white">Deposit</button>
            <button id="gotoWithdraw" class="px-3 py-2 rounded-md bg-amber-500 text-white">Withdraw</button>
          </div>
        </div>
      </div>
    </div>

    <!-- PLANS -->
    <div id="tab-plans" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Available Plans</h3>
        <div class="text-sm text-slate-500">Choose & then submit deposit to start</div>
      </div>
      <div id="plansContainer" class="grid md:grid-cols-3 gap-4"></div>
    </div>

    <!-- DEPOSIT -->
    <div id="tab-deposit" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Deposit</h3>
        <div class="text-sm text-slate-500">Easypaisa / JazzCash</div>
      </div>

      <div class="grid md:grid-cols-2 gap-6">
        <div class="p-6 bg-white rounded-xl shadow">
          <div class="text-sm text-slate-600">Select Method</div>
          <div class="mt-3 flex flex-col gap-3">
            <button class="methodBtn px-4 py-2 rounded-md bg-green-500 text-white" data-method="Easypaisa" data-number="03379827882">Easypaisa</button>
            <button class="methodBtn px-4 py-2 rounded-md bg-blue-500 text-white" data-method="JazzCash" data-number="03705519562">JazzCash</button>
          </div>

          <div id="depositInfo" class="mt-4 hidden">
            <div id="depositNumber" class="copy-box mb-2">—</div>
            <div class="flex gap-2">
              <button id="copyDeposit" class="px-3 py-1 rounded-md bg-indigo-600 text-white">Copy Number</button>
              <button id="showDepForm" class="px-3 py-1 rounded-md border">Proceed</button>
            </div>
          </div>
        </div>

        <div id="depositFormCard" class="p-6 bg-white rounded-xl shadow hidden">
          <h4 class="font-semibold">Submit Deposit Proof</h4>
          <div class="mt-3 flex flex-col gap-3">
            <input id="depAmt" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
            <input id="depTxn" placeholder="Transaction ID" class="p-2 rounded border" />
            <input id="depProof" type="file" accept="image/*" />
            <button id="depSubmit" class="px-4 py-2 rounded-md btn-grad text-white">Submit Deposit</button>
          </div>
        </div>
      </div>
    </div>

    <!-- WITHDRAW -->
    <div id="tab-withdraw" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Withdraw</h3>
        <div class="text-sm text-slate-500">Easypaisa / JazzCash / Bank</div>
      </div>

      <div class="grid md:grid-cols-2 gap-6">
        <div class="p-6 bg-white rounded-xl shadow">
          <div class="text-sm text-slate-600">Select Method</div>
          <div class="mt-3 flex flex-col gap-2">
            <label><input type="radio" name="wMethod" value="easypaisa" checked> Easypaisa</label>
            <label><input type="radio" name="wMethod" value="jazzcash"> JazzCash</label>
            <label><input type="radio" name="wMethod" value="bank"> Bank Transfer</label>
          </div>
        </div>

        <div class="p-6 bg-white rounded-xl shadow">
          <h4 class="font-semibold">Request Withdrawal</h4>
          <div class="mt-3 flex flex-col gap-3">
            <input id="wdAmt" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
            <input id="wdTo" placeholder="Recipient Number / IBAN" class="p-2 rounded border" />
            <textarea id="wdNote" placeholder="Note (optional)" class="p-2 rounded border"></textarea>
            <button id="wdSubmit" class="px-4 py-2 rounded-md bg-amber-500 text-white">Submit Withdrawal</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ACTIVITY -->
    <div id="tab-activity" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Activity History</h3>
        <div><button id="clearActs" class="px-3 py-1 rounded-md border">Clear</button></div>
      </div>
      <div id="actsList" class="space-y-2"></div>
    </div>

    <!-- COMPANY -->
    <div id="tab-company" class="tab hidden p-6 bg-white rounded-xl shadow">
      <h3 class="text-lg font-bold">Company Details</h3>
      <p class="text-sm text-slate-600 mt-2">Rock Earning LLC — California (Established 2018). Demo interface only. Support: <strong>support@rockearning.com</strong></p>
    </div>

  </section>

</main>

<script>
/* ---------- Simple, clean logic ---------- */

// State
let STATE = {
  user: null,
  balance: 0,
  plan: null,
  activities: []
};

// Plans
const PLANS = [
  {name:'Plan 180', price:180, profit:35, duration:30},
  {name:'Plan 280', price:280, profit:55, duration:30},
  {name:'Plan 500', price:500, profit:110, duration:30},
  {name:'Plan 1000', price:1000, profit:240, duration:30},
  {name:'Plan 2000', price:2000, profit:480, duration:35},
  {name:'Plan 3000', price:3000, profit:700, duration:35},
  {name:'Plan 5000', price:5000, profit:1200, duration:40}
];

// Helpers: persistence
function saveState(){
  localStorage.setItem('re_user', JSON.stringify(STATE.user));
  localStorage.setItem('re_balance', String(STATE.balance));
  localStorage.setItem('re_plan', JSON.stringify(STATE.plan));
  localStorage.setItem('re_activities', JSON.stringify(STATE.activities));
}
function loadState(){
  const u = JSON.parse(localStorage.getItem('re_user'));
  if(u) STATE.user = u;
  const b = parseInt(localStorage.getItem('re_balance'));
  if(!isNaN(b)) STATE.balance = b;
  const p = JSON.parse(localStorage.getItem('re_plan'));
  if(p) STATE.plan = p;
  const a = JSON.parse(localStorage.getItem('re_activities'));
  if(a) STATE.activities = a;
}

// UI refs
const landingSection = document.getElementById('landingSection');
const appArea = document.getElementById('appArea');
const openAuth = document.getElementById('openAuth');

const inputEmail = document.getElementById('inputEmail');
const inputPhone = document.getElementById('inputPhone');
const inputPass = document.getElementById('inputPass');
const btnLogin = document.getElementById('btnLogin');
const btnSignup = document.getElementById('btnSignup');
const landingError = document.getElementById('landingError');

const headerButtons = ['btnHome','btnPlans','btnDeposit','btnWithdraw','btnActivity','btnCompany','profileBtn','shareBtn','logoutBtn'];

const displayName = document.getElementById('displayName');
const displayBalance = document.getElementById('displayBalance');

const plansContainer = document.getElementById('plansContainer');

const methodBtns = document.querySelectorAll('.methodBtn');
const depositInfo = document.getElementById('depositInfo');
const depositNumber = document.getElementById('depositNumber');
const copyDeposit = document.getElementById('copyDeposit');
const showDepForm = document.getElementById('showDepForm');
const depositFormCard = document.getElementById('depositFormCard');
const depAmt = document.getElementById('depAmt');
const depTxn = document.getElementById('depTxn');
const depProof = document.getElementById('depProof');
const depSubmit = document.getElementById('depSubmit');

const wdSubmit = document.getElementById('wdSubmit');
const wdAmt = document.getElementById('wdAmt');
const wdTo = document.getElementById('wdTo');
const wdNote = document.getElementById('wdNote');

const actsList = document.getElementById('actsList');
const clearActs = document.getElementById('clearActs');

const navHome = document.getElementById('btnHome');
const navPlans = document.getElementById('btnPlans');
const navDeposit = document.getElementById('btnDeposit');
const navWithdraw = document.getElementById('btnWithdraw');
const navActivity = document.getElementById('btnActivity');
const navCompany = document.getElementById('btnCompany');

const profileBtn = document.getElementById('profileBtn');
const shareBtn = document.getElementById('shareBtn');
const logoutBtn = document.getElementById('logoutBtn');

const gotoPlans = document.getElementById('gotoPlans');
const gotoDeposit = document.getElementById('gotoDeposit');
const gotoWithdraw = document.getElementById('gotoWithdraw');

// Simple validation
function validEmail(e){ return /\S+@\S+\.\S+/.test(e); }

// UI show/hide helpers
function showLanding(){ landingSection.classList.remove('hidden'); appArea.classList.add('hidden'); headerButtons.forEach(id=>document.getElementById(id).classList.add('hidden')); openAuth.classList.remove('hidden'); }
function showApp(){ landingSection.classList.add('hidden'); appArea.classList.remove('hidden'); headerButtons.forEach(id=>document.getElementById(id).classList.remove('hidden')); openAuth.classList.add('hidden'); switchTab('tab-home'); }

// Tab switching
function switchTab(tabId){
  document.querySelectorAll('#appArea .tab').forEach(t=>t.classList.add('hidden'));
  const el = document.getElementById(tabId);
  if(el) el.classList.remove('hidden');
  window.scrollTo({top:0,behavior:'smooth'});
}
navHome?.addEventListener('click', ()=> switchTab('tab-home'));
navPlans?.addEventListener('click', ()=> switchTab('tab-plans'));
navDeposit?.addEventListener('click', ()=> switchTab('tab-deposit'));
navWithdraw?.addEventListener('click', ()=> switchTab('tab-withdraw'));
navActivity?.addEventListener('click', ()=> switchTab('tab-activity'));
navCompany?.addEventListener('click', ()=> switchTab('tab-company'));

gotoPlans?.addEventListener('click', ()=> switchTab('tab-plans'));
gotoDeposit?.addEventListener('click', ()=> switchTab('tab-deposit'));
gotoWithdraw?.addEventListener('click', ()=> switchTab('tab-withdraw'));

// Render plans
function renderPlans(){
  plansContainer.innerHTML = '';
  PLANS.forEach(p=>{
    const card = document.createElement('div');
    card.className = 'p-4 rounded-xl bg-white shadow card-hover flex flex-col gap-3';
    card.innerHTML = `<div class="text-sm text-slate-500">${p.name}</div>
      <div class="font-bold text-2xl">PKR ${p.price}</div>
      <div class="text-xs text-slate-400">Daily Profit PKR ${p.profit} • ${p.duration} days</div>
      <div class="mt-auto flex gap-2"><button class="buyBtn px-3 py-2 rounded-md btn-grad text-white">Buy</button><button class="detBtn border px-3 py-2 rounded-md">Details</button></div>`;
    plansContainer.appendChild(card);
    card.querySelector('.buyBtn').addEventListener('click', ()=>{
      if(!STATE.user) return alert('Login first.');
      STATE.plan = {...p, startTime: null, count:0};
      STATE.activities.unshift(`Plan selected: ${p.name} — PKR ${p.price}`);
      saveState();
      alert(`Plan ${p.name} selected. Now go to Deposit and submit proof to start.`);
      switchTab('tab-deposit');
      renderAll();
    });
    card.querySelector('.detBtn').addEventListener('click', ()=> alert(`${p.name}\nPrice: PKR ${p.price}\nDaily: PKR ${p.profit}\nDuration: ${p.duration} days`));
  });
}

// Deposit flow
let currentMethod = null;
methodBtns.forEach(btn=>{
  btn.addEventListener('click', ()=>{
    currentMethod = {method: btn.dataset.method, number: btn.dataset.number};
    depositInfo.classList.remove('hidden');
    depositNumber.textContent = `${currentMethod.method}: ${currentMethod.number}`;
    depositFormCard.classList.add('hidden');
  });
});
copyDeposit?.addEventListener('click', ()=> {
  if(!currentMethod) return alert('Select method first.');
  navigator.clipboard.writeText(currentMethod.number);
  alert('Number copied.');
});
showDepForm?.addEventListener('click', ()=> {
  if(!currentMethod) return alert('Select method first.');
  depositFormCard.classList.remove('hidden');
});
depSubmit?.addEventListener('click', ()=>{
  if(!STATE.user) return alert('Login first.');
  const amt = parseInt(depAmt.value);
  const txn = depTxn.value.trim();
  const file = depProof.files[0];
  if(!amt || !txn || !file) return alert('Provide amount, txn id and proof.');
  if(STATE.plan && !STATE.plan.startTime){
    STATE.plan.startTime = new Date().toISOString();
    STATE.plan.count = 0;
    STATE.activities.unshift(`Plan started: ${STATE.plan.name} at ${new Date().toLocaleString()}`);
  }
  STATE.balance += amt;
  STATE.activities.unshift(`Deposit: PKR ${amt} via ${currentMethod ? currentMethod.method : '—'} TXN:${txn}`);
  saveState();
  renderAll();
  alert('Deposit submitted (simulation).');
  depAmt.value=''; depTxn.value=''; depProof.value=null;
  depositFormCard.classList.add('hidden');
});

// Withdraw flow
wdSubmit?.addEventListener('click', ()=>{
  if(!STATE.user) return alert('Login first.');
  const amt = parseInt(wdAmt.value);
  const to = wdTo.value.trim();
  const method = document.querySelector('input[name="wMethod"]:checked')?.value || 'easypaisa';
  if(!amt || !to) return alert('Enter amount and recipient.');
  if(amt > STATE.balance) return alert('Insufficient balance.');
  STATE.balance -= amt;
  STATE.activities.unshift(`Withdrawal requested: PKR ${amt} via ${method} to ${to}`);
  saveState();
  renderAll();
  alert('Withdrawal request submitted (simulation).');
  wdAmt.value=''; wdTo.value=''; wdNote.value='';
});

// Activities
clearActs?.addEventListener('click', ()=> {
  if(!confirm('Clear activity history?')) return;
  STATE.activities = [];
  saveState();
  renderAll();
});

// Auth: Signup & Login
btnSignup.addEventListener('click', ()=> {
  const email = inputEmail.value.trim();
  const phone = inputPhone.value.trim();
  const pass = inputPass.value.trim();
  if(!email || !phone || !pass){ landingError.textContent = 'Please fill all fields.'; landingError.classList.remove('hidden'); return; }
  if(!validEmail(email)){ landingError.textContent = 'Invalid email.'; landingError.classList.remove('hidden'); return; }
  // save user object
  const user = {email, phone, pass, name: email.split('@')[0]};
  localStorage.setItem('re_user', JSON.stringify(user));
  STATE.user = user;
  // init other data if not present
  if(!localStorage.getItem('re_balance')) localStorage.setItem('re_balance','0');
  if(!localStorage.getItem('re_activities')) localStorage.setItem('re_activities', JSON.stringify([]));
  loadState(); // load defaults
  landingError.classList.add('hidden');
  afterLogin();
});

btnLogin.addEventListener('click', ()=> {
  const email = inputEmail.value.trim();
  const phone = inputPhone.value.trim();
  const pass = inputPass.value.trim();
  if(!email || !phone || !pass){ landingError.textContent = 'Please fill all fields.'; landingError.classList.remove('hidden'); return; }
  const stored = JSON.parse(localStorage.getItem('re_user'));
  if(stored && stored.email === email && stored.phone === phone && stored.pass === pass){
    STATE.user = stored;
    loadState();
    landingError.classList.add('hidden');
    afterLogin();
  } else {
    landingError.textContent = 'No account found or wrong credentials. Please sign up or check details.';
    landingError.classList.remove('hidden');
  }
});

// Header buttons: profile / share / logout
profileBtn?.addEventListener('click', ()=> {
  if(!STATE.user)
