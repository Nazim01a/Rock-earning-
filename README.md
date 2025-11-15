<ROCK>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Rock Earn — Full Final</title>
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet"/>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  :root{--cy:#00f7ff;--bg1:#0f1123;--bg2:#13142b;}
  body{font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;background:linear-gradient(120deg,var(--bg1),var(--bg2));color:#e6f7ff;margin:0;min-height:100vh;}
  .glass{background:rgba(0,0,0,0.45);backdrop-filter:blur(8px);border-radius:14px;border:1px solid rgba(0,255,255,0.06);box-shadow:0 8px 30px rgba(0,0,0,0.6);}
  .btn{background:#07121a;color:var(--cy);padding:.6rem 1.1rem;border-radius:10px;font-weight:700;box-shadow:0 6px 18px rgba(0,255,255,0.04);cursor:pointer;border:1px solid rgba(0,255,255,0.07);}
  .btn:hover{transform:translateY(-3px);box-shadow:0 10px 30px rgba(0,255,255,0.07);}
  #notif{position:fixed;right:18px;top:18px;background:linear-gradient(90deg,var(--cy),#66f0ff);color:#012; padding:10px 14px;border-radius:10px;box-shadow:0 10px 30px rgba(0,0,0,0.6);transform:translateX(350px);transition:0.45s;z-index:9999;font-weight:700;}
  #notif.show{transform:translateX(0);}
  /* layout */
  #mainWrapper{display:flex;gap:20px;padding:18px;}
  #leftSidebar{width:84px;display:flex;flex-direction:column;align-items:center;padding-top:18px;gap:12px;height:calc(100vh - 36px);position:fixed;left:18px;top:18px;background:rgba(0,0,0,0.45);border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.6);}
  .icon-btn{width:60px;height:60px;border-radius:12px;display:flex;align-items:center;justify-content:center;flex-direction:column;color:#bfeff6;background:#07121a;border:1px solid rgba(0,255,255,0.04);cursor:pointer;transition:0.25s;}
  .icon-btn:hover{transform:translateY(-6px);box-shadow:0 6px 28px rgba(0,255,255,0.06);}
  .icon-label{font-size:11px;color:#bfeff6;margin-top:6px;text-align:center;}
  #centerArea{margin-left:130px;margin-right:18px;flex:1;padding-top:18px;}
  #topBar{display:flex;justify-content:space-between;align-items:center;gap:14px;}
  #welcomeCard{padding:18px 20px;border-radius:14px;display:flex;justify-content:space-between;align-items:center;gap:20px;}
  .stat{background:linear-gradient(180deg,rgba(0,255,255,0.03), rgba(0,255,255,0.01));padding:12px;border-radius:10px;min-width:120px;text-align:center;}
  #contentCard{margin-top:18px;padding:18px;border-radius:14px;min-height:60vh;overflow:auto;}
  /* plans */
  .plan-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px;}
  .plan{padding:12px;border-radius:12px;border:1px solid rgba(0,255,255,0.06);transition:0.35s;cursor:pointer;}
  .plan:hover{transform:translateY(-6px);box-shadow:0 16px 60px rgba(0,255,255,0.04);}
  .plan h3{font-weight:800;font-size:18px}
  .plan .price{font-weight:900;font-size:20px;color:var(--cy)}
  /* forms */
  input,select,textarea{background:rgba(0,0,0,0.35);border-radius:10px;padding:10px;border:1px solid rgba(255,255,255,0.04);color:#e6f7ff;width:100%;box-sizing:border-box;}
  label{font-size:13px;color:#bfeff6;font-weight:700;margin-bottom:6px;display:block;}
  .small{font-size:12px;color:#9fdfe6}
  .rightTiny{font-size:12px;color:#9fdfe6;text-align:right}
  /* admin list */
  .req-card{display:flex;gap:12px;align-items:flex-start;padding:12px;border-radius:12px;border:1px solid rgba(255,255,255,0.04);background:linear-gradient(180deg,rgba(255,255,255,0.01),transparent)}
  .img-preview{width:120px;height:80px;border-radius:8px;object-fit:cover;border:1px solid rgba(255,255,255,0.04);}
  /* responsive */
  @media (max-width:900px){
    #leftSidebar{position:static;width:100%;height:auto;display:flex;flex-direction:row;justify-content:space-around;padding:8px;border-radius:10px;margin-bottom:12px;}
    #centerArea{margin-left:18px;margin-right:18px;}
  }
</style>
</head>
<body>

<div id="notif"></div>

<!-- SIDEBAR (icons hidden until login) -->
<div id="leftSidebar" style="display:none">
  <div style="font-size:22px">🚀</div>
  <div class="icon-btn" id="btnPlans" title="Plans"><i class="fa fa-gem fa-lg"></i><div class="icon-label">Plans</div></div>
  <div class="icon-btn" id="btnDeposit" title="Deposit"><i class="fa fa-money-bill-wave fa-lg"></i><div class="icon-label">Deposit</div></div>
  <div class="icon-btn" id="btnWithdraw" title="Withdraw"><i class="fa fa-hand-holding-dollar fa-lg"></i><div class="icon-label">Withdraw</div></div>
  <div class="icon-btn" id="btnProfit" title="Profit"><i class="fa fa-coins fa-lg"></i><div class="icon-label">Profit</div></div>
  <div class="icon-btn" id="btnHistory" title="History"><i class="fa fa-clock fa-lg"></i><div class="icon-label">History</div></div>
  <div class="icon-btn" id="btnReferral" title="Referral"><i class="fa fa-share-alt fa-lg"></i><div class="icon-label">Referral</div></div>
  <div class="icon-btn" id="btnAdmin" title="Admin" style="display:none"><i class="fa fa-user-shield fa-lg"></i><div class="icon-label">Admin</div></div>
  <div style="flex:1"></div>
  <div class="icon-btn" id="btnLogout" title="Logout"><i class="fa fa-right-from-bracket fa-lg"></i><div class="icon-label">Logout</div></div>
</div>

<!-- CENTER -->
<div id="centerArea">

  <!-- topbar / auth -->
  <div id="topBar">
    <div id="authArea" class="glass p-4" style="display:flex;gap:12px;align-items:center;">
      <!-- If not logged in, show signup/login -->
      <div id="authForms">
        <div id="authBox" style="max-width:420px;">
          <div style="display:flex;gap:8px;align-items:center;justify-content:space-between;">
            <h2 style="font-weight:900">Rock Earn</h2>
            <small class="small">Client Demo</small>
          </div>
          <div style="margin-top:10px;display:flex;gap:8px;">
            <input id="authName" placeholder="Full name" style="flex:1" />
            <input id="authEmail" placeholder="Email" style="flex:1" />
            <input id="authPass" type="password" placeholder="Password" style="flex:1" />
          </div>
          <div style="margin-top:10px;display:flex;gap:8px;">
            <button class="btn" id="signupBtn"><i class="fa fa-user-plus mr-2"></i>Sign Up</button>
            <button class="btn" id="loginBtn" style="background:#052b2a;color:#c9fff9"><i class="fa fa-sign-in-alt mr-2"></i>Login</button>
          </div>
          <p class="small" style="margin-top:8px">Use <b>admin@rockearn.com</b> / <i>admin123</i> for admin.</p>
        </div>
      </div>

      <div id="userBox" style="display:none;align-items:center;gap:12px;">
        <div>
          <div id="welcomeTxt" style="font-weight:800"></div>
          <div class="small">Balance: <span id="balValue">0</span> PKR · Profit: <span id="profValue">0</span> PKR</div>
        </div>
      </div>
    </div>

    <div style="display:flex;gap:10px;align-items:center">
      <button class="btn" id="openShare">Share</button>
      <button class="btn" id="openSupport">Support</button>
    </div>
  </div>

  <!-- welcome / stats -->
  <div id="welcomeCard" class="glass mt-4 p-4">
    <div>
      <h2 style="font-size:20px">🎉 Welcome to <span style="color:white;font-weight:900">Rock Earn</span> Premium</h2>
      <p class="small">Stylish dashboard — demo client-only app.</p>
    </div>
    <div style="display:flex;gap:12px;">
      <div class="stat"><div class="small">Balance</div><div id="balBig" style="font-weight:900;font-size:18px">0 PKR</div></div>
      <div class="stat"><div class="small">Total Profit</div><div id="profBig" style="font-weight:900;font-size:18px">0 PKR</div></div>
    </div>
  </div>

  <!-- content area -->
  <div id="contentCard" class="glass mt-4 p-4">
    <!-- default home -->
    <div id="homeView">
      <h3 style="font-weight:900;margin-bottom:8px">Featured Plans</h3>
      <div class="plan-grid" id="planGrid"></div>
    </div>

    <!-- dynamic sections -->
    <div id="depositView" style="display:none">
      <h3 style="font-weight:900">Deposit — JazzCash / EasyPaisa</h3>
      <div style="display:grid;grid-template-columns:1fr 320px;gap:12px;margin-top:10px">
        <div>
          <label>Amount (PKR)</label>
          <input id="depAmount" type="number" placeholder="e.g. 2000" />
          <label>Payment Method</label>
          <select id="depMethod">
            <option>JazzCash</option>
            <option>EasyPaisa</option>
          </select>
          <label>Transaction ID / Reference</label>
          <input id="depTx" placeholder="Txn ID / Reference" />
          <label>Upload Payment Proof (image)</label>
          <input id="depProof" type="file" accept="image/*" />
          <div class="rightTiny" style="margin-top:8px">Admin will approve deposit to add balance</div>
          <div style="margin-top:12px;display:flex;gap:8px">
            <button class="btn" id="sendDeposit">Send Deposit</button>
            <button class="btn" id="cancelDeposit">Cancel</button>
          </div>
        </div>
        <div>
          <div class="small" style="color:#9fdfe6">Example JazzCash Number</div>
          <div class="glass p-3 mt-2" style="border-radius:10px">
            <div style="font-weight:800">0333-1234567</div>
            <div class="small">Rock Earn (Demo)</div>
            <div class="small" style="margin-top:8px">Send amount and upload screenshot/tx id above.</div>
          </div>
        </div>
      </div>
    </div>

    <div id="withdrawView" style="display:none">
      <h3 style="font-weight:900">Withdraw</h3>
      <div style="max-width:520px;margin-top:10px">
        <label>Full Name</label><input id="wName" placeholder="Full name" />
        <label>Account Number / Mobile</label><input id="wAcc" placeholder="JazzCash / EasyPaisa number or bank ac" />
        <label>Amount (PKR)</label><input id="wAmt" type="number" />
        <label>Optional proof screenshot (for manual processing)</label><input id="wProof" type="file" accept="image/*" />
        <div style="display:flex;gap:8px;margin-top:10px">
          <button class="btn" id="sendWithdraw">Request Withdraw</button>
          <button class="btn" id="cancelWithdraw">Cancel</button>
        </div>
        <div class="small" style="margin-top:8px;color:#9fdfe6">Withdraw requests are processed by admin demo only.</div>
      </div>
    </div>

    <div id="historyView" style="display:none">
      <h3 style="font-weight:900">History</h3>
      <div id="historyList" style="margin-top:12px;display:flex;flex-direction:column;gap:8px"></div>
    </div>

    <div id="profitView" style="display:none">
      <h3 style="font-weight:900">Daily Profit</h3>
      <div style="margin-top:8px">
        <div class="small">Total accumulated profit:</div>
        <div style="font-weight:900;font-size:18px" id="profitTotal">0 PKR</div>
      </div>
    </div>

    <div id="referralView" style="display:none">
      <h3 style="font-weight:900">Referral</h3>
      <div style="margin-top:8px">
        <div class="small">Your invite code (use email):</div>
        <div class="glass p-3 mt-2" id="inviteBox" style="display:inline-block"></div>
      </div>
    </div>

    <div id="adminView" style="display:none">
      <h3 style="font-weight:900">Admin Panel</h3>
      <div style="margin-top:8px" id="adminRequests"></div>
    </div>

    <div id="supportView" style="display:none">
      <h3 style="font-weight:900">Support</h3>
      <div class="small" style="margin-top:8px">Contact demo support: support@rockearn.com</div>
    </div>
  </div>
</div>

<script>
/* --------------------------
   Utility & Notification
   -------------------------- */
function notify(msg){
  const n = document.getElementById('notif');
  n.innerText = msg;
  n.classList.add('show');
  setTimeout(()=> n.classList.remove('show'), 3000);
}

/* --------------------------
   LocalStorage keys & defaults
   -------------------------- */
let USERS_KEY = 'reUsers_demo';
let CURRENT_KEY = 'reCurrent_demo';
let REQ_KEY = 'reRequests_demo';

// init users array and admin
let users = JSON.parse(localStorage.getItem(USERS_KEY)) || [];
if(!users.find(u=>u.email==='admin@rockearn.com')){
  users.push({name:'Admin', email:'admin@rockearn.com', pass:'admin123', balance:0, profit:0, plans:[], history:[]});
  localStorage.setItem(USERS_KEY, JSON.stringify(users));
}
let currentUser = JSON.parse(localStorage.getItem(CURRENT_KEY)) || null;
let requests = JSON.parse(localStorage.getItem(REQ_KEY)) || [];

/* --------------------------
   Elements
   -------------------------- */
const leftSidebar = document.getElementById('leftSidebar');
const authBox = document.getElementById('authBox');
const userBox = document.getElementById('userBox');
const welcomeTxt = document.getElementById('welcomeTxt');
const balValue = document.getElementById('balValue');
const profValue = document.getElementById('profValue');
const balBig = document.getElementById('balBig');
const profBig = document.getElementById('profBig');
const inviteBox = document.getElementById('inviteBox');

/* --------------------------
   Plans data
   -------------------------- */
let plans = [
  {name:'Basic', amount:200, daily:30, days:1, badge:'Starter'},
  {name:'Starter', amount:500, daily:75, days:1, badge:'Hot'},
  {name:'Pro', amount:1000, daily:180, days:1, badge:'Best'},
  {name:'Advanced', amount:1500, daily:250, days:2, badge:'Pro'},
  {name:'Silver', amount:2000, daily:350, days:3, badge:'Stable'},
  {name:'Gold', amount:3000, daily:550, days:3, badge:'Value'},
  {name:'Platinum', amount:5000, daily:950, days:4, badge:'Premium'},
  {name:'Diamond', amount:7000, daily:1350, days:5, badge:'VIP'}
];

/* --------------------------
   Render featured plans
   -------------------------- */
function renderPlans(){
  const grid = document.getElementById('planGrid');
  grid.innerHTML = '';
  plans.forEach(p=>{
    const div = document.createElement('div');
    div.className = 'plan glass';
    div.innerHTML = `
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <h3>${p.name}</h3>
          <div class="small">${p.badge}</div>
        </div>
        <div class="price">${p.amount} PKR</div>
      </div>
      <div style="margin-top:8px">
        <div class="small">Daily: <b>${p.daily} PKR</b> · Days: <b>${p.days}</b></div>
      </div>
      <div style="margin-top:12px;display:flex;gap:8px">
        <button class="btn" onclick="buyPlan('${p.name}')">Buy</button>
        <button class="btn" onclick="viewPlan('${p.name}')">Details</button>
      </div>
    `;
    grid.appendChild(div);
  });
}

/* --------------------------
   Auth: signup / login / logout
   -------------------------- */
document.getElementById('signupBtn').onclick = () => {
  let n = document.getElementById('authName').value.trim();
  let e = document.getElementById('authEmail').value.trim().toLowerCase();
  let p = document.getElementById('authPass').value.trim();
  if(!n || !e || !p) return notify('Fill all fields');
  if(!/^\S+@\S+\.\S+$/.test(e)) return notify('Invalid email');
  users = JSON.parse(localStorage.getItem(USERS_KEY)) || users;
  if(users.find(u=>u.email===e)) return notify('Email already registered');
  let u = {name:n, email:e, pass:p, balance:0, profit:0, plans:[], history:[]};
  users.push(u);
  localStorage.setItem(USERS_KEY, JSON.stringify(users));
  currentUser = u;
  localStorage.setItem(CURRENT_KEY, JSON.stringify(currentUser));
  afterLogin();
  notify('Signup successful');
};

document.getElementById('loginBtn').onclick = () => {
  let e = document.getElementById('authEmail').value.trim().toLowerCase();
  let p = document.getElementById('authPass').value.trim();
  users = JSON.parse(localStorage.getItem(USERS_KEY)) || users;
  let u = users.find(x=>x.email===e && x.pass===p);
  if(!u) return notify('Invalid credentials');
  currentUser = u;
  localStorage.setItem(CURRENT_KEY, JSON.stringify(currentUser));
  afterLogin();
  notify('Welcome ' + currentUser.name.split(' ')[0]);
};

document.getElementById('btnLogout').onclick = () => {
  currentUser = null;
  localStorage.removeItem(CURRENT_KEY);
  leftSidebar.style.display = 'none';
  authBox.style.display = 'block';
  userBox.style.display = 'none';
  document.getElementById('homeView').style.display = 'block';
  hideAllViews();
  renderPlans();
  notify('Logged out');
};

/* Keep in sync across tabs */
window.addEventListener('storage', (e)=>{
  if(e.key === USERS_KEY) users = JSON.parse(localStorage.getItem(USERS_KEY)) || users;
  if(e.key === CURRENT_KEY){
    currentUser = JSON.parse(localStorage.getItem(CURRENT_KEY));
    if(currentUser) afterLogin();
    else { leftSidebar.style.display='none'; authBox.style.display='block'; userBox.style.display='none'; }
  }
  if(e.key === REQ_KEY) requests = JSON.parse(localStorage.getItem(REQ_KEY)) || [];
});

/* After successful login */
function afterLogin(){
  // hide auth / show user
  authBox.style.display = 'none';
  userBox.style.display = 'flex';
  leftSidebar.style.display = 'flex';
  document.getElementById('homeView').style.display = 'block';
  hideAllViews();
  // update UI values
  updateUserUI();
  // show admin button if admin
  if(currentUser.email === 'admin@rockearn.com'){
    document.getElementById('btnAdmin').style.display = 'block';
  } else {
    document.getElementById('btnAdmin').style.display = 'none';
  }
  renderPlans();
  renderHistory();
  inviteBox.innerText = currentUser.email;
}

/* update user balances in UI (reads currentUser from memory or localStorage) */
function updateUserUI(){
  currentUser = JSON.parse(localStorage.getItem(CURRENT_KEY)) || currentUser;
  if(!currentUser) return;
  welcomeTxt.innerText = currentUser.name;
  balValue.innerText = (currentUser.balance || 0);
  profValue.innerText = (currentUser.profit || 0);
  balBig.innerText = (currentUser.balance || 0) + ' PKR';
  profBig.innerText = (currentUser.profit || 0) + ' PKR';
}

/* on load if already logged in */
if(currentUser){
  afterLogin();
} else {
  // show auth area
  authBox.style.display = 'block';
  leftSidebar.style.display = 'none';
}

/* --------------------------
   Navigation buttons
   -------------------------- */
document.getElementById('btnPlans').onclick = ()=>{ showView('homeView'); renderPlans(); };
document.getElementById('btnDeposit').onclick = ()=>{ showView('depositView'); };
document.getElementById('btnWithdraw').onclick = ()=>{ showView('withdrawView'); };
document.getElementById('btnProfit').onclick = ()=>{ showView('profitView'); updateProfitView(); };
document.getElementById('btnHistory').onclick = ()=>{ showView('historyView'); renderHistory(); };
document.getElementById('btnReferral').onclick = ()=>{ showView('referralView'); };
document.getElementById('btnAdmin').onclick = ()=>{ showView('adminView'); renderAdmin(); };
document.getElementById('openSupport').onclick = ()=>{ showView('supportView'); };
document.getElementById('openShare').onclick = ()=>{ shareNow(); };

/* show/hide helpers */
function hideAllViews(){
  const ids = ['homeView','depositView','withdrawView','historyView','profitView','referralView','adminView','supportView'];
  ids.forEach(id => document.getElementById(id).style.display = 'none');
}
function showView(id){
  hideAllViews();
  document.getElementById(id).style.display = 'block';
}

/* --------------------------
   Buy Plan
   -------------------------- */
function buyPlan(name){
  if(!currentUser) return notify('Login first');
  const p = plans.find(x=>x.name===name);
  if(!p) return;
  if(currentUser.balance < p.amount) return notify('Insufficient balance');
  // deduct, add profit, add plan & history
  currentUser.balance = Number(currentUser.balance) - Number(p.amount);
  currentUser.profit = Number(currentUser.profit) + Number(p.daily);
  currentUser.plans = currentUser.plans || [];
  let expiry = new Date(); expiry.setDate(expiry.g
