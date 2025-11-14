<ROCK ON>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Ultra Pro Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(135deg,#0b0f1a,#1c1f3b);color:white;overflow-x:hidden;}
.neon-card{transition:0.3s;border:1px solid rgba(255,255,255,0.1);background:rgba(255,255,255,0.05);backdrop-filter:blur(15px);}
.neon-card:hover{transform:scale(1.05);box-shadow:0 0 25px rgba(0,150,255,0.7);}
.btn-glow{transition:0.3s;box-shadow:0 0 5px #00f,0 0 10px #00f;}
.btn-glow:hover{transform:scale(1.05);box-shadow:0 0 15px #00f,0 0 30px #0ff;}
#sidePanel{position:fixed;top:0;right:-400px;width:380px;height:100vh;background:#0f172a;padding:20px;transition:0.4s;overflow-y:auto;z-index:999;}
.animate-welcome{animation:fadeIn 2s ease-in-out infinite alternate;}
@keyframes fadeIn{0%{opacity:0.5;}100%{opacity:1;}}
input, select{background:rgba(255,255,255,0.1);color:white;border:none;padding:10px;width:100%;border-radius:8px;margin-top:8px;}
</style>
</head>
<body class="p-4">

<!-- AUTH BOX -->
<div id="authBox" class="card mb-6 p-6 max-w-md mx-auto">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" class="w-full p-2 text-black rounded mb-2" />
<input id="authEmail" placeholder="Email" class="w-full p-2 text-black rounded mb-2" />
<input id="authPass" type="password" placeholder="Password" class="w-full p-2 text-black rounded mb-2" />
<button onclick="signupUser()" class="btn-glow w-full mb-2 bg-blue-600">Sign Up</button>
<button onclick="loginUser()" class="btn-glow w-full mb-2 bg-green-600">Login</button>
<p class="text-center mt-2"><a href="#" onclick="forgotPass()" class="text-yellow-400">Forgot Password?</a></p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="hidden">

<header class="text-center py-6">
  <h1 class="text-4xl font-bold">🚀 Rock Earn Ultra Pro</h1>
  <p class="opacity-70 animate-welcome" id="welcomeUser"></p>
  <p class="opacity-50">Since 2018 • Crypto FinTech • Partnered with Binance</p>
</header>

<!-- ICONS MENU -->
<div class="flex gap-4 flex-wrap justify-center mt-4 mb-6">
  <button onclick="openPanel('profile')" class="bg-blue-600 px-4 py-2 rounded-lg btn-glow">Profile</button>
  <button onclick="openPanel('plans')" class="bg-indigo-600 px-4 py-2 rounded-lg btn-glow">Plans</button>
  <button onclick="openPanel('deposit')" class="bg-green-600 px-4 py-2 rounded-lg btn-glow">Deposit</button>
  <button onclick="openPanel('withdraw')" class="bg-yellow-600 px-4 py-2 rounded-lg btn-glow">Withdraw</button>
  <button onclick="openPanel('activity')" class="bg-purple-600 px-4 py-2 rounded-lg btn-glow">Activity</button>
  <button onclick="openPanel('company')" class="bg-gray-600 px-4 py-2 rounded-lg btn-glow">Company</button>
  <button onclick="openPanel('settings')" class="bg-red-600 px-4 py-2 rounded-lg btn-glow">Settings</button>
  <button onclick="logoutUser()" class="bg-red-700 px-4 py-2 rounded-lg btn-glow">Logout</button>
</div>

<!-- PLANS SECTION -->
<section class="text-center mb-10">
  <h2 class="text-3xl font-bold mb-6">Our Investment Plans</h2>
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="plansContainer"></div>
</section>

<!-- SIDE PANEL -->
<div id="sidePanel"></div>

<script>
// USERS & CURRENT USER
let users = JSON.parse(localStorage.getItem("reUsers"))||[];
let currentUser = JSON.parse(localStorage.getItem("reCurrent"))||null;

// PLANS DATA
const plans = [
  {name:"Starter",amount:180,daily:20},
  {name:"Basic",amount:500,daily:60},
  {name:"Standard",amount:1000,daily:130},
  {name:"Premium",amount:2500,daily:320},
  {name:"Gold",amount:5000,daily:650},
  {name:"Elite",amount:7000,daily:900},
  {name:"Mega Booster (Coming Soon)",amount:10000,daily:1500,coming:true},
  {name:"Ultra Pro (Coming Soon)",amount:15000,daily:2300,coming:true},
  {name:"Crypto Miner (Coming Soon)",amount:20000,daily:3500,coming:true}
];

// LOAD DASHBOARD ON PAGE LOAD
window.onload = function(){
  if(currentUser){
    document.getElementById('authBox').style.display='none';
    document.getElementById('dashboard').style.display='block';
    updateWelcome();
    showPlans();
  }
}

// AUTH FUNCTIONS
function signupUser(){
  let n=document.getElementById('authName').value.trim();
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  if(!n||!e||!p){alert('Fill all fields');return;}
  if(users.find(u=>u.email===e)){alert('Email already registered');return;}
  let u={name:n,email:e,pass:p};
  users.push(u); localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(u));
  document.getElementById('authBox').style.display='none';
  document.getElementById('dashboard').style.display='block';
  updateWelcome();
  showPlans();
}

function loginUser(){
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  let u=users.find(u=>u.email===e && u.pass===p);
  if(!u){alert('Invalid credentials');return;}
  currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(u));
  document.getElementById('authBox').style.display='none';
  document.getElementById('dashboard').style.display='block';
  updateWelcome();
  showPlans();
}

function forgotPass(){
  let e=prompt("Enter your registered email:");
  let user=users.find(u=>u.email===e);
  if(user){alert("Your password is: "+user.pass);}
  else{alert("Email not found.");}
}

function logoutUser(){
  currentUser=null;
  localStorage.removeItem('reCurrent');
  location.reload();
}

// UPDATE WELCOME
function updateWelcome(){
  if(currentUser){
    document.getElementById('welcomeUser').innerText='Welcome, '+currentUser.name+'!';
  }
}

// OPEN SIDE PANEL
function openPanel(type){
  const panel=document.getElementById('sidePanel');
  panel.style.right='0px';
  if(type==='profile'){
    panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Your Profile</h2>
      <p>Name: ${currentUser.name}</p>
      <p>Email: ${currentUser.email}</p>
      <button class='bg-blue-600 px-3 py-1 rounded mt-2 btn-glow' onclick="copyText('${currentUser.email}')">Copy Profile Link</button>`;
  }
  if(type==='plans'){ showPlans(true); }
  if(type==='deposit'){
    panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Deposit</h2>
      <p class='mb-2'>Amount Selected: <b id='depositAmount'>0</b> PKR</p>
      <p>JazzCash: 03705519562 <button class='bg-blue-600 px-2 rounded ml-2 btn-glow' onclick="copyText('03705519562')">Copy</button></p>
      <p class='mt-2'>EasyPaisa: 03379827882 <button class='bg-blue-600 px-2 rounded ml-2 btn-glow' onclick="copyText('03379827882')">Copy</button></p>
      <input type='file' class='mt-3 w-full p-2 rounded bg-white/20'>
      <input type='text' placeholder='Transaction ID' class='mt-3 p-2 w-full rounded bg-white/20'>
      <button class='bg-green-600 px-4 py-2 rounded mt-4 w-full btn-glow'>Submit</button>`;
  }
  if(type==='withdraw'){
    panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Withdraw</h2>
      <input type='number' placeholder='Amount' class='w-full p-2 rounded bg-white/20 mb-3'>
      <select class='w-full p-2 rounded bg-white/20 mb-3'>
        <option>JazzCash</option>
        <option>EasyPaisa</option>
        <option>Bank</option>
      </select>
      <input type='text' placeholder='Account Number' class='w-full p-2 rounded bg-white/20 mb-3'>
      <button class='bg-yellow-600 px-4 py-2 rounded w-full btn-glow'>Withdraw</button>`;
  }
  if(type==='activity'){
    panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Activity History</h2>
      <p>• Deposit: 500 PKR</p>
      <p>• Withdraw: 300 PKR</p>
      <p>• Plan Bought: Basic Plan</p>`;
  }
  if(type==='company'){
    panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Company Details</h2>
      <p>Since: <b>2018</b></p>
      <p>Industry: <b>Crypto & FinTech</b></p>
      <p>Partner: <b>Binance</b></p>
      <p>Address: <b>1234 Crypto Avenue, San Francisco, California, USA</b></p>
      <p>Email: support@rockearnpro.com</p>`;
  }
  if(type==='settings'){
    panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Settings</h2>
      <input type='text' placeholder='Change Name' class='w-full p-2 rounded bg-white/20 mb-3'>
      <input type='password' placeholder='Change Password' class='w-full p-2 rounded bg-white/20 mb-3'>
      <button class='bg-red-600 px-4 py-2 rounded w-full btn-glow'>Save</button>`;
  }
}

// SHOW PLANS
function showPlans(openPanelDirect=false){
  const container=document.getElementById('plansContainer');
  container.innerHTML='';
  plans.forEach(p=>{
    container.innerHTML+=`<div class='neon-card p-6 rounded-2xl ${p.coming?'opacity-50':''}'>
      <h3 class='text-xl font-semibold mb-1'>${p.name}</h3>
      <p>Amount: ${p.amount} PKR</p>
      <p>Daily Profit: ${p.daily} PKR</p>
      ${!p.coming?`<button onclick="buyPlan(${p.amount})" class="mt-3 bg-blue-600 px-4 py-2 rounded-lg btn-glow">Buy Now</button>`:''}
      </div>`;
  });
  if(openPanelDirect) document.getElementById('sidePanel').style.right='0px';
}

// BUY PLAN
function buyPlan(amount){
  openPanel('deposit');
  document.getElementById('depositAmount').innerText=amount;
}

// COPY TEXT
function copyText(text){
  navigator.clipboard.writeText(text);
  alert("Copied: "+text);
}
</script>
</body>
</html>
