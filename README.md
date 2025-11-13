<ROCK>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>🪨 Rock Earning LLC — Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{--accent1:#5b21b6;--accent2:#06b6d4;}
  body{font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,Arial;background:linear-gradient(180deg,#f8fafc,#eef2ff);color:#0f172a;}
  .btn-grad{background:linear-gradient(90deg,var(--accent1),var(--accent2));}
  .card-hover:hover{transform:translateY(-6px);box-shadow:0 12px 30px rgba(2,6,23,0.08);transition:0.22s;}
  .hidden{display:none;}
  .copy-box{background:#f1f5f9;padding:10px;border-radius:8px;text-align:center;font-weight:700;}
  .logo-circle{background:linear-gradient(135deg,var(--accent1),var(--accent2));color:white;}
  input,textarea,select{outline:none;}
</style>
</head>
<body>

<!-- HEADER -->
<header class="w-full shadow-sm">
  <div class="max-w-6xl mx-auto flex items-center justify-between p-4">
    <div class="flex items-center gap-3">
      <div class="w-12 h-12 rounded-xl flex items-center justify-center logo-circle">
        <span class="text-xl font-extrabold">RE</span>
      </div>
      <div>
        <div class="text-lg font-extrabold">🪨 Rock Earning LLC — California (Established 2018)</div>
        <div class="text-xs text-slate-500">Professional simulated dashboard</div>
      </div>
    </div>

    <div id="headerControls" class="flex items-center gap-2">
      <button id="navHome" class="hidden px-3 py-1 rounded-md hover:bg-slate-100 text-sm" data-tab="homeTab">Home</button>
      <button id="navPlans" class="hidden px-3 py-1 rounded-md hover:bg-slate-100 text-sm" data-tab="plansTab">Plans</button>
      <button id="navDeposit" class="hidden px-3 py-1 rounded-md hover:bg-slate-100 text-sm" data-tab="depositTab">Deposit</button>
      <button id="navWithdraw" class="hidden px-3 py-1 rounded-md hover:bg-slate-100 text-sm" data-tab="withdrawTab">Withdraw</button>
      <button id="navActivity" class="hidden px-3 py-1 rounded-md hover:bg-slate-100 text-sm" data-tab="activityTab">Activity</button>
      <button id="navCompany" class="hidden px-3 py-1 rounded-md hover:bg-slate-100 text-sm" data-tab="companyTab">Company</button>

      <button id="profileBtn" class="hidden px-3 py-1 rounded-md border text-sm">Profile</button>
      <button id="shareBtn" class="hidden px-3 py-1 rounded-md border text-sm">Share</button>
      <button id="logoutBtn" class="hidden px-3 py-1 rounded-md bg-red-500 text-white text-sm">Logout</button>

      <button id="openAuthBtn" class="px-3 py-1 rounded-md btn-grad text-white text-sm">Login / Sign Up</button>
    </div>
  </div>
</header>

<main class="max-w-6xl mx-auto p-6">

  <!-- LANDING: simple form (email, phone, password) -->
  <section id="landing" class="min-h-[300px] flex items-center justify-center">
    <div class="w-full max-w-md bg-white rounded-xl p-6 shadow card-hover">
      <h2 class="text-2xl font-bold mb-2 text-center">Welcome to Rock Earning</h2>
      <p class="text-sm text-slate-500 mb-4 text-center">Login or Sign Up to access the dashboard</p>

      <!-- form directly on landing per your request -->
      <div class="flex flex-col gap-3">
        <input id="emailInput" type="email" placeholder="Gmail / Email" class="p-3 rounded border" />
        <input id="phoneInput" type="text" placeholder="Phone number (e.g., 03xxxxxxxxx)" class="p-3 rounded border" />
        <input id="passInput" type="password" placeholder="Password" class="p-3 rounded border" />
        <div class="flex gap-3 justify-center mt-2">
          <button id="loginBtn" class="px-4 py-2 rounded-md btn-grad text-white">Login</button>
          <button id="signupBtn" class="px-4 py-2 rounded-md border">Sign Up</button>
        </div>
        <div id="landingMsg" class="text-sm text-red-500 text-center hidden"></div>
      </div>
    </div>
  </section>

  <!-- APP AREA (hidden until login) -->
  <section id="app" class="hidden">
    <!-- HOME -->
    <div id="homeTab" class="tab-content hidden">
      <div class="grid md:grid-cols-3 gap-4 mb-6">
        <div class="col-span-2 p-6 rounded-xl bg-white shadow">
          <div class="flex justify-between items-start">
            <div>
              <h3 class="text-2xl font-bold">Welcome, <span id="userName">User</span></h3>
              <p class="text-sm text-slate-500">Dashboard overview</p>
            </div>
            <div class="text-right">
              <div class="text-xs text-slate-400">Account Balance</div>
              <div id="balanceDisplay" class="text-2xl font-extrabold">PKR 0</div>
            </div>
          </div>
          <div class="mt-4 text-sm text-slate-500">Choose a plan, then deposit proof to start the simulation. Daily profit will be added automatically.</div>
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
    <div id="plansTab" class="tab-content hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Available Plans</h3>
        <div class="text-sm text-slate-500">Select a plan then submit deposit to start.</div>
      </div>
      <div id="plansGrid" class="grid md:grid-cols-3 gap-4"></div>
    </div>

    <!-- DEPOSIT -->
    <div id="depositTab" class="tab-content hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Deposit</h3>
        <div class="text-sm text-slate-500">Select method, copy number, pay and upload proof.</div>
      </div>

      <div class="grid md:grid-cols-2 gap-6">
        <div class="p-6 bg-white rounded-xl shadow">
          <div class="text-sm text-slate-600">Deposit Methods</div>
          <div class="mt-3 flex flex-col gap-3">
            <button class="depositMethod px-4 py-2 rounded-md bg-green-500 text-white" data-method="Easypaisa" data-number="03379827882">Easypaisa</button>
            <button class="depositMethod px-4 py-2 rounded-md bg-blue-500 text-white" data-method="JazzCash" data-number="03705519562">JazzCash</button>
          </div>

          <div id="depositInfoBox" class="mt-4 hidden">
            <div id="depositNumberBox" class="copy-box mb-2">—</div>
            <div class="flex gap-2">
              <button id="copyDepositBtn" class="px-3 py-1 rounded-md bg-indigo-600 text-white">Copy Number</button>
              <button id="openDepositFormBtn" class="px-3 py-1 rounded-md border">Proceed to Deposit</button>
            </div>
          </div>
        </div>

        <div id="depositForm" class="p-6 bg-white rounded-xl shadow hidden">
          <h4 class="font-semibold">Submit Deposit Proof</h4>
          <div class="mt-3 flex flex-col gap-3">
            <input id="depAmount" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
            <input id="depTxn" placeholder="Transaction ID / Ref" class="p-2 rounded border" />
            <input id="depProof" type="file" accept="image/*" />
            <button id="submitDeposit" class="px-4 py-2 rounded-md btn-grad text-white">Submit Deposit</button>
          </div>
        </div>
      </div>
    </div>

    <!-- WITHDRAW -->
    <div id="withdrawTab" class="tab-content hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Withdraw</h3>
        <div class="text-sm text-slate-500">Easypaisa / JazzCash / Bank</div>
      </div>

      <div class="grid md:grid-cols-2 gap-6">
        <div class="p-6 bg-white rounded-xl shadow">
          <div class="text-sm text-slate-600">Select Method</div>
          <div class="mt-3 flex flex-col gap-2">
            <label class="flex items-center gap-2"><input type="radio" name="wMethod" value="easypaisa" checked> Easypaisa</label>
            <label class="flex items-center gap-2"><input type="radio" name="wMethod" value="jazzcash"> JazzCash</label>
            <label class="flex items-center gap-2"><input type="radio" name="wMethod" value="bank"> Bank Transfer</label>
          </div>
        </div>

        <div class="p-6 bg-white rounded-xl shadow">
          <h4 class="font-semibold">Request Withdrawal</h4>
          <div class="mt-3 flex flex-col gap-3">
            <input id="wdAmount" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
            <input id="wdTo" placeholder="Recipient Number / IBAN" class="p-2 rounded border" />
            <textarea id="wdNote" placeholder="Note (optional)" class="p-2 rounded border"></textarea>
            <button id="submitWithdraw" class="px-4 py-2 rounded-md bg-amber-500 text-white">Submit Withdrawal</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ACTIVITY -->
    <div id="activityTab" class="tab-content hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Activity History</h3>
        <div><button id="clearActivity" class="px-3 py-1 rounded-md border">Clear</button></div>
      </div>
      <div id="activityList" class="space-y-2"></div>
    </div>

    <!-- COMPANY -->
    <div id="companyTab" class="tab-content hidden p-6 bg-white rounded-xl shadow">
      <h3 class="text-lg font-bold">Company Details</h3>
      <p class="text-sm text-slate-600 mt-2">Rock Earning LLC — California (Established 2018). This is a simulated demo interface. For support: <strong>support@rockearning.com</strong></p>
      <div class="text-sm text-slate-500 mt-3">Address: 123 Demo Street, San Francisco, CA. Legal: Rock Earning LLC (Demo)</div>
    </div>
  </section>

  <footer class="mt-8 text-center text-xs text-slate-400">© Rock Earning LLC — Demo interface</footer>
</main>

<script>
/* ---------- State ---------- */
const state = {
  user: null,
  balance: 0,
  plan: null, // {name,price,profit,duration,startTime,count}
  activities: []
};

/* Plans */
const PLANS = [
  {name:'Plan A', price:180, profit:35, duration:30},
  {name:'Plan B', price:280, profit:55, duration:30},
  {name:'Plan C', price:500, profit:110, duration:30},
  {name:'Plan D', price:1000, profit:240, duration:30},
  {name:'Plan E', price:2000, profit:480, duration:35},
  {name:'Plan F', price:3000, profit:700, duration:35},
  {name:'Plan G', price:5000, profit:1200, duration:40}
];

/* Elements */
const landing = document.getElementById('landing');
const app = document.getElementById('app');
const openAuthBtn = document.getElementById('openAuthBtn');
const emailInput = document.getElementById('emailInput');
const phoneInput = document.getElementById('phoneInput');
const passInput = document.getElementById('passInput');
const loginBtn = document.getElementById('loginBtn');
const signupBtn = document.getElementById('signupBtn');
const landingMsg = document.getElementById('landingMsg');

const headerIdsShow = ['navHome','navPlans','navDeposit','navWithdraw','navActivity','navCompany','profileBtn','shareBtn','logoutBtn'];
const userNameSpan = document.getElementById('userName');
const balanceDisplay = document.getElementById('balanceDisplay');
const plansGrid = document.getElementById('plansGrid');

const depositMethods = document.querySelectorAll('.depositMethod');
const depositInfoBox = document.getElementById('depositInfoBox');
const depositNumberBox = document.getElementById('depositNumberBox');
const copyDepositBtn = document.getElementById('copyDepositBtn');
const openDepositFormBtn = document.getElementById('openDepositFormBtn');
const depositForm = document.getElementById('depositForm');
const depAmount = document.getElementById('depAmount');
const depTxn = document.getElementById('depTxn');
const depProof = document.getElementById('depProof');
const submitDeposit = document.getElementById('submitDeposit');

const submitWithdraw = document.getElementById('submitWithdraw');
const wdAmount = document.getElementById('wdAmount');
const wdTo = document.getElementById('wdTo');
const wdNote = document.getElementById('wdNote');

const activityList = document.getElementById('activityList');
const clearActivity = document.getElementById('clearActivity');

const navHome = document.getElementById('navHome');
const navPlans = document.getElementById('navPlans');
const navDeposit = document.getElementById('navDeposit');
const navWithdraw = document.getElementById('navWithdraw');
const navActivity = document.getElementById('navActivity');
const navCompany = document.getElementById('navCompany');

const profileBtn = document.getElementById('profileBtn');
const shareBtn = document.getElementById('shareBtn');
const logoutBtn = document.getElementById('logoutBtn');

const gotoPlans = document.getElementById('gotoPlans');
const gotoDeposit = document.getElementById('gotoDeposit');
const gotoWithdraw = document.getElementById('gotoWithdraw');

/* Persist helpers */
function saveAll(){
  localStorage.setItem('re_user', JSON.stringify(state.user));
  localStorage.setItem('re_balance', String(state.balance));
  localStorage.setItem('re_plan', JSON.stringify(state.plan));
  localStorage.setItem('re_acts', JSON.stringify(state.activities));
}
function loadAll(){
  const u = JSON.parse(localStorage.getItem('re_user'));
  if(u) state.user = u;
  const bal = parseInt(localStorage.getItem('re_balance'));
  if(!isNaN(bal)) state.balance = bal;
  const p = JSON.parse(localStorage.getItem('re_plan'));
  if(p) state.plan = p;
  const a = JSON.parse(localStorage.getItem('re_acts'));
  if(a) state.activities = a;
}

/* UI helpers */
function showLanding(){
  landing.classList.remove('hidden');
  app.classList.add('hidden');
  headerIdsShow.forEach(id=> document.getElementById(id).classList.add('hidden'));
  document.getElementById('openAuthBtn').classList.remove('hidden');
}
function showApp(){
  landing.classList.add('hidden');
  app.classList.remove('hidden');
  headerIdsShow.forEach(id=> document.getElementById(id).classList.remove('hidden'));
  document.getElementById('openAuthBtn').classList.add('hidden');
  switchTo('homeTab');
}
function switchTo(tabId){
  // hide all tab-content inside #app
  document.querySelectorAll('#app .tab-content').forEach(t=>t.classList.add('hidden'));
  const el = document.getElementById(tabId);
  if(el) el.classList.remove('hidden');
  window.scrollTo({top:0,behavior:'smooth'});
}

/* Render plans */
function renderPlans(){
  plansGrid.innerHTML = '';
  PLANS.forEach(p=>{
    const card = document.createElement('div');
    card.className = 'p-4 rounded-xl bg-white shadow card-hover flex flex-col gap-3';
    card.innerHTML = `
      <div class="text-sm text-slate-500">${p.name}</div>
      <div class="font-bold text-2xl">PKR ${p.price}</div>
      <div class="text-xs text-slate-400">Daily Profit PKR ${p.profit} • ${p.duration} days</div>
      <div class="mt-auto flex gap-2">
        <button class="buyPlan btn-grad text-white px-3 py-2 rounded-md">Buy</button>
        <button class="detailsPlan border px-3 py-2 rounded-md">Details</button>
      </div>`;
    plansGrid.appendChild(card);

    card.querySelector('.buyPlan').addEventListener('click', ()=>{
      if(!state.user) return alert('Please login/signup first.');
      state.plan = {...p, startTime: null, count: 0};
      state.activities.unshift(`Plan selected: ${p.name} — PKR ${p.price}`);
      saveAll();
      alert(`Plan ${p.name} selected. Now go to Deposit and submit proof to start.`);
      switchTo('depositTab');
      renderAll();
    });
    card.querySelector('.detailsPlan').addEventListener('click', ()=> {
      alert(`${p.name}\nPrice: PKR ${p.price}\nDaily: PKR ${p.profit}\nDuration: ${p.duration} days`);
    });
  });
}

/* Deposit logic */
let chosenDeposit = null;
depositMethods.forEach(btn=>{
  btn.addEventListener('click', ()=> {
    chosenDeposit = {method: btn.dataset.method, number: btn.dataset.number};
    depositInfoBox.classList.remove('hidden');
    depositNumberBox.textContent = `${chosenDeposit.method}: ${chosenDeposit.number}`;
    depositForm.classList.add('hidden');
  });
});
copyDepositBtn.addEventListener('click', ()=> {
  if(!chosenDeposit) return alert('Select method first');
  navigator.clipboard.writeText(chosenDeposit.number);
  alert('Number copied!');
});
openDepositFormBtn.addEventListener('click', ()=> {
  if(!chosenDeposit) return alert('Select method first');
  depositForm.classList.remove('hidden');
});
submitDeposit.addEventListener('click', ()=> {
  if(!state.user) return alert('Please login first.');
  const amt = parseInt(depAmount.value);
  const txn = depTxn.value.trim();
  const file = depProof.files[0];
  if(!amt || !txn || !file) return alert('Provide amount, txn id, and proof.');
  // start plan if selected
  if(state.plan && !state.plan.startTime){
    state.plan.startTime = new Date().toISOString();
    state.plan.count = 0;
    state.activities.unshift(`Plan started: ${state.plan.name} at ${new Date().toLocaleString()}`);
  }
  state.balance += amt;
  state.activities.unshift(`Deposit: PKR ${amt} via ${chosenDeposit.method} TXN:${txn}`);
  saveAll();
  renderAll();
  alert('Deposit submitted (simulation). Balance updated.');
  depAmount.value=''; depTxn.value=''; depProof.value=null;
  depositForm.classList.add('hidden');
});

/* Withdraw logic */
submitWithdraw.addEventListener('click', ()=> {
  if(!state.user) return alert('Please login first.');
  const amt = parseInt(wdAmount.value);
  const to = wdTo.value.trim();
  const method = document.querySelector('input[name="wMethod"]:checked').value;
  if(!amt || !to) return alert('Enter amount and recipient.');
  if(amt > state.balance) return alert('Insufficient balance.');
  state.balance -= amt;
  state.activities.unshift(`Withdrawal requested: PKR ${amt} via ${method} to ${to}`);
  saveAll();
  renderAll();
  alert('Withdrawal request submitted (simulation).');
  wdAmount.value=''; wdTo.value=''; wdNote.value='';
});

/* Activity */
function renderActivities(){
  activityList.innerHTML = '';
  if(state.activities.length === 0){
    activityList.innerHTML = '<div class="p-4 bg-white rounded shadow text-sm text-slate-500">No activity yet.</div>';
    return;
  }
  state.activities.forEach(a=>{
    const d = document.createElement('div');
    d.className = 'p-3 bg-white rounded shadow text-sm';
    d.textContent = a;
    activityList.appendChild(d);
  });
}
clearActivity.addEventListener('click', ()=> {
  if(!confirm('Clear activity history?')) return;
  state.activities = [];
  saveAll();
  renderActivities();
});

/* Nav & header actions */
navHome.addEventListener('click', ()=> switchTo('homeTab'));
navPlans.addEventListener('click', ()=> switchTo('plansTab'));
navDeposit.addEventListener('click', ()=> switchTo('depositTab'));
navWithdraw.addEventListener('click', ()=> switchTo('withdrawTab'));
navActivity.addEventListener('click', ()=> switchTo('activityTab'));
navCompany.addEventListener('click', ()=> switchTo('companyTab'));

gotoPlans.addEventListener('click', ()=> switchTo('plansTab'));
gotoDeposit.addEventListener('click', ()=> switchTo('depositTab'));
gotoWithdraw.addEventListener('click', ()=> switchTo('withdrawTab'));

/* Profile / Share / Logout */
profileBtn.addEventListener('click', ()=> {
  if(!state.user) return alert('Login first');
  alert(`Name: ${state.user.name}\nEmail: ${state.user.email}\nPhone: ${state.user.phone}\nBalance: PKR ${state.balance}`);
});
shareBtn.addEventListener('click', ()=> {
  if(!state.user) return alert('Login first');
  const link = `${location.origin}${location.
