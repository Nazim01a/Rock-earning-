<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn — Pro Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;}
.button-animate:hover{transform:scale(1.05);transition:0.3s;}
.card-hover:hover{transform:translateY(-6px);box-shadow:0 12px 25px rgba(0,0,0,0.25);transition:0.3s;}
.hidden{display:none;}
.bg-gradient-primary{background:linear-gradient(90deg,#4f46e5,#3b82f6);}
.bg-gradient-secondary{background:linear-gradient(90deg,#f97316,#facc15);}
.bg-gradient-success{background:linear-gradient(90deg,#10b981,#3b82f6);}
</style>
</head>
<body class="bg-gray-50 min-h-screen flex flex-col items-center p-4">

<header class="w-full max-w-6xl mb-6 p-6 rounded-xl bg-gradient-to-r from-indigo-600 via-pink-500 to-purple-600 text-white shadow-2xl text-center">
  <h1 class="text-4xl font-extrabold">Rock Earn</h1>
  <p class="mt-2 text-base opacity-90">Your interactive earning dashboard</p>
</header>

<main class="w-full max-w-6xl bg-white rounded-xl shadow-lg p-6 flex flex-col gap-6">

<!-- Landing -->
<div id="landingStep" class="flex flex-col items-center gap-6">
  <h2 class="text-2xl font-bold">Welcome! Login or Sign Up to Continue</h2>
  <div class="flex gap-4">
    <button id="loginBtn" class="px-6 py-3 rounded-xl bg-gradient-primary text-white font-semibold button-animate">Login</button>
    <button id="signupBtn" class="px-6 py-3 rounded-xl bg-gradient-secondary text-white font-semibold button-animate">Sign Up</button>
  </div>
</div>

<!-- Login / SignUp Form -->
<div id="formStep" class="hidden flex flex-col gap-4">
  <h2 class="text-2xl font-bold" id="formTitle">Login</h2>
  <form id="authForm" class="flex flex-col gap-4">
    <input name="name" id="nameInput" placeholder="Full Name" class="p-3 rounded-lg border border-gray-200" required>
    <input type="email" name="email" id="emailInput" placeholder="Email" class="p-3 rounded-lg border border-gray-200" required>
    <input type="password" name="password" id="passwordInput" placeholder="Password" class="p-3 rounded-lg border border-gray-200" required>
    <div class="flex items-center gap-3">
      <input type="checkbox" id="termsCheck" required>
      <label for="termsCheck" class="text-sm">I agree to the <a href="#" class="underline text-indigo-600">Terms & Conditions</a></label>
    </div>
    <button type="submit" class="py-3 rounded-xl bg-gradient-primary text-white font-semibold button-animate">Submit</button>
  </form>
  <button id="backLanding" class="mt-2 text-sm text-gray-500 underline">Back</button>
</div>

<!-- Dashboard -->
<div id="dashboardStep" class="hidden flex flex-col gap-6">

  <!-- Welcome & Balance -->
  <div class="flex flex-col md:flex-row justify-between items-center gap-4">
    <div>
      <h2 class="text-2xl font-bold">Welcome, <span id="userName">User</span></h2>
      <p class="text-gray-500">Your personalized dashboard</p>
    </div>
    <div class="p-4 rounded-xl shadow-xl bg-gradient-success text-white">
      <h3 class="font-semibold">Account Balance</h3>
      <div class="text-2xl font-bold">PKR <span id="accountBalance">0</span></div>
    </div>
  </div>

  <!-- Company Info -->
  <div class="p-4 rounded-xl bg-gray-100 border shadow-sm text-gray-700">
    <h3 class="font-semibold mb-1">Company Info</h3>
    <p>Rock Earn LLC, launched 2018, based in California, United States. Professional simulated earning platform with interactive plans, deposits, and withdrawals. Trusted and user-friendly experience.</p>
  </div>

  <!-- Plans -->
  <div>
    <h3 class="font-semibold mb-2">Available Plans</h3>
    <div class="grid md:grid-cols-3 gap-4">
      <div class="p-4 rounded-xl bg-white shadow-md card-hover flex flex-col gap-2">
        <div class="text-gray-500 text-sm">Micro</div>
        <div class="font-bold text-xl">PKR 180</div>
        <div class="text-xs text-gray-400">Daily Profit PKR 35 • Duration 30 days</div>
        <button class="mt-2 px-3 py-2 rounded-xl bg-gradient-primary text-white font-semibold buyPlan" data-profit="35" data-duration="30">Buy Plan</button>
      </div>
      <div class="p-4 rounded-xl bg-white shadow-md card-hover flex flex-col gap-2">
        <div class="text-gray-500 text-sm">Starter</div>
        <div class="font-bold text-xl">PKR 500</div>
        <div class="text-xs text-gray-400">Daily Profit PKR 95 • Duration 35 days</div>
        <button class="mt-2 px-3 py-2 rounded-xl bg-gradient-primary text-white font-semibold buyPlan" data-profit="95" data-duration="35">Buy Plan</button>
      </div>
      <div class="p-4 rounded-xl bg-white shadow-md card-hover flex flex-col gap-2">
        <div class="text-gray-500 text-sm">Bronze</div>
        <div class="font-bold text-xl">PKR 1000</div>
        <div class="text-xs text-gray-400">Daily Profit PKR 200 • Duration 35 days</div>
        <button class="mt-2 px-3 py-2 rounded-xl bg-gradient-primary text-white font-semibold buyPlan" data-profit="200" data-duration="35">Buy Plan</button>
      </div>
      <div class="p-4 rounded-xl bg-white shadow-md card-hover flex flex-col gap-2">
        <div class="text-gray-500 text-sm">Silver</div>
        <div class="font-bold text-xl">PKR 2500</div>
        <div class="text-xs text-gray-400">Daily Profit PKR 500 • Duration 35 days</div>
        <button class="mt-2 px-3 py-2 rounded-xl bg-gradient-primary text-white font-semibold buyPlan" data-profit="500" data-duration="35">Buy Plan</button>
      </div>
      <div class="p-4 rounded-xl bg-white shadow-md card-hover flex flex-col gap-2">
        <div class="text-gray-500 text-sm">Gold</div>
        <div class="font-bold text-xl">PKR 5000</div>
        <div class="text-xs text-gray-400">Daily Profit PKR 1200 • Duration 35 days</div>
        <button class="mt-2 px-3 py-2 rounded-xl bg-gradient-primary text-white font-semibold buyPlan" data-profit="1200" data-duration="35">Buy Plan</button>
      </div>
      <div class="p-4 rounded-xl bg-white shadow-md card-hover flex flex-col gap-2">
        <div class="text-gray-500 text-sm">Platinum</div>
        <div class="font-bold text-xl">PKR 7000</div>
        <div class="text-xs text-gray-400">Daily Profit PKR 1750 • Duration 35 days</div>
        <button class="mt-2 px-3 py-2 rounded-xl bg-gradient-primary text-white font-semibold buyPlan" data-profit="1750" data-duration="35">Buy Plan</button>
      </div>
    </div>
  </div>

  <!-- Deposit Section -->
  <div id="depositSection" class="p-4 rounded-xl bg-gray-50 border shadow-sm hidden flex flex-col gap-4">
    <h3 class="font-semibold mb-2">Make a Deposit (Simulation)</h3>

    <!-- Easypaisa -->
    <div class="p-3 border rounded-xl flex flex-col gap-2">
      <div class="font-semibold text-green-600">Easypaisa</div>
      <div>Number: <span id="easypaisaNumber">03379827882</span> <button onclick="copyNumber('easypaisaNumber')" class="px-2 py-1 bg-gray-200 rounded">Copy</button></div>
      <label>Amount (PKR)</label>
      <input id="easypaisaAmount" class="p-2 rounded-lg border border-gray-200">
      <label>Transaction ID</label>
      <input id="easypaisaTxn" class="p-2 rounded-lg border border-gray-200">
      <label>Upload Proof</label>
      <input type="file" id="easypaisaProof" accept="image/*">
      <button onclick="submitDeposit('Easypaisa')" class="py-2 rounded-xl bg-green-500 text-white button-animate">Submit Easypaisa Deposit</button>
    </div>

    <!-- JazzCash -->
    <div class="p-3 border rounded-xl flex flex-col gap-2">
      <div class="font-semibold text-blue-600">JazzCash</div>
      <div>Number: <span id="jazzcashNumber">03705519562</span> <button onclick="copyNumber('jazzcashNumber')" class="px-2 py-1 bg-gray-200 rounded">Copy</button></div>
      <label>Amount (PKR)</label>
      <input id="jazzcashAmount" class="p-2 rounded-lg border border-gray-200">
      <label>Transaction ID</label>
      <input id="jazzcashTxn" class="p-2 rounded-lg border border-gray-200">
      <label>Upload Proof</label>
      <input type="file" id="jazzcashProof" accept="image/*">
      <button onclick="submitDeposit('JazzCash')" class="py-2 rounded-xl bg-blue-500 text-white button-animate">Submit JazzCash Deposit</button>
    </div>
  </div>

  <!-- Withdraw Section -->
  <div id="withdrawSection" class="p-4 rounded-xl bg-gray-50 border shadow-sm hidden flex flex-col gap-2">
    <h3 class="font-semibold mb-2">Request Withdrawal (Simulation)</h3>
    <label>Amount (PKR)</label>
    <input id="withdrawAmount" class="p-2 rounded-lg border border-gray-200">
    <label>Method</label>
    <select id="withdrawMethod" class="p-2 rounded-lg border border-gray-200">
      <option value="easypaisa">Easypaisa</option>
      <option value="jazzcash">JazzCash</option>
    </select>
    <label>Note</label>
    <input id="withdrawNote" class="p-2 rounded-lg border border-gray-200">
    <button id="requestWithdraw" class="py-2 rounded-xl bg-amber-500 text-white button-animate">Submit Withdrawal</button>
  </div>

  <!-- Plan Status & Activity -->
  <div class="flex flex-col gap-4 mt-4">
    <div id="planStatus" class="p-4 rounded-xl bg-gray-50 border shadow-sm hidden">
      <h3 class="font-semibold mb-2">Your Plan Status</h3>
      <div id="planInfo" class="text-gray-700"></div>
    </div>

    <div id="activityLog" class="p-4 rounded-xl bg-gray-50 border shadow-sm">
      <h3 class="font-semibold mb-2">Activity / Requests Log</h3>
      <div id="activityList" class="space-y-2 text-sm text-gray-700"></div>
    </div>
  </div>

</div>
</main>

<script>
// State
const state={user:null,activities:[],balance:0,plan:null};

// Elements
const landingStep=document.getElementById('landingStep');
const formStep=document.getElementById('formStep');
const dashboardStep=document.getElementById('dashboardStep');
const authForm=document.getElementById('authForm');
const formTitle=document.getElementById('formTitle');
const userNameSpan=document.getElementById('userName');
const backLanding=document.getElementById('backLanding');
const loginBtn=document.getElementById('loginBtn');
const signupBtn=document.getElementById('signupBtn');
const accountBalance=document.getElementById('accountBalance');
const planInfo=document.getElementById('planInfo');
const planStatus=document.getElementById('planStatus');

// Load localStorage
window.onload=()=>{
  const savedUser=JSON.parse(localStorage.getItem('rockUser'));
  const savedBalance=parseInt(localStorage.getItem('rockBalance'));
  const savedPlan=JSON.parse(localStorage.getItem('rockPlan'));
  const savedActivities=JSON.parse(localStorage.getItem('rockActivities'));
  if(savedUser){state.user=savedUser;userNameSpan.textContent=savedUser.name;dashboardStep.classList.remove('hidden');landingStep.classList.add('hidden');formStep.classList.add('hidden');}
  if(savedBalance) state.balance=savedBalance;
  if(savedPlan) state.plan=savedPlan;
  if(savedActivities) state.activities=savedActivities;
  updateBalance();
  updatePlanInfo();
  renderActivities();
};

// Landing Buttons
loginBtn.onclick=()=>{showForm('Login');};
signupBtn.onclick=()=>{showForm('Sign Up');};
backLanding.onclick=()=>{formStep.classList.add('hidden');landingStep.classList.remove('hidden');};
function showForm(type){landingStep.classList.add('hidden');formStep.classList.remove('hidden');formTitle.textContent=type;}

// Auth Form Submit
authForm.addEventListener('submit',e=>{
  e.preventDefault();
  const data=Object.fromEntries(new FormData(authForm).entries());
  state.user={name:data.name,email:data.email};
  localStorage.setItem('rockUser',JSON.stringify(state.user));
  userNameSpan.textContent=state.user.name;
  formStep.classList.add('hidden');dashboardStep.classList.remove('hidden');
  alert(`${formTitle.textContent} successful! Select a plan to start.`);
});

// Buy Plan
document.querySelectorAll('.buyPlan').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.getElementById('depositSection').classList.remove('hidden');
    document.getElementById('withdrawSection').classList.remove('hidden');
    const profit=parseInt(btn.dataset.profit);
    const duration=parseInt(btn.dataset.duration);
    const startTime=new Date();
    state.plan={profit,duration,startTime,count:0};
    localStorage.setItem('rockPlan',JSON.stringify(state.plan));
    planStatus.classList.remove('hidden');
    updatePlanInfo();
    alert('Plan selected! Deposit to start earning daily profit.');
  });
});

// Deposit Functions
function copyNumber(id){const number=document.getElementById(id).textContent;navigator.clipboard.writeText(number).then(()=>alert('Number copied: '+number));}
function submitDeposit(method){
  let amt, txn, file;
  if(method==='Easypaisa'){
    amt=parseInt(document.getElementById('easypaisaAmount').value);
    txn=document.getElementById('easypaisaTxn').value;
    file=document.getElementById('easypaisaProof').files[0];
  } else {
    amt=parseInt(document.getElementById('jazzcashAmount').value);
    txn=document.getElementById('jazzcashTxn').value;
    file=document.getElementById('jazzcashProof').files[0];
  }
  if(!amt || !txn || !file){alert('Provide all deposit details.'); return;}
  state.balance+=amt; localStorage.setItem('rockBalance', state.balance);
  updateBalance();
  const log=`${method} Deposit submitted PKR ${amt} — TXN: ${txn} (Simulation)`;
  state.activities.unshift(log);
  localStorage.setItem('rockActivities', JSON.stringify(state.activities));
  renderActivities();
  alert(method+' deposit submitted (simulation).');
}

// Withdraw Simulation
document.getElementById('requestWithdraw').addEventListener('click',()=>{
  const amt=parseInt(document.getElementById('withdrawAmount').value);
  const method=document.getElementById('withdrawMethod').value;
  const note=document.getElementById('withdrawNote').value;
  if(!amt){alert('Enter withdrawal amount.'); return;}
  if(amt>state.balance){alert('Insufficient balance.'); return;}
  state.balance-=amt; localStorage.setItem('rockBalance', state.balance);
  updateBalance();
  const log=`Withdrawal requested PKR ${amt} via ${method} — Note: ${note||'—'} (Simulation)`;
  state.activities.unshift(log);
  localStorage.setItem('rockActivities', JSON.stringify(state.activities));
  renderActivities();
  alert('Withdrawal request submitted (simulation).');
});

// Activities & Balance
function renderActivities(){const container=document.getElementById('activityList');container.innerHTML='';state.activities.forEach(a=>{const div=document.createElement('div');div.className='p-2 border rounded bg-white shadow-sm';div.textContent=a;container.appendChild(div);});}
function updateBalance(){accountBalance.textContent=state.balance;}

// Update plan info & add daily profit
function updatePlanInfo(){
  if(!state.plan) return;
  const now=new Date();
  const start=new Date(state.plan.startTime);
  const elapsed=Math.floor((now-start)/1000);
  const daysElapsed=Math.floor(elapsed/86400);
  const toAdd=daysElapsed - state.plan.count;
  if(toAdd>0){
    state.balance += state.plan.profit * toAdd;
    state.plan.count += toAdd;
    localStorage.setItem('rockBalance', state.balance);
    localStorage.setItem('rock// Continue from last line
localStorage.setItem('rockPlan', JSON.stringify(state.plan));
  }
  planInfo.textContent = `Plan: Daily Profit PKR ${state.plan.profit} • Duration ${state.plan.duration} days • Days Earned: ${state.plan.count}`;
}

// Auto-update plan profit every 1 hour (simulation)
setInterval(updatePlanInfo, 3600000);
</script>

</body>
</html>
