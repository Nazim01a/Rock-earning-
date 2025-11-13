<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earning — Full Demo</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  body { font-family: 'Inter', sans-serif; }
  .card { background: white; border-radius: 12px; box-shadow: 0 6px 18px rgba(0,0,0,0.06); }
</style>
</head>
<body class="bg-gray-100 p-6">

<!-- AUTH -->
<div id="auth" class="max-w-md mx-auto"></div>

<!-- DASHBOARD -->
<div id="dashboard" class="max-w-6xl mx-auto hidden">

<header class="flex items-center justify-between mb-6">
  <h1 class="text-2xl font-semibold">Rock Earning — Dashboard</h1>
  <div class="flex items-center gap-3">
    <span id="welcomeUser" class="text-gray-700"></span>
    <button id="copyReferral" class="px-3 py-2 bg-blue-600 text-white rounded">Copy Referral Link</button>
    <button id="logout" class="px-3 py-2 bg-red-500 text-white rounded">Logout</button>
  </div>
</header>

<main class="grid grid-cols-1 lg:grid-cols-3 gap-6">

<!-- Left: Balance + Actions -->
<section class="card p-4 lg:col-span-1">
  <div class="mb-4">
    <div class="text-sm text-gray-500">Available Balance</div>
    <div id="balance" class="text-3xl font-bold">₨ 0</div>
  </div>
  <div class="space-y-3">
    <button id="openDeposit" class="w-full px-4 py-2 bg-green-600 text-white rounded">Deposit</button>
    <button id="openWithdraw" class="w-full px-4 py-2 bg-yellow-500 text-black rounded">Withdraw</button>
  </div>
</section>

<!-- Middle: Plans + Deposit -->
<section class="card p-4 lg:col-span-1">
  <h2 class="font-semibold mb-3">Investment Plans</h2>
  <div id="plans" class="grid grid-cols-1 gap-3"></div>

  <!-- Deposit Form -->
  <div id="depositSection" class="mt-4 hidden">
    <h3 class="font-medium mb-2">Deposit via</h3>
    <div class="flex gap-2 mb-3">
      <button class="depositMethodBtn px-3 py-2 border rounded" data-method="easypaisa">Easypaisa: 03379827882</button>
      <button class="depositMethodBtn px-3 py-2 border rounded" data-method="jazzcash">JazzCash: 03705519562</button>
    </div>
    <form id="depositForm" class="space-y-3">
      <input type="number" id="depositAmount" placeholder="Amount (₨)" required class="w-full px-3 py-2 border rounded"/>
      <input type="text" id="depositTXID" placeholder="Transaction ID" required class="w-full px-3 py-2 border rounded"/>
      <input type="file" id="depositProof" accept="image/*" class="w-full"/>
      <div id="previewWrap" class="mt-2 hidden"><img id="preview" class="max-w-full rounded border"/></div>
      <button type="submit" class="px-4 py-2 bg-blue-600 text-white rounded">Submit Deposit</button>
      <button type="button" id="clearDeposit" class="px-4 py-2 bg-gray-200 rounded">Clear</button>
      <input type="hidden" id="selectedMethod" value="easypaisa"/>
    </form>
  </div>
</section>

<!-- Right: Deposit History -->
<section class="card p-4 lg:col-span-1">
  <div class="flex items-center justify-between mb-3">
    <h2 class="font-semibold">Deposits</h2>
    <button id="refreshList" class="text-sm text-blue-600">Refresh</button>
  </div>
  <div id="depositsList" class="space-y-3 max-h-96 overflow-auto"></div>
</section>

</main>
<footer class="mt-6 text-sm text-gray-600">
  Rock Earning Demo — Client-side LocalStorage version
</footer>
</div>

<script>
// ----- AUTH -----
const authDiv=document.getElementById('auth');
const dashboardDiv=document.getElementById('dashboard');
let users=JSON.parse(localStorage.getItem('rockUsers')||'[]');
let currentUser=JSON.parse(localStorage.getItem('rockCurrentUser')||'null');

function showAuth(){
  authDiv.innerHTML=`
  <div class="card p-6">
    <h2 class="text-xl font-semibold mb-4">Login</h2>
    <form id="loginForm" class="space-y-3">
      <input id="loginEmail" type="email" placeholder="Gmail" required class="w-full px-3 py-2 border rounded">
      <input id="loginPassword" type="password" placeholder="Password" required class="w-full px-3 py-2 border rounded">
      <button type="submit" class="w-full bg-blue-600 text-white px-3 py-2 rounded">Login</button>
    </form>
    <div class="mt-4 text-center">
      <span>Don't have account?</span>
      <button id="showSignup" class="text-blue-600 underline">Signup</button>
    </div>
  </div>`;
  document.getElementById('showSignup').onclick=showSignup;
  document.getElementById('loginForm').onsubmit=function(e){
    e.preventDefault();
    const email=document.getElementById('loginEmail').value.trim();
    const pass=document.getElementById('loginPassword').value.trim();
    const user=users.find(u=>u.email===email && u.password===pass);
    if(user){ currentUser=user; localStorage.setItem('rockCurrentUser',JSON.stringify(user)); showDashboard(); }
    else alert('Invalid email/password');
  }
}
function showSignup(){
  authDiv.innerHTML=`
  <div class="card p-6">
    <h2 class="text-xl font-semibold mb-4">Signup</h2>
    <form id="signupForm" class="space-y-3">
      <input id="signupName" type="text" placeholder="Username" required class="w-full px-3 py-2 border rounded">
      <input id="signupEmail" type="email" placeholder="Gmail" required class="w-full px-3 py-2 border rounded">
      <input id="signupPassword" type="password" placeholder="Password" required class="w-full px-3 py-2 border rounded">
      <button type="submit" class="w-full bg-green-600 text-white px-3 py-2 rounded">Signup</button>
    </form>
    <div class="mt-4 text-center">
      <span>Already have account?</span>
      <button id="showLogin" class="text-blue-600 underline">Login</button>
    </div>
  </div>`;
  document.getElementById('showLogin').onclick=showAuth;
  document.getElementById('signupForm').onsubmit=function(e){
    e.preventDefault();
    const name=document.getElementById('signupName').value.trim();
    const email=document.getElementById('signupEmail').value.trim();
    const pass=document.getElementById('signupPassword').value.trim();
    if(users.find(u=>u.email===email)){ alert('Email already used'); return; }
    const newUser={name,email,password:pass,balance:0,deposits:[],plan:null};
    users.push(newUser); localStorage.setItem('rockUsers',JSON.stringify(users));
    alert('Signup successful'); showAuth();
  }
}

// ----- DASHBOARD -----
const plansEl=document.getElementById('plans');
const depositSection=document.getElementById('depositSection');
const depositForm=document.getElementById('depositForm');
const depositAmount=document.getElementById('depositAmount');
const depositTXID=document.getElementById('depositTXID');
const depositProof=document.getElementById('depositProof');
const previewWrap=document.getElementById('previewWrap');
const previewImg=document.getElementById('preview');
const selectedMethod=document.getElementById('selectedMethod');
const depositMethodBtns=document.querySelectorAll('.depositMethodBtn');
const depositsList=document.getElementById('depositsList');
const balanceEl=document.getElementById('balance');

const plans=[
  {name:'Plan 1', amount:180, daily:3},
  {name:'Plan 2', amount:500, daily:3.5},
  {name:'Plan 3', amount:1000, daily:4},
  {name:'Plan 4', amount:1500, daily:4.5},
  {name:'Plan 5', amount:2500, daily:5},
  {name:'Plan 6', amount:5000, daily:5.5},
  {name:'Plan 7', amount:7000, daily:6}
];

function showDashboard(){
  authDiv.classList.add('hidden'); dashboardDiv.classList.remove('hidden');
  document.getElementById('welcomeUser').textContent=`Welcome, ${currentUser.name}`;
  renderPlans();
  loadUserData();
}

function renderPlans(){
  plansEl.innerHTML='';
  plans.forEach((p,i)=>{
    const div=document.createElement('div');
    div.className='p-3 border rounded flex justify-between items-center';
    div.innerHTML=`<div><b>${p.name}</b><br>₨${p.amount} — Daily ${p.daily}%</div>
      <button class="px-3 py-1 bg-green-600 text-white rounded buyBtn">Buy Now</button>`;
    div.querySelector('.buyBtn').addEventListener('click', ()=>{
      depositSection.classList.remove('hidden');
      depositAmount.value=p.amount;
    });
    plansEl.appendChild(div);
  });
}

depositMethodBtns.forEach(btn=>btn.addEventListener('click', ()=>{
  selectedMethod.value=btn.dataset.method;
}));

depositProof.addEventListener('change', (e)=>{
  const f=e.target.files[0]; if(!f){ previewWrap.classList.add('hidden'); return; }
  previewImg.src=URL.createObjectURL(f); previewWrap.classList.remove('hidden');
});

depositForm.onsubmit=function(e){
  e.preventDefault();
  const amt=Number(depositAmount.value);
  const txid=depositTXID.value.trim();
  const method=selectedMethod.value;
  if(!amt||!txid){ alert('Fill amount and TXID'); return; }
  const file=depositProof.files[0];
  const dep={amount:amt,txid,method,proof:null,status:'Pending',date:new Date().toISOString()};
  if(file){ const reader=new FileReader();
    reader.onload=function(ev){ dep.proof=ev.target.result; saveDeposit(dep); }
    reader.readAsDataURL(file);
  } else saveDeposit(dep);
};

function saveDeposit(dep){
  currentUser.deposits.unshift(dep);
  currentUser.balance+=Number(dep.amount);
  updateUserStorage(); loadUserData();
  depositForm.reset(); previewWrap.classList.add('hidden'); alert('Deposit submitted (demo).');
}

function loadUserData(){
  balanceEl.textContent=`₨ ${currentUser.balance.toLocaleString()}`;
  depositsList.innerHTML='';
  currentUser.deposits.forEach((d,i)=>{
    const div=document.createElement('div');
    div.className='p-3 border rounded flex items-start gap-3';
    div.innerHTML=`<div class="flex-1">
      <div class="flex justify-between items-start">
        <div>
          <div class="text-sm text-gray-500">${new Date(d.date).toLocaleString()}</div>
          <div class="font-medium">${d.method.toUpperCase()} — ₨ ${d.amount}</div>
          <div class="text-xs text-gray-600">TXID: <span class="font-mono">${d.txid}</span></div>
        </div>
        <div class="text-sm">${d.status}</div>
      </div>
      ${d.proof?`<div class="mt-2"><img src="${d.proof}" class="max-w-xs rounded border"/></div>`:''}
    </div>`;
    depositsList.appendChild(div);
  });
}

function updateUserStorage(){
  const idx=users.findIndex(u=>u.email===currentUser.email);
  if(idx>-1){ users[idx]=currentUser; localStorage.setItem('rockUsers',JSON.stringify(users)); }
  localStorage.setItem('rockCurrentUser',JSON.stringify(currentUser));
}

document.getElementById('copyReferral').addEventListener('click', async()=>{
  const link=`https://rockearning.com?ref=${currentUser.name}`;
  try{ await navigator.clipboard.writeText(link); alert('Referral copied!'); }
  catch(e){ alert('Copy failed: '+link); }
});

document.getElementById('openWithdraw').addEventListener('click', ()=>{
  const amt=prompt('Enter withdraw amount:'); if(!amt) return;
  if(Number(amt)>currentUser.balance){ alert('Insufficient balance'); return; }
  currentUser.balance-=Number(amt); updateUserStorage(); loadUserData(); alert('Withdrawal requested (demo).');
});

document.getElementById('logout').addEventListener('click', ()=>{
  if(confirm('Logout?')){ localStorage.removeItem('rockCurrentUser'); currentUser=null; dashboardDiv.classList.add('hidden'); showAuth(); }
});

document.getElementById('clearDeposit').addEventListener('click', ()=>{ depositForm.reset(); previewWrap.classList.add('hidden'); });

// INITIAL
if(currentUser) showDashboard(); else showAuth();
</script>

</body>
</html>
