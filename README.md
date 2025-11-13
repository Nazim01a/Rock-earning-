<ROCK EARNING>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Rock Earn LLC — Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{--accent1:#6d28d9;--accent2:#3b82f6;}
  body{font-family:Inter,system-ui,Segoe UI,Roboto,Arial; background:linear-gradient(180deg,#f8fafc,#eef2ff);}
  .logo-mark{font-weight:800;letter-spacing:1px;}
  .card-hover:hover{transform:translateY(-6px);box-shadow:0 12px 30px rgba(0,0,0,0.08);transition:0.22s;}
  .btn-grad{background:linear-gradient(90deg,var(--accent1),var(--accent2));}
  .hidden{display:none;}
  .copy-box{background:#f3f4f6;padding:10px;border-radius:8px;text-align:center;font-weight:700;}
</style>
</head>
<body class="min-h-screen">

<!-- HEADER -->
<header id="topHeader" class="w-full shadow-sm">
  <div class="max-w-6xl mx-auto flex items-center justify-between p-4">
    <div class="flex items-center gap-3">
      <div class="w-12 h-12 rounded-xl flex items-center justify-center" style="background:linear-gradient(135deg,var(--accent1),var(--accent2)); color:white;">
        <span class="logo-mark text-lg">R</span>
      </div>
      <div>
        <div id="brandTitle" class="text-lg font-extrabold text-slate-700">Rock Earn LLC — California (Established 2018)</div>
        <div class="text-xs text-slate-500">Professional simulated dashboard</div>
      </div>
    </div>

    <div id="headerRight" class="flex items-center gap-3">
      <!-- shown only after login -->
      <button id="navHome" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="homeTab">Home</button>
      <button id="navPlans" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="plansTab">Plans</button>
      <button id="navDeposit" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="depositTab">Deposit</button>
      <button id="navWithdraw" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="withdrawTab">Withdraw</button>
      <button id="navActivity" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="activityTab">Activity</button>
      <button id="navCompany" class="hidden text-sm px-3 py-1 rounded-md hover:bg-slate-100" data-tab="companyTab">Company</button>

      <button id="profileBtn" class="hidden text-sm px-3 py-1 rounded-md border">Profile</button>
      <button id="shareBtn" class="hidden text-sm px-3 py-1 rounded-md border">Share</button>
      <button id="logoutBtn" class="hidden text-sm px-3 py-1 rounded-md bg-red-500 text-white">Logout</button>

      <button id="loginOpenBtn" class="px-3 py-1 rounded-md btn-grad text-white">Login / Sign Up</button>
    </div>
  </div>
</header>

<main class="max-w-6xl mx-auto p-6">

  <!-- LOGIN / SIGN UP (start screen) -->
  <section id="landingScreen" class="min-h-[300px] flex items-center justify-center">
    <div class="w-full max-w-md bg-white rounded-xl p-6 shadow card-hover text-center">
      <h2 class="text-2xl font-bold mb-2">Welcome to Rock Earn</h2>
      <p class="text-sm text-slate-500 mb-4">Login or Sign up to access the dashboard</p>
      <div class="flex gap-3 justify-center">
        <button id="loginBtn" class="px-4 py-2 rounded-md btn-grad text-white">Login</button>
        <button id="signupBtn" class="px-4 py-2 rounded-md border">Sign Up</button>
      </div>
    </div>
  </section>

  <!-- TABS (hidden until login) -->
  <section id="appArea" class="hidden">

    <!-- HOME -->
    <div id="homeTab" class="tab hidden">
      <div class="grid md:grid-cols-3 gap-4 mb-6">
        <div class="col-span-2 p-6 rounded-xl bg-white shadow">
          <div class="flex justify-between items-start">
            <div>
              <h3 class="text-2xl font-bold">Welcome, <span id="userName">User</span></h3>
              <p class="text-sm text-slate-500">Dashboard overview</p>
            </div>
            <div class="text-right">
              <div class="text-xs text-slate-400">Account Balance</div>
              <div id="accountBalance" class="text-2xl font-extrabold">PKR 0</div>
            </div>
          </div>
          <div class="mt-4 text-sm text-slate-500">Select a plan, then deposit proof to start earning simulation profit daily.</div>
        </div>
        <div class="p-6 rounded-xl bg-white shadow">
          <div class="text-sm text-slate-600">Quick Actions</div>
          <div class="mt-4 flex flex-col gap-3">
            <button id="btnShowPlans" class="px-3 py-2 rounded-md btn-grad text-white">Browse Plans</button>
            <button id="btnShowDeposit" class="px-3 py-2 rounded-md bg-green-500 text-white">Deposit</button>
            <button id="btnShowWithdraw" class="px-3 py-2 rounded-md bg-amber-500 text-white">Withdraw</button>
          </div>
        </div>
      </div>
    </div>

    <!-- PLANS -->
    <div id="plansTab" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Available Plans</h3>
        <div class="text-sm text-slate-500">Choose & then submit deposit to start</div>
      </div>

      <div id="plansGrid" class="grid md:grid-cols-3 gap-4"></div>
    </div>

    <!-- DEPOSIT -->
    <div id="depositTab" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Deposit</h3>
        <div class="text-sm text-slate-500">Select method, copy number, pay and upload proof</div>
      </div>

      <div class="grid md:grid-cols-2 gap-6">
        <div class="p-6 bg-white rounded-xl shadow">
          <div class="text-sm text-slate-600">Methods</div>
          <div class="mt-3 flex flex-col gap-3">
            <button class="depositMethod px-4 py-2 rounded-md bg-green-500 text-white" data-method="Easypaisa" data-number="03379827882">Easypaisa</button>
            <button class="depositMethod px-4 py-2 rounded-md bg-blue-500 text-white" data-method="JazzCash" data-number="03705519562">JazzCash</button>
          </div>

          <div id="depositMethodBox" class="mt-4 hidden">
            <div id="depositNumberBox" class="copy-box mb-2">—</div>
            <div class="flex gap-2">
              <button id="copyDepositBtn" class="px-3 py-1 rounded-md bg-indigo-600 text-white">Copy Number</button>
              <button id="openDepositFormBtn" class="px-3 py-1 rounded-md border">Proceed to Deposit</button>
            </div>
          </div>
        </div>

        <div id="depositFormCard" class="p-6 bg-white rounded-xl shadow hidden">
          <h4 class="font-semibold">Submit Deposit Proof</h4>
          <div class="mt-3 flex flex-col gap-3">
            <input id="depositAmount" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
            <input id="depositTxn" placeholder="Transaction ID / Ref" class="p-2 rounded border" />
            <input id="depositProof" type="file" accept="image/*" />
            <button id="submitDepositBtn" class="px-4 py-2 rounded-md btn-grad text-white">Submit Deposit</button>
          </div>
        </div>
      </div>
    </div>

    <!-- WITHDRAW -->
    <div id="withdrawTab" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Withdraw</h3>
        <div class="text-sm text-slate-500">JazzCash / Easypaisa / Bank</div>
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
            <input id="withdrawAmount" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
            <input id="withdrawTo" placeholder="Recipient Number / IBAN" class="p-2 rounded border" />
            <textarea id="withdrawNote" placeholder="Note (optional)" class="p-2 rounded border"></textarea>
            <button id="submitWithdrawBtn" class="px-4 py-2 rounded-md bg-amber-500 text-white">Submit Withdrawal</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ACTIVITY -->
    <div id="activityTab" class="tab hidden">
      <div class="mb-4 flex items-center justify-between">
        <h3 class="text-xl font-bold">Activity History</h3>
        <div>
          <button id="clearActivityBtn" class="px-3 py-1 rounded-md border">Clear</button>
        </div>
      </div>
      <div id="activityList" class="space-y-2"></div>
    </div>

    <!-- COMPANY -->
    <div id="companyTab" class="tab hidden p-6 bg-white rounded-xl shadow">
      <h3 class="text-lg font-bold">Company Details</h3>
      <p class="text-sm text-slate-600 mt-2">Rock Earn LLC — California (Established 2018). This is a simulated demo interface. For support: <strong>support@rockearn.com</strong></p>
      <div class="text-sm text-slate-500 mt-3">Address: 123 Demo Street, San Francisco, CA. Legal: Rock Earn LLC (Demo)</div>
    </div>

  </section>

  <footer class="mt-8 text-center text-xs text-slate-400">© Rock Earn LLC — Demo interface</footer>
</main>

<!-- AUTH MODAL (simple) -->
<div id="authModal" class="fixed inset-0 flex items-center justify-center hidden bg-black/30">
  <div class="bg-white rounded-xl shadow-lg w-full max-w-md p-6">
    <h3 id="authTitle" class="text-xl font-bold mb-2">Login / Sign Up</h3>
    <form id="authForm" class="flex flex-col gap-3">
      <input id="inputName" name="name" placeholder="Full name" class="p-2 rounded border" required />
      <input id="inputEmail" name="email" type="email" placeholder="Email" class="p-2 rounded border" required />
      <input id="inputPass" name="password" type="password" placeholder="Password" class="p-2 rounded border" required />
      <div class="flex items-center gap-2"><input id="inputAgree" type="checkbox" required /><label class="text-sm text-slate-500">I agree to terms</label></div>
      <div class="flex gap-2 mt-3">
        <button type="submit" class="px-4 py-2 rounded-md btn-grad text-white">Submit</button>
        <button id="closeAuthBtn" type="button" class="px-4 py-2 rounded-md border">Cancel</button>
      </div>
    </form>
  </div>
</div>

<script>
/* ---------- STATE & SELECTORS ---------- */
const state = {
  user: null,
  balance: 0,
  plan: null, // {name,price,profit,duration,startTime,count}
  activities: []
};

const PLANS = [
  {name:'Plan A', price:180, profit:35, duration:30},
  {name:'Plan B', price:280, profit:55, duration:30},
  {name:'Plan C', price:500, profit:110, duration:30},
  {name:'Plan D', price:1000, profit:240, duration:30},
  {name:'Plan E', price:2000, profit:480, duration:35},
  {name:'Plan F', price:3000, profit:700, duration:35},
  {name:'Plan G', price:5000, profit:1200, duration:40}
];

const elems = {
  loginOpenBtn: document.getElementById('loginOpenBtn') || document.getElementById('loginOpenBtn'),
  loginBtn: document.getElementById('loginBtn'),
  signupBtn: document.getElementById('signupBtn'),
  landingScreen: document.getElementById('landingScreen'),
  appArea: document.getElementById('appArea'),
  authModal: document.getElementById('authModal'),
  authForm: document.getElementById('authForm'),
  closeAuthBtn: document.getElementById('closeAuthBtn'),
  inputName: document.getElementById('inputName'),
  inputEmail: document.getElementById('inputEmail'),
  inputPass: document.getElementById('inputPass'),
  inputAgree: document.getElementById('inputAgree'),
  headerRight: document.getElementById('headerRight'),
  profileBtn: document.getElementById('profileBtn'),
  shareBtn: document.getElementById('shareBtn'),
  logoutBtn: document.getElementById('logoutBtn'),
  userNameEl: document.getElementById('userName'),
  accountBalanceEl: document.getElementById('accountBalance'),

  // nav
  navHome: document.getElementById('navHome'),
  navPlans: document.getElementById('navPlans'),
  navDeposit: document.getElementById('navDeposit'),
  navWithdraw: document.getElementById('navWithdraw'),
  navActivity: document.getElementById('navActivity'),
  navCompany: document.getElementById('navCompany'),

  // tabs & quick
  tabs: document.querySelectorAll('.tab'),
  btnShowPlans: document.getElementById('btnShowPlans'),
  btnShowDeposit: document.getElementById('btnShowDeposit'),
  btnShowWithdraw: document.getElementById('btnShowWithdraw'),

  plansGrid: document.getElementById('plansGrid'),

  // deposit
  depositMethodButtons: document.querySelectorAll('.depositMethod'),
  depositMethodBox: document.getElementById('depositMethodBox'),
  depositNumberBox: document.getElementById('depositNumberBox'),
  copyDepositBtn: document.getElementById('copyDepositBtn'),
  openDepositFormBtn: document.getElementById('openDepositFormBtn'),
  depositFormCard: document.getElementById('depositFormCard'),
  depositAmountInput: document.getElementById('depositAmount'),
  depositTxnInput: document.getElementById('depositTxn'),
  depositProofInput: document.getElementById('depositProof'),
  submitDepositBtn: document.getElementById('submitDepositBtn'),

  // withdraw
  submitWithdrawBtn: document.getElementById('submitWithdrawBtn'),
  withdrawAmountInput: document.getElementById('withdrawAmount'),
  withdrawToInput: document.getElementById('withdrawTo'),
  withdrawNoteInput: document.getElementById('withdrawNote'),

  // activity
  activityList: document.getElementById('activityList'),
  clearActivityBtn: document.getElementById('clearActivityBtn'),
};

/* ---------- UTILS ---------- */
function saveAll(){
  localStorage.setItem('rk_user', JSON.stringify(state.user));
  localStorage.setItem('rk_balance', String(state.balance));
  localStorage.setItem('rk_plan', JSON.stringify(state.plan));
  localStorage.setItem('rk_acts', JSON.stringify(state.activities));
}
function loadAll(){
  const u = JSON.parse(localStorage.getItem('rk_user'));
  if(u) state.user = u;
  const bal = parseInt(localStorage.getItem('rk_balance'));
  if(!isNaN(bal)) state.balance = bal;
  const p = JSON.parse(localStorage.getItem('rk_plan'));
  if(p) state.plan = p;
  const a = JSON.parse(localStorage.getItem('rk_acts'));
  if(a) state.activities = a;
}

/* ---------- NAV & UI ---------- */
function showOnlyLogin(){
  document.getElementById('landingScreen').classList.remove('hidden');
  document.getElementById('appArea').classList.add('hidden');
  // header buttons hide (except login)
  ['navHome','navPlans','navDeposit','navWithdraw','navActivity','navCompany','profileBtn','shareBtn','logoutBtn'].forEach(id=>{
    const el = document.getElementById(id);
    if(el) el.classList.add('hidden');
  });
  document.getElementById('loginOpenBtn')?.classList?.remove('hidden');
}
function showAppArea(){
  document.getElementById('landingScreen').classList.add('hidden');
  document.getElementById('appArea').classList.remove('hidden');
  // show header nav & profile & logout
  ['navHome','navPlans','navDeposit','navWithdraw','navActivity','navCompany','profileBtn','shareBtn','logoutBtn'].forEach(id=>{
    const el = document.getElementById(id);
    if(el) el.classList.remove('hidden');
  });
  document.getElementById('loginOpenBtn')?.classList?.add('hidden');
  // default to home
  switchTab('homeTab');
}
function switchTab(tabId){
  document.querySelectorAll('.tab').forEach(t=>t.classList.add('hidden'));
  const el = document.getElementById(tabId);
  if(el) el.classList.remove('hidden');
  window.scrollTo({top:0,behavior:'smooth'});
}

/* ---------- PLANS RENDER ---------- */
function renderPlans(){
  elems.plansGrid.innerHTML = '';
  PLANS.forEach(p=>{
    const card = document.createElement('div');
    card.className = 'p-4 rounded-xl bg-white shadow card-hover flex flex-col gap-3';
    card.innerHTML = `
      <div class="text-sm text-slate-500">${p.name}</div>
      <div class="font-bold text-2xl">PKR ${p.price}</div>
      <div class="text-xs text-slate-400">Daily Profit PKR ${p.profit} • ${p.duration} days</div>
      <div class="mt-auto flex gap-2">
        <button class="px-3 py-2 rounded-md btn-grad text-white buyPlanBtn">Buy</button>
        <button class="px-3 py-2 rounded-md border showPlanBtn">Details</button>
      </div>`;
    elems.plansGrid.appendChild(card);

    card.querySelector('.buyPlanBtn').addEventListener('click', ()=>{
      if(!state.user) return alert('Please login/signup first.');
      state.plan = {...p, startTime: null, count: 0};
      state.activities.unshift(`Plan selected: ${p.name} — PKR ${p.price}`);
      saveAll();
      alert(`Plan ${p.name} selected. Now go to Deposit and submit proof to start.`);
      switchTab('depositTab');
      renderAll();
    });
    card.querySelector('.showPlanBtn').addEventListener('click', ()=>{
      alert(`${p.name}\nPrice: PKR ${p.price}\nDaily: PKR ${p.profit}\nDuration: ${p.duration} days`);
    });
  });
}

/* ---------- AUTH ---------- */
document.getElementById('loginOpenBtn')?.addEventListener('click', ()=> {
  elems.authModal.classList.remove('hidden');
});
elems.loginBtn.addEventListener('click', ()=> elems.authModal.classList.remove('hidden'));
elems.signupBtn.addEventListener('click', ()=> elems.authModal.classList.remove('hidden'));
elems.closeAuthBtn.addEventListener('click', ()=> elems.authModal.classList.add('hidden'));

elems.authForm.addEventListener('submit', (e)=>{
  e.preventDefault();
  const name = elems.inputName.value.trim();
  const email = elems.inputEmail.value.trim();
  if(!name||!email) return alert('Name and email required.');
  state.user = {name, email};
  // persist
  saveAll();
  elems.authModal.classList.add('hidden');
  // show app area
  afterLogin();
});

/* ---------- AFTER LOGIN ---------- */
function afterLogin(){
  elems.userNameEl.textContent = state.user.name;
  showAppArea();
  renderAll();
}

/* ---------- PROFILE & SHARE & LOGOUT ---------- */
elems.profileBtn.addEventListener('click', ()=>{
  if(!state.user) return alert('Login first');
  alert(`Name: ${state.user.name}\nEmail: ${state.user.email}\nBalance: PKR ${state.balance}`);
});
elems.shareBtn.addEventListener('click', ()=>{
  if(!state.user) return alert('Login first');
  const link = `${location.origin}${location.pathname}?ref=${encodeURIComponent(state.user.name)}`;
  navigator.clipboard.writeText(link);
  alert('Share link copied!');
});
elems.logoutBtn.addEventListener('click', ()=>{
  if(!confirm('Logout?')) return;
  // clear user but keep activities/balance maybe — here we clear user only and keep data
  state.user = null;
  localStorage.removeItem('rk_user');
  showOnlyLogin();
  renderAll();
});

/* ---------- NAV BUTTONS ---------- */
['navHome','navPlans','navDeposit','navWithdraw','navActivity','navCompany'].forEach(id=>{
  const el = document.getElementById(id);
  if(el) el.addEventListener('click', ()=> switchTab(el.dataset.tab || el.id.replace('nav','').toLowerCase()+'Tab'));
});
elems.btnShowPlans.addEventListener('click', ()=> switchTab('plansTab'));
elems.btnShowDeposit.addEventListener('click', ()=> switchTab('depositTab'));
elems.btnShowWithdraw.addEventListener('click', ()=> switchTab('withdrawTab'));

/* ---------- DEPOSIT ---------- */
let chosenDeposit = null;
document.querySelectorAll('.depositMethod').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    chosenDeposit = {method: btn.dataset.method, number: btn.dataset.number};
    elems.depositMethodBox.classList.remove('hidden');
    elems.depositNumberBox.te
