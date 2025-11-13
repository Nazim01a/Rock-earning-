<ROCK>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Rock Earn — Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{--accent1:#6d28d9;--accent2:#06b6d4;--card:#ffffff;}
  body{font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;}
  .logo-mark{font-weight:800;letter-spacing:1px;}
  .nav-item{cursor:pointer}
  .glass{backdrop-filter: blur(6px); background: rgba(255,255,255,0.6);}
  .card-hover:hover{transform:translateY(-6px); box-shadow:0 12px 30px rgba(0,0,0,0.12); transition:0.25s;}
  .btn-grad{background:linear-gradient(90deg,var(--accent1),#3b82f6);}
  .hidden{display:none;}
  .copy-box{background:#f3f4f6;padding:10px;border-radius:8px;text-align:center;font-weight:700;}
</style>
</head>
<body class="bg-gradient-to-br from-slate-50 to-slate-100 min-h-screen">

<!-- Header / Topbar -->
<header class="w-full shadow-md">
  <div class="max-w-6xl mx-auto flex items-center justify-between p-4">
    <div class="flex items-center gap-4">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 rounded-xl flex items-center justify-center" style="background:linear-gradient(135deg,var(--accent1),var(--accent2)); color:white;">
          <span class="logo-mark text-lg">R</span>
        </div>
        <div>
          <div class="text-xl font-extrabold text-slate-700">Rock <span style="color:var(--accent1)">Earn</span></div>
          <div class="text-xs text-slate-500">Professional Earning Dashboard</div>
        </div>
      </div>
    </div>

    <!-- Menu -->
    <nav class="hidden md:flex items-center gap-3 text-sm text-slate-600">
      <div class="nav-item px-3 py-2 rounded-md hover:bg-slate-100" data-tab="homeTab">Home</div>
      <div class="nav-item px-3 py-2 rounded-md hover:bg-slate-100" data-tab="plansTab">Plans</div>
      <div class="nav-item px-3 py-2 rounded-md hover:bg-slate-100" data-tab="depositTab">Deposit</div>
      <div class="nav-item px-3 py-2 rounded-md hover:bg-slate-100" data-tab="withdrawTab">Withdraw</div>
      <div class="nav-item px-3 py-2 rounded-md hover:bg-slate-100" data-tab="activityTab">Activity</div>
      <div class="nav-item px-3 py-2 rounded-md hover:bg-slate-100" data-tab="companyTab">Company</div>
    </nav>

    <!-- Right buttons -->
    <div class="flex items-center gap-3">
      <button id="shareBtn" class="hidden px-3 py-2 rounded-md border text-sm">Share</button>
      <button id="profileBtn" class="hidden px-3 py-2 rounded-md border text-sm">Profile</button>
      <button id="loginTrigger" class="px-3 py-2 rounded-md btn-grad text-white text-sm">Login / Signup</button>
    </div>
  </div>
</header>

<main class="max-w-6xl mx-auto p-6">

  <!-- MAIN LAYOUT -->
  <div id="homeTab" class="tab-content">

    <!-- Greeting + balance -->
    <div class="grid md:grid-cols-3 gap-4 mb-6">
      <div class="col-span-2 p-6 rounded-xl glass">
        <div class="flex items-center justify-between">
          <div>
            <h2 id="welcomeTxt" class="text-2xl font-bold text-slate-700">Welcome, <span id="userName">Guest</span></h2>
            <p class="text-sm text-slate-500 mt-1">Dashboard overview — buy plans, deposit, withdraw, and track activity.</p>
          </div>
          <div class="text-right">
            <div class="text-xs text-slate-200">Account Balance</div>
            <div id="accountBalance" class="text-2xl font-extrabold text-slate-800">PKR 0</div>
          </div>
        </div>
        <div class="mt-4 text-sm text-slate-500">Tip: Select a plan and submit deposit proof to simulate starting the plan. Daily profit adds automatically.</div>
      </div>

      <!-- Quick Actions -->
      <div class="p-6 rounded-xl bg-white shadow card-hover">
        <div class="text-sm text-slate-500">Quick Actions</div>
        <div class="mt-4 flex flex-col gap-3">
          <button id="goPlans" class="px-4 py-2 rounded-md btn-grad text-white">Browse Plans</button>
          <button id="goDeposit" class="px-4 py-2 rounded-md bg-green-500 text-white">Deposit</button>
          <button id="goWithdraw" class="px-4 py-2 rounded-md bg-amber-500 text-white">Withdraw</button>
        </div>
      </div>
    </div>

  </div>

  <!-- PLANS -->
  <section id="plansTab" class="tab-content hidden">
    <div class="flex items-center justify-between mb-4">
      <h3 class="text-xl font-bold">Available Plans</h3>
      <div class="text-sm text-slate-500">Choose one and then submit deposit (simulation) to start.</div>
    </div>

    <div class="grid md:grid-cols-3 gap-4">
      <!-- cards: 180, 280, 500, 1000, 2000, 3000, 5000 -->
      <template id="plan-template">
        <div class="p-4 rounded-xl bg-white shadow card-hover flex flex-col gap-3">
          <div class="text-sm text-slate-500 plan-name"></div>
          <div class="font-bold text-2xl plan-price"></div>
          <div class="text-xs text-slate-400 plan-meta"></div>
          <div class="mt-auto flex gap-2">
            <button class="buy-btn px-3 py-2 rounded-md btn-grad text-white text-sm">Buy</button>
            <button class="share-plan px-3 py-2 rounded-md border text-sm">Share</button>
          </div>
        </div>
      </template>
    </div>
  </section>

  <!-- DEPOSIT -->
  <section id="depositTab" class="tab-content hidden">
    <div class="flex items-center justify-between mb-4">
      <h3 class="text-xl font-bold">Deposit</h3>
      <div class="text-sm text-slate-500">Select a method, copy number, pay and upload proof.</div>
    </div>

    <div class="grid md:grid-cols-2 gap-6">
      <div class="p-6 rounded-xl bg-white shadow card-hover">
        <div class="text-sm text-slate-600">Select Method</div>
        <div class="mt-3 flex flex-col gap-3">
          <button class="deposit-method px-4 py-2 rounded-md bg-green-500 text-white" data-method="Easypaisa" data-num="03379827882">Easypaisa</button>
          <button class="deposit-method px-4 py-2 rounded-md bg-blue-500 text-white" data-method="JazzCash" data-num="03705519562">JazzCash</button>
        </div>

        <div id="depositInfo" class="mt-4 hidden">
          <div class="copy-box mb-2" id="depositNumberDisplay">—</div>
          <div class="flex items-center gap-2">
            <button id="copyDepositNumber" class="px-3 py-1 rounded-md bg-indigo-600 text-white">Copy Number</button>
            <button id="showDepositForm" class="px-3 py-1 rounded-md border">Proceed to Deposit</button>
          </div>
        </div>

      </div>

      <div id="depositFormCard" class="p-6 rounded-xl bg-white shadow hidden">
        <h4 class="font-semibold">Submit Deposit Proof</h4>
        <div class="mt-3 text-sm text-slate-500">Amount, transaction ID and screenshot/proof.</div>
        <div class="mt-3 flex flex-col gap-3">
          <input id="depositAmount" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
          <input id="depositTxn" placeholder="Transaction ID / Reference" class="p-2 rounded border" />
          <input id="depositProof" type="file" accept="image/*" />
          <button id="submitDeposit" class="px-4 py-2 rounded-md bg-gradient-to-r from-orange-500 to-pink-500 text-white">Submit Deposit</button>
        </div>
      </div>
    </div>
  </section>

  <!-- WITHDRAW -->
  <section id="withdrawTab" class="tab-content hidden">
    <div class="flex items-center justify-between mb-4">
      <h3 class="text-xl font-bold">Withdraw</h3>
      <div class="text-sm text-slate-500">Choose method and submit withdrawal request.</div>
    </div>

    <div class="grid md:grid-cols-2 gap-6">
      <div class="p-6 rounded-xl bg-white shadow card-hover">
        <div class="text-sm text-slate-500">Select withdrawal method</div>
        <div class="mt-3 flex flex-col gap-3">
          <label class="flex items-center gap-2"><input type="radio" name="wmethod" value="easypaisa" checked> Easypaisa</label>
          <label class="flex items-center gap-2"><input type="radio" name="wmethod" value="jazzcash"> JazzCash</label>
          <label class="flex items-center gap-2"><input type="radio" name="wmethod" value="bank"> Bank Transfer</label>
        </div>
      </div>

      <div class="p-6 rounded-xl bg-white shadow card-hover">
        <h4 class="font-semibold">Request Withdrawal</h4>
        <div class="mt-3 flex flex-col gap-3">
          <input id="withdrawAmount" type="number" placeholder="Amount (PKR)" class="p-2 rounded border" />
          <input id="withdrawTo" placeholder="Recipient Number / IBAN" class="p-2 rounded border" />
          <textarea id="withdrawNote" placeholder="Note (optional)" class="p-2 rounded border"></textarea>
          <button id="submitWithdraw" class="px-4 py-2 rounded-md bg-amber-500 text-white">Submit Withdrawal</button>
        </div>
      </div>
    </div>
  </section>

  <!-- ACTIVITY -->
  <section id="activityTab" class="tab-content hidden">
    <div class="mb-4 flex items-center justify-between">
      <h3 class="text-xl font-bold">Activity History</h3>
      <button id="clearActivities" class="px-3 py-1 rounded-md border text-sm">Clear</button>
    </div>
    <div id="activityList" class="space-y-2"></div>
  </section>

  <!-- COMPANY -->
  <section id="companyTab" class="tab-content hidden p-6 rounded-xl bg-white shadow mt-6">
    <h3 class="text-lg font-bold mb-2">Company Details</h3>
    <p class="text-sm text-slate-600">Rock Earn LLC — Founded 2018, California. This is a simulated interface for demonstration purposes. For support contact <strong>support@rockearn.com</strong>.</p>
    <div class="mt-4 text-sm text-slate-500">Address (example): 123 Silicon Ave, San Francisco, CA. <br>Legal: Rock Earn LLC (Demo)</div>
  </section>

  <!-- FOOTER -->
  <footer class="mt-8 text-center text-xs text-slate-400">© Rock Earn — Demo interface. Not a real investment platform.</footer>
</main>

<!-- AUTH MODAL (lightweight) -->
<div id="authModal" class="fixed inset-0 flex items-center justify-center hidden">
  <div class="bg-white rounded-xl shadow-lg w-full max-w-md p-6">
    <h3 id="authTitle" class="text-xl font-bold mb-2">Login / Sign Up</h3>
    <form id="authFormModal" class="flex flex-col gap-3">
      <input id="authName" name="name" placeholder="Full name" class="p-2 rounded border" required />
      <input id="authEmail" name="email" type="email" placeholder="Email" class="p-2 rounded border" required />
      <input id="authPass" name="password" type="password" placeholder="Password" class="p-2 rounded border" required />
      <div class="flex items-center gap-2"><input id="authAgree" type="checkbox" required /><label class="text-sm text-slate-500">I agree to terms</label></div>
      <div class="flex gap-2 mt-3">
        <button type="submit" class="px-4 py-2 rounded-md btn-grad text-white">Submit</button>
        <button type="button" id="closeAuth" class="px-4 py-2 rounded-md border">Cancel</button>
      </div>
    </form>
  </div>
</div>

<script>
// ---------- STATE ----------
const state = {
  user: null,
  balance: 0,
  plan: null, // {name,price,profit,duration,startTime,count}
  activities: []
};

// Predefined plans
const PLANS = [
  {name:'Basic', price:180, profit:35, duration:30},
  {name:'Starter+', price:280, profit:55, duration:30},
  {name:'Silver', price:500, profit:110, duration:30},
  {name:'Pro', price:1000, profit:240, duration:30},
  {name:'Gold', price:2000, profit:480, duration:35},
  {name:'Platinum', price:3000, profit:700, duration:35},
  {name:'VIP', price:5000, profit:1200, duration:40}
];

// --------- ELEMENTS ----------
const tabs = document.querySelectorAll('.tab-content');
const navItems = document.querySelectorAll('.nav-item');
const loginTrigger = document.getElementById('loginTrigger');
const authModal = document.getElementById('authModal');
const authFormModal = document.getElementById('authFormModal');
const closeAuth = document.getElementById('closeAuth');
const welcomeTxt = document.getElementById('userName');
const accountBalanceEl = document.getElementById('accountBalance');
const profileBtn = document.getElementById('profileBtn');
const shareBtn = document.getElementById('shareBtn');
const loginTriggerBtn = document.getElementById('loginTrigger');

// plan area
const planTemplate = document.getElementById('plan-template');
const plansContainer = document.querySelector('#plansTab .grid');

// deposit
const depositMethods = document.querySelectorAll('.deposit-method');
const depositInfo = document.getElementById('depositInfo');
const depositNumberDisplay = document.getElementById('depositNumberDisplay');
const copyDepositNumber = document.getElementById('copyDepositNumber');
const showDepositForm = document.getElementById('showDepositForm');
const depositFormCard = document.getElementById('depositFormCard');
const submitDeposit = document.getElementById('submitDeposit');
const depositAmountInput = document.getElementById('depositAmount');
const depositTxnInput = document.getElementById('depositTxn');
const depositProofInput = document.getElementById('depositProof');

// withdraw
const submitWithdraw = document.getElementById('submitWithdraw');
const withdrawAmountInput = document.getElementById('withdrawAmount');
const withdrawToInput = document.getElementById('withdrawTo');
const withdrawNoteInput = document.getElementById('withdrawNote');

// activity
const activityList = document.getElementById('activityList');
const clearActivitiesBtn = document.getElementById('clearActivities');

// quick actions
document.getElementById('goPlans').onclick = ()=> switchTo('plansTab');
document.getElementById('goDeposit').onclick = ()=> switchTo('depositTab');
document.getElementById('goWithdraw').onclick = ()=> switchTo('withdrawTab');
document.getElementById('goPlans').onclick = ()=> switchTo('plansTab');

// ---------- UTIL ----------
function switchTo(tabId){
  tabs.forEach(t=>t.classList.add('hidden'));
  const el = document.getElementById(tabId);
  if(el) el.classList.remove('hidden');
  // active menu highlight (optional)
  navItems.forEach(n=> n.classList.toggle('bg-slate-100', n.dataset.tab===tabId));
  window.scrollTo({top:0,behavior:'smooth'});
}

navItems.forEach(n=>{
  n.addEventListener('click', ()=> switchTo(n.dataset.tab));
});

// ---------- AUTH ----------
loginTrigger.addEventListener('click', ()=> {
  authModal.classList.remove('hidden');
});
closeAuth.addEventListener('click', ()=> authModal.classList.add('hidden'));

authFormModal.addEventListener('submit', (e)=>{
  e.preventDefault();
  const name = document.getElementById('authName').value.trim();
  const email = document.getElementById('authEmail').value.trim();
  if(!name||!email){ alert('Please enter name and email'); return; }
  state.user = {name, email};
  // persist
  localStorage.setItem('rock_user', JSON.stringify(state.user));
  authModal.classList.add('hidden');
  afterLogin();
});

// show profile & share only after login
function afterLogin(){
  welcomeTxt.textContent = state.user ? state.user.name : 'Guest';
  profileBtn.classList.remove('hidden');
  shareBtn.classList.remove('hidden');
  loginTrigger.classList.add('hidden');
  // load persisted balance/plan/activities if exist
  const bal = parseInt(localStorage.getItem('rock_balance'));
  const plan = JSON.parse(localStorage.getItem('rock_plan'));
  const activities = JSON.parse(localStorage.getItem('rock_activities'));
  if(!isNaN(bal)) state.balance = bal;
  if(plan) state.plan = plan;
  if(activities) state.activities = activities;
  renderAll();
}

// profile & share
profileBtn.addEventListener('click', ()=>{
  if(!state.user) return alert('Login first');
  alert(`Name: ${state.user.name}\nEmail: ${state.user.email}\nBalance: PKR ${state.balance}`);
});
shareBtn.addEventListener('click', ()=>{
  if(!state.user) return alert('Login first');
  const link = state.user ? `${location.origin}${location.pathname}?ref=${encodeURIComponent(state.user.name)}` : location.href;
  navigator.clipboard.writeText(link);
  alert('Share link copied!');
});

// ---------- PLANS RENDER ----------
function renderPlans(){
  const grid = document.querySelector('#plansTab .grid');
  grid.innerHTML = '';
  PLANS.forEach(p=>{
    const node = planTemplate.content.cloneNode(true);
    node.querySelector('.plan-name').textContent = p.name;
    node.querySelector('.plan-price').textContent = `PKR ${p.price}`;
    node.querySelector('.plan-meta').textContent = `Daily Profit PKR ${p.profit} • Duration ${p.duration} days`;
    const buyBtn = node.querySelector('.buy-btn');
    buyBtn.addEventListener('click', ()=> {
      if(!state.user) return alert('Please login/signup first.');
      // set plan in state (not active until deposit submitted)
      state.plan = {...p, startTime: null, count:0};
      localStorage.setItem('rock_plan', JSON.stringify(state.plan));
      state.activities.unshift(`Plan selected: ${p.name} — PKR ${p.price}`);
      persistActivities();
      alert(`Plan ${p.name} selected. Now go to Deposit to submit proof.`)
      switchTo('depositTab');
      renderAll();
    });
    node.querySelector('.share-plan').addEventListener('click', ()=>{
      if(!state.user){ alert('Login to share plan'); return; }
      navigator.clipboard.writeText(`Check this plan: ${p.name} PKR ${p.price}`);
      alert('Plan details copied to clipboard');
    });
    grid.appendChild(node);
  });
}

// ---------- DEPOSIT LOGIC ----------
let currentDepositMethod = null;
depositMethods.forEach(btn=>{
  btn.addEventListener('click', ()=>{
    currentDepositMethod = {method: btn.dataset.method, number: btn.dataset.num};
    depositInfo.classList.remove('hidden');
    depositNumberDisplay.textContent = `${currentDepositMethod.method}: ${currentDepositMethod.number}`;
    depositFormCard.classList.add('hidden');
  });
});

copyDepositNumber.addEventListener('click', ()=>{
  if(!currentDepositMethod) return alert('Select method first');
  navigator.clipboard.writeText(currentDepositMethod.number);
  alert('Number copied!');
});

showDepositForm.addEventListener('click', ()=>{
  if(!currentDepositMethod) return alert('Select deposit method first');
  depositFormCard.classList.remove('hidden');
});

// submit deposit (simulation)
submitDeposit.addEventListener('click', ()=>{
  if(!state.user) return alert('Login first');
  const amt = parseInt(depositAmountInput.value);
  const txn = depositTxnInput.value.trim();
  const file = depositProofInput.files[0];
  if(!amt || !txn || !file) return alert('Provide amount, TXN and proof image.');
  // If user had selected a plan, mark plan startTime if not started
  if(state.plan && !state.plan.startTime){
    state.plan.startTime = new Date().toISOString();
    state.plan.count = 0;
    localStorage.setItem('rock_plan', JSON.stringify(state.plan));
    state.activities.unshift(`Plan started: ${state.plan.name} at ${new Date().toLocaleString()}`);
  }
  state.balance += amt;
  state.activities.unshift(`Deposit: PKR ${amt} via ${currentDepositMethod ? currentDepositMethod.method : '—'} TXN:${txn}`);
  persistState();
  renderAll();
  alert('Deposit submitted (simulation) — balance updated.');
  // reset deposit form
  depositAmountInput.value=''; depositTxnInput.value=''; depositProofInput.value=null;
  depositFormCard.classList.add('hidden');
});

// ---------- WITHDRAW LOGIC ----------
submitWithdraw.addEventListener('click', ()=>{
  if(!state.user) return alert('Login first');
  const amt = parseInt(withdrawAmountInput.value);
  const to = withdrawToInput.value.trim();
  const method = document.querySelector('input[name="wmethod"]:checked').value;
  if(!amt || !to) return alert('Enter amount and recipient.');
  if(amt > state.balance) return alert('Insufficient balance.');
  state.balance -= amt;
  state.activities.unshift(`Withdrawal requested: PKR ${amt} via ${method} to ${to}`);
  persistState();
  renderAll();
  alert('Withdrawal request
