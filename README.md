<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn – Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;background:linear-gradient(120deg,#0f1123,#1c1f3b);color:white;overflow-x:hidden;}
.fadeIn{animation:fadeIn .6s ease;}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px);}to{opacity:1;transform:translateY(0);}}

/* SIDEBAR */
#sidebar{position:fixed;top:0;left:0;width:220px;height:100vh;background:rgba(20,20,20,0.95);padding:20px;display:none;flex-direction:column;gap:20px;z-index:999;}
#sidebar.show{display:flex;}
#sidebar h2{font-size:20px;font-weight:700;margin-bottom:15px;}
#sidebar p{cursor:pointer;padding:10px;border-radius:12px;transition:0.3s;}
#sidebar p:hover{background:#0ff;color:#000;}

/* DASHBOARD */
#dashboard{display:none;padding-left:240px;padding-top:20px;}
#dashboard h1{font-size:28px;margin-bottom:10px;}
.stats{display:flex;gap:20px;margin:20px 0;}
.stat-card{background:rgba(0,255,255,0.05);padding:12px 20px;border-radius:12px;box-shadow:0 0 5px #0ff,0 0 10px #0ff;}
.stat-label{color:#0ff;font-size:12px;margin-bottom:5px;}
.stat-value{font-size:18px;font-weight:700;color:white;}

/* SECTIONS */
.section{display:none;padding:20px;background:rgba(0,0,0,0.7);border-radius:20px;margin-bottom:20px;}
.plan-card{border:1px solid #0ff;padding:15px;margin-bottom:15px;border-radius:15px;transition:0.3s;cursor:pointer;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #0ff;transform:scale(1.03);}
input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}
.btn{background:#111;color:#0ff;padding:10px 15px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 5px #0ff,0 0 10px #0ff;transition:0.3s;}
.btn:hover{transform:scale(1.05);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#0ff,#0ffcc0);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
</style>
</head>
<body>

<div id="notif"></div>

<!-- LOGIN -->
<div id="loginPage" class="min-h-screen flex flex-col justify-center items-center p-4 fadeIn">
<h1 class="text-3xl font-bold mb-4">Rock Earn Login</h1>
<input id="loginEmail" type="email" placeholder="Email" class="w-72 p-2 mb-3 rounded bg-white/20">
<input id="loginPass" type="password" placeholder="Password" class="w-72 p-2 mb-3 rounded bg-white/20">
<button onclick="login()" class="w-72 p-2 bg-blue-600 rounded mt-2">Login</button>
<p class="mt-3">Create new account? <span onclick="showSignup()" class="text-blue-300 underline cursor-pointer">Sign Up</span></p>
</div>

<!-- SIGNUP -->
<div id="signupPage" class="hidden min-h-screen flex flex-col justify-center items-center p-4 fadeIn">
<h1 class="text-3xl font-bold mb-4">Create Account</h1>
<input id="signupName" type="text" placeholder="Full Name" class="w-72 p-2 mb-3 rounded bg-white/20">
<input id="signupEmail" type="email" placeholder="Email" class="w-72 p-2 mb-3 rounded bg-white/20">
<input id="signupPass" type="password" placeholder="Password" class="w-72 p-2 mb-3 rounded bg-white/20">
<button onclick="signup()" class="w-72 p-2 bg-green-600 rounded mt-2">Sign Up</button>
<p class="mt-3">Already have account? <span onclick="showLogin()" class="text-blue-300 underline cursor-pointer">Login</span></p>
</div>

<!-- DASHBOARD -->
<div id="dashboard">
<h1 id="welcomeUser"></h1>

<div class="stats">
<div class="stat-card"><p class="stat-label">Balance</p><p class="stat-value" id="balValue">0 PKR</p></div>
<div class="stat-card"><p class="stat-label">Total Profit</p><p class="stat-value" id="profValue">0 PKR</p></div>
</div>

<!-- SECTIONS -->
<div id="plansSection" class="section">
<h2 class="text-xl font-bold mb-4">Investment Plans</h2>
<div id="plansContainer"></div>
</div>

<div id="depositSection" class="section">
<h2 class="text-xl font-bold mb-4">Deposit Funds</h2>
<select id="depositMethod">
<option value="JazzCash">JazzCash</option>
<option value="Easypaisa">Easypaisa</option>
</select>
<input id="depositAmount" type="number" placeholder="Amount">
<input id="depositProof" type="text" placeholder="Transaction ID / Proof">
<button onclick="makeDeposit()" class="btn">Submit Deposit</button>
</div>

<div id="withdrawSection" class="section">
<h2 class="text-xl font-bold mb-4">Withdraw Funds</h2>
<input id="withdrawName" placeholder="Full Name">
<input id="withdrawAcc" placeholder="Account Number">
<input id="withdrawAmount" type="number" placeholder="Amount">
<button onclick="makeWithdraw()" class="btn">Request Withdraw</button>
</div>

<div id="profitSection" class="section">
<h2 class="text-xl font-bold mb-4">Daily Profit</h2>
<p id="dailyProfit">0 PKR</p>
</div>

<div id="historySection" class="section">
<h2 class="text-xl font-bold mb-4">Transaction History</h2>
<div id="historyList"></div>
</div>

<div id="settingsSection" class="section">
<h2 class="text-xl font-bold mb-4">Settings</h2>
<button onclick="logout()" class="btn">Logout</button>
</div>

<!-- SIDEBAR -->
<div id="sidebar">
<h2>Menu</h2>
<p onclick="openSection('plansSection')">🔥 Plans</p>
<p onclick="openSection('depositSection')">💰 Deposit</p>
<p onclick="openSection('withdrawSection')">🏧 Withdraw</p>
<p onclick="openSection('profitSection')">📈 Daily Profit</p>
<p onclick="openSection('historySection')">📜 History</p>
<p onclick="openSection('settingsSection')">⚙️ Settings</p>
<p onclick="logout()" class="text-red-400 mt-10">Logout</p>
</div>

<script>
// Notifications
function showNotif(msg){
let n=document.getElementById('notif');
n.innerText=msg;
n.classList.add('show');
setTimeout(()=>{n.classList.remove('show');},3000);
}

// Toggle sections
function openSection(id){
let sections=document.querySelectorAll('.section');
sections.forEach(s=>s.style.display='none');
document.getElementById(id).style.display='block';
}

// Signup / Login
function showSignup(){loginPage.classList.add('hidden');signupPage.classList.remove('hidden');}
function showLogin(){signupPage.classList.add('hidden');loginPage.classList.remove('hidden');}
function signup(){
if(!signupName.value || !signupEmail.value || !signupPass.value){showNotif("Fill all fields");return;}
let user={name:signupName.value,email:signupEmail.value.toLowerCase(),pass:signupPass.value,balance:0,profit:0,history:[]};
localStorage.setItem("rockUser",JSON.stringify(user));
showNotif("Account Created!");
showLogin();
}
function login(){
let user=JSON.parse(localStorage.getItem("rockUser"));
if(!user){showNotif("No account found!");return;}
if(loginEmail.value.toLowerCase()===user.email && loginPass.value===user.pass){
localStorage.setItem("loggedIn","yes");
loadDashboard();
}else{showNotif("Wrong Email or Password");}
}
window.onload=function(){if(localStorage.getItem("loggedIn")==="yes"){loadDashboard();}};

// Dashboard
function loadDashboard(){
loginPage.classList.add('hidden');signupPage.classList.add('hidden');dashboard.style.display='block';
sidebar.classList.add('show');
let user=JSON.parse(localStorage.getItem("rockUser"));
welcomeUser.innerText="Welcome, "+user.name;
balValue.innerText=user.balance+" PKR";profValue.innerText=user.profit+" PKR";
loadPlans();loadHistory();
}

// Logout
function logout(){localStorage.setItem("loggedIn","no");dashboard.style.display='none';sidebar.classList.remove('show');loginPage.classList.remove('hidden');}

// Plans
let plans=[
{name:'Basic',amount:200,daily:30,days:1},
{name:'Starter',amount:500,daily:75,days:1},
{name:'Pro',amount:1000,daily:180,days:1},
{name:'Advanced',amount:1500,daily:250,days:2},
{name:'Silver',amount:2000,daily:350,days:3},
{name:'Gold',amount:3000,daily:550,days:3},
{name:'Platinum',amount:5000,daily:950,days:4},
{name:'Diamond',amount:7000,daily:1350,days:5}
];
function loadPlans(){
let container=document.getElementById('plansContainer');container.innerHTML='';
plans.forEach(p=>{
let div=document.createElement('div');div.className='plan-card';
div.innerHTML=`<h3 class="font-bold">${p.name}</h3><p>Amount: ${p.amount} PKR</p><p>Daily: ${p.daily} PKR</p><p>Days: ${p.days}</p>`;
div.onclick=()=>{buyPlan(p)};
container.appendChild(div);
});
}
function buyPlan(plan){
let user=JSON.parse(localStorage.getItem("rockUser"));
if(user.balance<plan.amount){showNotif("Insufficient Balance");return;}
user.balance-=plan.amount;user.profit+=plan.daily*plan.days;
user.history.push(`Bought ${plan.name} plan for ${plan.amount} PKR`);
localStorage.setItem("rockUser",JSON.stringify(user));
balValue.innerText=user.balance+" PKR";profValue.innerText=user.profit+" PKR";loadHistory();showNotif("Plan Purchased!");
}

// Deposit
function makeDeposit(){
let method=depositMethod.value,amt=depositAmount.value,proof=depositProof.value;
if(!amt || !proof){showNotif("Fill all fields");return;}
let user=JSON.parse(localStorage.getItem("rockUser"));
user.balance=parseInt(user.balance)+parseInt(amt);
user.history.push(`Deposit: ${amt} PKR via ${method}, Proof: ${proof}`);
localStorage.setItem("rockUser",JSON.stringify(user));
balValue.innerText=user.balance+" PKR";loadHistory();showNotif("Deposit Added!");
depositAmount.value='';depositProof.value='';
}

// Withdraw
function makeWithdraw(){
let name=withdrawName.value,acc=withdrawAcc.value,amt=withdrawAmount.value;
if(!name || !acc || !amt){showNotif("Fill all fields");return;}
let user=JSON.parse(localStorage.getItem("rockUser"));
if(user.balance<amt){showNotif("Insufficient Balance");return;}
user.balance-=parseInt(amt);
user.history.push(`Withdraw: ${amt} PKR to ${name} (${acc})`);
localStorage.setItem("rockUser",JSON.stringify(user));
balValue.innerText=user.balance+" PKR";loadHistory();showNotif("Withdraw Requested!");
withdrawName.value='';withdrawAcc.value='';withdrawAmount.value='';
}

// History
function loadHistory(){
let user=JSON.parse(localStorage.getItem("rockUser"));
let list=document.getElementById('historyList');list.innerHTML='';
user.history.forEach(h=>{let p=document.createElement('p');p.innerText=h;list.appendChild(p);});
}

</script>

</body>
</html>
