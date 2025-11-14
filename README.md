<ROCK ON><html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Rock Earn Ultra Pro Premium Animated</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{
  font-family:'Segoe UI',sans-serif;
  background:#0a0f1e;
  color:white;
  overflow-x:hidden;
}
.neon-card{
  transition:0.3s;
  border:1px solid rgba(255,255,255,0.1);
  background:rgba(255,255,255,0.05);
  backdrop-filter:blur(20px);
}
.neon-card:hover{
  transform:scale(1.05);
  box-shadow:0 0 30px rgba(0,255,255,0.8);
}
#sidePanel{
  position:fixed;
  top:0;
  right:-400px;
  width:400px;
  height:100vh;
  background:#0c1230;
  padding:20px;
  transition:0.4s;
  overflow-y:auto;
  z-index:999;
}
.btn-primary{
  background:#00ffe0;
  color:#000;
  font-weight:600;
  transition:0.3s;
}
.btn-primary:hover{
  transform:scale(1.05);
}
@keyframes fadeInUp{
  0%{opacity:0; transform:translateY(20px);}
  100%{opacity:1; transform:translateY(0);}
}
.animated{
  animation: fadeInUp 0.7s ease forwards;
}
@keyframes glow{
  0% {text-shadow:0 0 5px #00ffe0,0 0 10px #00bfff;}
  50% {text-shadow:0 0 15px #ff00ff,0 0 25px #00ffe0;}
  100% {text-shadow:0 0 5px #00ffe0,0 0 10px #00bfff;}
}
.glow-text{
  background: linear-gradient(90deg, #00ffe0, #00bfff, #ff00ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: glow 3s ease-in-out infinite;
}
</style>
</head>
<body class="p-4"><!-- AUTH SYSTEM --><div id="authBox" class="card p-6 mb-6 max-w-md mx-auto animated">
  <h2 class="text-3xl font-bold mb-4 text-center glow-text">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" class="w-full p-2 text-black rounded mb-2" />
  <input id="authEmail" placeholder="Email" class="w-full p-2 text-black rounded mb-2" />
  <input id="authPass" type="password" placeholder="Password" class="w-full p-2 text-black rounded mb-2" />
  <button onclick="signupUser()" class="btn btn-primary w-full mb-2">Sign Up</button>
  <button onclick="loginUser()" class="btn btn-primary w-full mb-2">Login</button>
  <button onclick="forgotPassword()" class="w-full p-2 rounded bg-yellow-500 font-semibold">Forgot Password?</button>
</div><script>
let users = JSON.parse(localStorage.getItem("reUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("reCurrent")) || null;
if(currentUser){ loadDashboard(); }
function signupUser(){
  let n=document.getElementById('authName').value.trim();
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  if(!n||!e||!p){alert('Fill all fields'); return;}
  if(users.find(u=>u.email===e)){alert('Email already registered'); return;}
  let u={name:n,email:e,pass:p};
  users.push(u);
  localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  loadDashboard();
}
function loginUser(){
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  let u=users.find(u=>u.email===e && u.pass===p);
  if(!u){alert('Invalid credentials'); return;}
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  loadDashboard();
}
function forgotPassword(){
  let email=prompt('Enter your registered email');
  let user=users.find(u=>u.email===email);
  if(user){ alert('Your password is: '+user.pass); } else { alert('Email not found');}
}
</script><!-- HEADER --><header class="text-center py-6 animated">
  <h1 class="text-5xl font-bold mb-2 glow-text">🚀 Rock Earn Ultra Pro Premium</h1>
  <p class="opacity-70">Welcome, <span id="welcomeName">Guest</span> • Since 2018 • Crypto FinTech</p>
</header><!-- ICON MENU --><div class="flex gap-4 flex-wrap justify-center mt-4 animated" id="iconMenu">
  <button onclick="openPanel('profile')" class="bg-blue-600 px-4 py-2 rounded-lg">Profile</button>
  <button onclick="openPanel('plans')" class="bg-purple-600 px-4 py-2 rounded-lg">Plans</button>
  <button onclick="openPanel('deposit')" class="bg-green-600 px-4 py-2 rounded-lg">Deposit</button>
  <button onclick="openPanel('withdraw')" class="bg-yellow-600 px-4 py-2 rounded-lg">Withdraw</button>
  <button onclick="openPanel('activity')" class="bg-pink-600 px-4 py-2 rounded-lg">Activity</button>
  <button onclick="openPanel('company')" class="bg-gray-600 px-4 py-2 rounded-lg">Company</button>
  <button onclick="openPanel('settings')" class="bg-red-600 px-4 py-2 rounded-lg">Settings</button>
  <button onclick="logout()" class="bg-black px-4 py-2 rounded-lg mt-6">Logout</button>
</div><!-- PLANS SECTION --><section class="mt-10 text-center animated" id="plansSection">
  <h2 class="text-3xl font-bold mb-6 glow-text">Investment Plans</h2>
  <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="plansContainer"></div>
</section><!-- SIDE PANEL --><div id="sidePanel"></div><script>
const plans=[
{name:'Starter Plan',price:180,daily:20},
{name:'Basic Plan',price:500,daily:60},
{name:'Standard Plan',price:1000,daily:130},
{name:'Premium Plan',price:2500,daily:320},
{name:'Gold Plan',price:5000,daily:650},
{name:'Elite Plan',price:7000,daily:900},
{name:'Ultra Plan',price:10000,daily:1500},
{name:'Titan Plan',price:15000,daily:2300},
{name:'Coming Soon 1',price:0,daily:0},
{name:'Coming Soon 2',price:0,daily:0}
];
function loadDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('welcomeName').innerText=currentUser.name;
  renderPlans();
}
function renderPlans(){
  let container=document.getElementById('plansContainer');
  container.innerHTML='';
  plans.forEach(p=>{
    let card=document.createElement('div');
    card.className='neon-card p-6 rounded-2xl animated';
    if(p.price===0){ card.classList.add('opacity-50'); }
    card.innerHTML=`<h3 class='text-xl font-semibold mb-1'>${p.name}</h3>
                      <p>Amount: ${p.price} PKR</p>
                      <p>Daily Profit: ${p.daily} PKR</p>
                      ${p.price>0?`<button onclick="buyPlan(${p.price})" class='mt-3 bg-blue-600 px-4 py-2 rounded-lg'>Buy Now</button>`:''}`;
    container.appendChild(card);
  });
}
function openPanel(type){
  let panel=document.getElementById('sidePanel');
  panel.style.right='0px';
  switch(type){
    case 'profile': panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Profile</h2><p>Name: ${currentUser.name}</p>`; break;
    case 'plans': panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Plans</h2><p>Check available plans below</p>`; break;
    case 'deposit': panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Deposit</h2>`; break;
    case 'withdraw': panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Withdraw</h2>`; break;
    case 'activity': panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Activity</h2>`; break;
    case 'company': panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Company</h2>`; break;
    case 'settings': panel.innerHTML=`<h2 class='text-2xl font-bold mb-4'>Settings</h2>`; break;
  }
}
function buyPlan(amount){
  openPanel('deposit');
  document.getElementById('sidePanel').innerHTML+=`<p>Selected Plan Amount: ${amount} PKR</p>`;
}
function logout(){
  localStorage.removeItem('reCurrent');
  location.reload();
}
</script></body>
</html>
