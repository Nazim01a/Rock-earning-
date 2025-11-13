<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn — Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:#f0f4f8;}
.button-animate:hover{transform:scale(1.05);transition:0.3s;}
.card-hover:hover{transform:translateY(-5px);box-shadow:0 10px 20px rgba(0,0,0,0.25);transition:0.3s;}
.hidden{display:none;}
.bg-gradient-primary{background:linear-gradient(90deg,#4f46e5,#3b82f6);}
.bg-gradient-secondary{background:linear-gradient(90deg,#f97316,#facc15);}
.bg-gradient-success{background:linear-gradient(90deg,#10b981,#3b82f6);}
.bg-gradient-customer{background:linear-gradient(90deg,#f43f5e,#ec4899);}
.copyBtn{cursor:pointer;background:#4f46e5;color:#fff;padding:2px 6px;border-radius:4px;font-size:12px;}
</style>
</head>
<body class="min-h-screen flex flex-col items-center p-4"><header class="w-full max-w-6xl mb-6 p-6 rounded-xl bg-gradient-to-r from-indigo-600 via-pink-500 to-purple-600 text-white shadow-2xl text-center flex flex-col md:flex-row justify-between items-center">
  <div>
    <h1 class="text-4xl font-extrabold">Rock Earn</h1>
    <p class="mt-1 text-base opacity-90">Your interactive earning dashboard</p>
  </div>
  <div class="flex gap-3 mt-4 md:mt-0 hidden" id="dashboardBtns">
    <button id="profileBtn" class="px-4 py-2 bg-white text-indigo-600 font-semibold rounded-xl button-animate">Profile</button>
    <button id="shareBtn" class="px-4 py-2 bg-white text-indigo-600 font-semibold rounded-xl button-animate">Share Link</button>
  </div>
</header><main class="w-full max-w-6xl bg-white rounded-xl shadow-lg p-6 flex flex-col gap-6"><!-- Landing --><div id="landingStep" class="flex flex-col items-center gap-6">
  <h2 class="text-2xl font-bold">Welcome! Login or Sign Up</h2>
  <div class="flex gap-4">
    <button id="loginBtn" class="px-6 py-3 rounded-xl bg-gradient-primary text-white font-semibold button-animate">Login</button>
    <button id="signupBtn" class="px-6 py-3 rounded-xl bg-gradient-secondary text-white font-semibold button-animate">Sign Up</button>
  </div>
</div><!-- Login / SignUp Form --><div id="formStep" class="hidden flex flex-col gap-4">
  <h2 class="text-2xl font-bold" id="formTitle">Login</h2>
  <form id="authForm" class="flex flex-col gap-4">
    <input name="name" id="nameInput" placeholder="Full Name" class="p-3 rounded-lg border border-gray-200">
    <input type="email" name="email" id="emailInput" placeholder="Email" class="p-3 rounded-lg border border-gray-200" required>
    <input type="password" name="password" id="passwordInput" placeholder="Password" class="p-3 rounded-lg border border-gray-200" required>
    <div class="flex items-center gap-3">
      <input type="checkbox" id="termsCheck">
      <label for="termsCheck" class="text-sm">I agree to the <a href="#" class="underline text-indigo-600">Terms & Conditions</a></label>
    </div>
    <button type="submit" class="py-3 rounded-xl bg-gradient-primary text-white font-semibold button-animate">Submit</button>
  </form>
  <button id="backLanding" class="mt-2 text-sm text-gray-500 underline">Back</button>
</div><!-- Dashboard --><div id="dashboardStep" class="hidden flex flex-col gap-6">
  <div class="flex flex-col md:flex-row justify-between items-center gap-4">
    <div>
      <h2 class="text-2xl font-bold">Welcome, <span id="userName">User</span></h2>
      <p class="text-gray-500">Your personalized dashboard</p>
    </div>
    <div class="p-4 rounded-xl shadow-xl bg-gradient-success text-white">
      <h3 class="font-semibold">Account Balance</h3>
      <div class="text-2xl font-bold">PKR <span id="accountBalance">0</span></div>
    </div>
  </div>  <!-- Company Info -->  <div class="p-4 rounded-xl bg-gray-100 border shadow-sm text-gray-700 flex flex-col md:flex-row justify-between items-start gap-4">
    <div>
      <h3 class="font-semibold mb-1">Company Info</h3>
      <p>Rock Earn LLC, founded 2018, California. Trusted simulated earning platform offering interactive plans, deposits, withdrawals and unique features for all users. Our aim is to make earning simple and secure.</p>
    </div>
  </div>  <!-- Available Plans -->  <div>
    <h3 class="font-semibold mb-2">Available Plans</h3>
    <div class="grid md:grid-cols-3 gap-4" id="plansContainer"></div>
  </div>  <!-- Deposit / Withdraw -->  <div class="grid md:grid-cols-2 gap-4 mt-4">
    <div id="depositSection" class="p-4 rounded-xl bg-gray-50 border shadow-sm hidden flex flex-col gap-2">
      <h3 class="font-semibold mb-2">Make a Deposit (Simulation)</h3>
      <div class="flex flex-col gap-2">
        <div class="flex justify-between items-center">
          <span>Easypaisa: 03379827882</span>
          <span class="copyBtn" onclick="copyNumber('03379827882')">Copy</span>
        </div>
        <div class="flex justify-between items-center">
          <span>JazzCash: 03705519562</span>
          <span class="copyBtn" onclick="copyNumber('03705519562')">Copy</span>
        </div>
      </div>
      <div id="depositFormFields" class="hidden flex flex-col gap-2 mt-2">
        <label>Amount (PKR)</label>
        <input id="depositAmount" class="p-2 rounded-lg border border-gray-200">
        <label>Transaction ID</label>
        <input id="depositTxn" class="p-2 rounded-lg border border-gray-200">
        <label>Upload Proof</label>
        <input type="file" id="depositProof" accept="image/*">
        <button id="submitDeposit" class="py-2 rounded-xl bg-gradient-secondary text-white button-animate">Submit Deposit</button>
      </div>
    </div><div id="withdrawSection" class="p-4 rounded-xl bg-gray-50 border shadow-sm hidden flex flex-col gap-2">
  <h3 class="font-semibold mb-2">Request Withdrawal (Simulation)</h3>
  <label>Amount (PKR)</label>
  <input id="withdrawAmount" class="p-2 rounded-lg border border-gray-200">
  <label>Method</label>
  <select id="withdrawMethod" class="p-2 rounded-lg border border-gray-200">
    <option value="Easypaisa">Easypaisa</option>
    <option value="JazzCash">JazzCash</option>
    <option value="Bank">Bank</option>
  </select>
  <label>Note</label>
  <input id="withdrawNote" class="p-2 rounded-lg border border-gray-200">
  <button id="requestWithdraw" class="py-2 rounded-xl bg-amber-500 text-white button-animate">Submit Withdrawal</button>
</div>

  </div>  <!-- Activity Log -->  <div id="activityLog" class="p-4 rounded-xl bg-gray-50 border shadow-sm mt-4">
    <h3 class="font-semibold mb-2">Activity / Requests Log</h3>
    <div id="activityList" class="space-y-2 text-sm text-gray-700"></div>
  </div></main><script>
let state={user:null,balance:0,plan:null,activities:[],shareLink:'https://rockearn.com/ref/abc123'};

const landingStep=document.getElementById('landingStep');
const formStep=document.getElementById('formStep');
const dashboardStep=document.getElementById('dashboardStep');
const authForm=document.getElementById('authForm');
const formTitle=document.getElementById('formTitle');
const userNameSpan=document.getElementById('userName');
const backLanding=document.getElementById('backLanding');
const loginBtn=document.getElementById('loginBtn');
const signupBtn=document.getElementById('signupBtn');
const dashboardBtns=document.getElementById('dashboardBtns');
const depositSection=document.getElementById('depositSection');
const withdrawSection=document.getElementById('withdrawSection');
const plansContainer=document.getElementById('plansContainer');

// Plan data
const plans=[
  {name:'Basic',amount:180,profit:30,duration:30},
  {name:'Standard',amount:280,profit:50,duration:30},
  {name:'Silver',amount:500,profit:100,duration:30},
  {name:'Gold',amount:1000,profit:200,duration:30},
  {name:'Platinum',amount:2000,profit:400,duration:30},
  {name:'Diamond',amount:5000,profit:1000,duration:30},
  {name:'Ultimate',amount:7000,profit:1500,duration:30}
];

// Render plans
plans.forEach(p=>{
  const div=document.createElement('div');
  div.className='p-4 rounded-xl bg-white shadow-md card-hover flex flex-col gap-2';
  div.innerHTML=`<div class='text-gray-500 text-sm'>${p.name}</div>
    <div class='font-bold text-xl'>PKR ${p.amount}</div>
    <div class='text-xs text-gray-400'>Daily Profit PKR ${p.profit} • Duration ${p.duration} days</div>
    <button class='mt-2 px-3 py-2 rounded-xl bg-gradient-primary text-white font-semibold buyPlan'>Buy Plan</button>`;
  div.querySelector('.buyPlan').onclick=()=>{
    state.plan=p;
    depositSection.classList.remove('hidden');
    alert(`Plan ${p.name} selected! Proceed to deposit.`);
  };
  plansContainer.appendChild(div);
});

// Landing buttons
loginBtn.onclick=()=>{showForm('Login');};
signupBtn.onclick=()=>{showForm('Sign Up');};
backLanding.onclick=()=>{formStep.classList.add('hidden');landingStep.classList.remove('hidden');};

function showForm(type){
  formTitle.textContent=type;
  landingStep.classList.add('hidden');
  formStep.classList.remove('hidden');
  document.getElementById('nameInput').style.display=type==='Login'?'none':'block';
}

// Auth form submit
authForm.onsubmit=e=>{
  e.preventDefault();
  const name=document.getElementById('nameInput').value;
  const email=document.getElementById('emailInput').value;
  const password=document.getElementById('passwordInput').value;
  const terms=document.getElementById('termsCheck').checked;
  if(formTitle.textContent==='Sign Up' && !name){alert('Enter Full Name');return;}
  if(formTitle.textContent==='Sign Up' && !terms){alert('Agree to Terms');return;}
  state.user={name:name||email,email,password};
  localStorage.setItem('rockUser',JSON.stringify(state.user));
  userNameSpan.textContent=state.user.name;
  formStep.classList.add('hidden');
  dashboardStep.classList.remove('hidden');
  dashboardBtns.classList.remove('hidden');
  depositSection.classList.add('hidden'); // show on plan click
  withdrawSection.classList.remove('hidden');
  alert(`${formTitle.textContent} successful!`);
};

// Deposit copy function
function copyNumber(num){navigator.clipboard.writeText(num);alert(`Copied: ${num}`);}
</script></body>
</html>
