<ROCK ON><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Ultra Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(160deg,#0b0f1a,#001f3f);color:white;overflow-x:hidden;}
.neon-card{transition:0.4s;border:1px solid rgba(255,255,255,0.1);background:rgba(255,255,255,0.05);backdrop-filter:blur(15px);}
.neon-card:hover{transform:scale(1.05);box-shadow:0 0 25px rgba(0,150,255,0.7);}
.btn{transition:0.3s;}
.btn:hover{transform:scale(1.05);opacity:0.85;}
#sidePanel{position:fixed;top:0;right:-400px;width:360px;height:100vh;background:#0f172a;padding:20px;transition:0.4s;overflow-y:auto;z-index:999;box-shadow:-5px 0 25px rgba(0,0,0,0.7);border-left:1px solid rgba(255,255,255,0.1);}
@keyframes glow {0%{text-shadow:0 0 5px #00f;}50%{text-shadow:0 0 20px #0ff;}100%{text-shadow:0 0 5px #00f;}}
.animate-glow{animation:glow 1.5s infinite;}
</style>
</head>
<body class="p-4"><!-- AUTH SYSTEM --><div id="authBox" class="card mb-6 p-6 max-w-md mx-auto rounded-xl">
<h2 class="text-2xl font-bold mb-4 text-center animate-glow">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" class="w-full p-2 text-black rounded mb-2" />
<input id="authEmail" placeholder="Email" class="w-full p-2 text-black rounded mb-2" />
<input id="authPass" type="password" placeholder="Password" class="w-full p-2 text-black rounded mb-2" />
<div class="flex gap-2">
<button onclick="signupUser()" class="btn w-full bg-blue-600 py-2 rounded">Sign Up</button>
<button onclick="loginUser()" class="btn w-full bg-green-600 py-2 rounded">Login</button>
</div>
<button onclick="forgotPassword()" class="mt-2 text-sm text-blue-400 underline">Forgot Password?</button>
</div><!-- HEADER --><header class="text-center py-6">
<h1 class="text-4xl font-bold animate-glow">🚀 Rock Earn Ultra Premium</h1>
<p class="opacity-70">Since 2018 • Crypto FinTech • Partnered with Binance</p>
</header><!-- ICONS MENU --><div class="flex flex-wrap gap-4 justify-center mt-4">
<button onclick="openPanel('profile')" class="bg-blue-600 px-4 py-2 rounded-lg btn">Profile</button>
<button onclick="openPanel('plans')" class="bg-purple-600 px-4 py-2 rounded-lg btn">Plans</button>
<button onclick="openPanel('deposit')" class="bg-green-600 px-4 py-2 rounded-lg btn">Deposit</button>
<button onclick="openPanel('withdraw')" class="bg-yellow-600 px-4 py-2 rounded-lg btn">Withdraw</button>
<button onclick="openPanel('activity')" class="bg-pink-600 px-4 py-2 rounded-lg btn">Activity</button>
<button onclick="openPanel('company')" class="bg-gray-600 px-4 py-2 rounded-lg btn">Company</button>
<button onclick="openPanel('settings')" class="bg-red-600 px-4 py-2 rounded-lg btn">Settings</button>
<button onclick="logout()" class="bg-black px-4 py-2 rounded-lg btn">Logout</button>
</div><!-- PLANS SECTION --><section class="mt-10 text-center">
<h2 class="text-3xl font-bold mb-6 animate-glow">Our Investment Plans</h2>
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="plansContainer"></div>
</section><!-- SIDE PANEL --><div id="sidePanel"></div><script>
let users = JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser = JSON.parse(localStorage.getItem('reCurrent'))||null;
let plans = [
{name:'Starter',amount:180,daily:20},
{name:'Basic',amount:500,daily:60},
{name:'Standard',amount:1000,daily:130},
{name:'Premium',amount:2500,daily:320},
{name:'Gold',amount:5000,daily:650},
{name:'Elite',amount:7000,daily:900},
{name:'Mega Booster (Coming Soon)',amount:10000,daily:1500,coming:true},
{name:'Ultra Pro (Coming Soon)',amount:15000,daily:2300,coming:true},
{name:'Crypto Miner (Coming Soon)',amount:20000,daily:3500,coming:true}
];

function saveUsers(){localStorage.setItem('reUsers',JSON.stringify(users));}
function saveCurrent(){localStorage.setItem('reCurrent',JSON.stringify(currentUser));}

function signupUser(){
let n=document.getElementById('authName').value.trim();
let e=document.getElementById('authEmail').value.trim();
let p=document.getElementById('authPass').value.trim();
if(!n||!e||!p){alert('Fill all fields');return;}
if(users.find(u=>u.email===e)){alert('Email already registered');return;}
let u={name:n,email:e,pass:p};users.push(u);saveUsers();currentUser=u;saveCurrent();loadDashboard();}

function loginUser(){
let e=document.getElementById('authEmail').value.trim();
let p=document.getElementById('authPass').value.trim();
let u=users.find(u=>u.email===e && u.pass===p);
if(!u){alert('Invalid credentials');return;}
currentUser=u;saveCurrent();loadDashboard();}

function forgotPassword(){
let e=prompt('Enter your registered email:');
if(!e) return;
let u=users.find(u=>u.email===e);
if(u) alert('Your password is: '+u.pass);
else alert('Email not found');
}

function loadDashboard(){
document.getElementById('authBox').style.display='none';
document.getElementById('plansContainer').innerHTML='';
plans.forEach(p=>{
let card=document.createElement('div');
card.className='neon-card p-6 rounded-2xl';
card.innerHTML=`<h3 class='text-xl font-semibold mb-1'>${p.name}</h3><p>Amount: ${p.amount} PKR</p><p>Daily Profit: ${p.daily} PKR</p>${p.coming?'<span class="opacity-50">Coming Soon</span>':'<button onclick="buyPlan('+p.amount+')" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg btn">Buy Now</button>'}`;
document.getElementById('plansContainer').appendChild(card);
});
}

function openPanel(type){
let panel=document.getElementById('sidePanel');panel.style.right='0';
if(type==='profile') panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Profile</h2><p>Name: ${currentUser.name}</p><p>Email: ${currentUser.email}</p>`;
if(type==='plans') panel.innerHTML=document.getElementById('plansContainer').innerHTML;
if(type==='deposit') panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Deposit</h2><p>JazzCash: 03705519562 <button onclick="copyText('03705519562')" class='bg-blue-600 px-2 py-1 rounded btn'>Copy</button></p><p>EasyPaisa: 03379827882 <button onclick="copyText('03379827882')" class='bg-blue-600 px-2 py-1 rounded btn'>Copy</button></p><input type='file' class='mt-3'><input type='text' placeholder='Transaction ID' class='mt-3 p-2 w-full rounded bg-white/20'>`;
if(type==='withdraw') panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Withdraw</h2><input type='number' placeholder='Amount' class='w-full p-2 rounded bg-white/20 mb-3'><select class='w-full p-2 rounded bg-white/20 mb-3'><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type='text' placeholder='Account Number' class='w-full p-2 rounded bg-white/20 mb-3'>`;
if(type==='activity') panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Activity History</h2><p>• Deposit: 500 PKR</p><p>• Withdraw: 300 PKR</p><p>• Plan Bought: Basic Plan</p>`;
if(type==='company') panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Company Details</h2><p>Since 2018</p><p>Industry: Crypto & FinTech</p><p>Partner: Binance</p><p>Address: 1234 Crypto Avenue, San Francisco, CA, USA</p><p>Email: support@rockearnpro.com</p>`;
if(type==='settings') panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Settings</h2><input type='text' placeholder='Change Name' class='w-full p-2 rounded bg-white/20 mb-3'><input type='password' placeholder='Change Password' class='w-full p-2 rounded bg-white/20 mb-3'><button class='bg-red-600 px-4 py-2 rounded w-full'>Save</button>`;
}

function buyPlan(amount){openPanel('deposit');panelDepositAmount(amount);}
function panelDepositAmount(amount){let el=document.getElementById('sidePanel');if(el) el.querySelector('#depositAmount')?.innerText=amount;}
function copyText(t){navigator.clipboard.writeText(t);alert('Copied: '+t);}
function logout(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}

window.onload=function(){if(currentUser) loadDashboard();};
</script></body>
</html>
