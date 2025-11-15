<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Pro — Premium Plans</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(120deg,#0a0f1f,#1b1f3b);color:white;overflow-x:hidden;animation:bgAnim 20s infinite alternate;}
@keyframes bgAnim{0%{background:linear-gradient(120deg,#0a0f1f,#1b1f3b);}50%{background:linear-gradient(120deg,#1b1f3b,#0a0f1f);}100%{background:linear-gradient(120deg,#0a0f1f,#1b1f3b);}}
.card{background:rgba(255,255,255,0.05);border-radius:20px;padding:20px;backdrop-filter:blur(15px);box-shadow:0 0 30px #0ff6;transition:0.3s;transform:translateY(0);animation:fadeIn 1s ease forwards;}
@keyframes fadeIn{0%{opacity:0;transform:translateY(30px);}100%{opacity:1;transform:translateY(0);}}
.card:hover{transform:scale(1.05);box-shadow:0 0 35px #0ff, 0 0 60px #4f46e5;}
.btn{background:#4f46e5;padding:12px 18px;border-radius:12px;font-weight:700;transition:0.3s;animation:btnPulse 2s infinite;}
.btn:hover{background:#4338ca;transform:scale(1.08);}
@keyframes btnPulse{0%{box-shadow:0 0 5px #0ff;}50%{box-shadow:0 0 25px #4f46e5;}100%{box-shadow:0 0 5px #0ff;}}
input,select{padding:12px;border-radius:12px;width:100%;margin-bottom:12px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}
.icon-btn{display:flex;align-items:center;gap:10px;font-weight:600;transition:0.3s;animation:iconHover 1.5s infinite alternate;justify-content:flex-start;padding:12px;}
.icon-btn:hover{transform:scale(1.05);}
@keyframes iconHover{0%{transform:translateY(0);}50%{transform:translateY(-3px);}100%{transform:translateY(0);}}
#sidePanel{display:none; position:fixed; top:0; left:-450px; width:400px; max-width:90%; height:100vh; background:rgba(15,23,42,0.95); padding:40px 20px 20px 20px; transition:0.5s; overflow-y:auto; z-index:9999;}
#sidePanel.active{left:0;}
.sidebar{position:fixed;top:0;left:0;width:70px;height:100vh;background:rgba(0,0,0,0.3);display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:12px;animation:slideSidebar 1s ease forwards;}
@keyframes slideSidebar{0%{left:-70px;}100%{left:0;}}
.header-logo{font-size:30px;font-weight:900;text-align:center;background:linear-gradient(90deg,#1e3a8a,#4f46e5);-webkit-background-clip:text;-webkit-text-fill-color:transparent;animation:logoGlow 2.5s infinite alternate;margin-bottom:20px;}
@keyframes logoGlow{0%{text-shadow:0 0 5px #4f46e5;}50%{text-shadow:0 0 20px #0ff;}100%{text-shadow:0 0 5px #4f46e5;}}
.logout-btn{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);width:200px;animation:btnPulse 2s infinite;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;animation:planFade 0.6s ease forwards;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #4f46e5;transform:scale(1.04);}
@keyframes planFade{0%{opacity:0;transform:translateX(-20px);}100%{opacity:1;transform:translateX(0);}}
#notif{position:fixed;top:20px;right:-400px;background:#4f46e5;padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
</style>
</head>
<body class="p-4">

<div id="notif"></div>

<!-- SIDEBAR -->
<div class="sidebar">
  <div class="header-logo">🚀</div>
  <button class="icon-btn btn" onclick="openPanel('wallet')"><i class="fa fa-wallet"></i></button>
  <button class="icon-btn btn" onclick="openPanel('profile')"><i class="fa fa-user"></i></button>
  <button class="icon-btn btn" onclick="openPanel('plans')"><i class="fa fa-gem"></i></button>
  <button class="icon-btn btn" onclick="openPanel('activity')"><i class="fa fa-list"></i></button>
</div>

<!-- AUTH -->
<div id="authBox" class="card mb-6" style="margin-left:90px;">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" />
  <input id="authEmail" placeholder="Email" />
  <input id="authPass" type="password" placeholder="Password" />
  <button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
  <button onclick="loginUser()" class="btn w-full bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>

<!-- WELCOME / WALLET -->
<div id="welcomeBox" style="display:none;margin-left:90px;" class="card mb-4 p-4 text-center"></div>

<!-- SIDE PANEL -->
<div id="sidePanel">
  <button id="closePanel" onclick="closePanel()" style="position:absolute;top:10px;right:15px;font-size:24px;background:none;border:none;color:white;cursor:pointer;">&times;</button>
  <div id="panelContent"></div>
</div>

<script>
// Users / Login
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(currentUser){checkProfit();openDashboard();}

// Notifications
function showNotif(msg){
  const n=document.getElementById('notif');
  n.innerText=msg;
  n.classList.add('show');
  setTimeout(()=>{n.classList.remove('show');},3000);
}

// Auth
function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){
  let n=document.getElementById('authName').value.trim();
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  if(!n||!e||!p)return alert('Fill all fields');
  if(!validateEmail(e))return alert('Invalid email');
  if(users.find(u=>u.email===e))return alert('Email already registered');
  let u={name:n,email:e,pass:p,plans:[],balance:0,profit:0,nextProfit:Date.now()+86400000,deposits:[],notifications:true};
  users.push(u);
  localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(u));
  openDashboard();
  showNotif("Welcome "+n+"! 🥳");
}

function loginUser(){
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  let u=users.find(x=>x.email===e&&x.pass===p);
  if(!u)return alert('Invalid credentials');
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(u));
  checkProfit();
  openDashboard();
  showNotif("Logged in successfully ✅");
}

function openDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('welcomeBox').style.display='block';
  document.getElementById('welcomeBox').innerHTML=`<h2>Welcome, ${currentUser.name} 💎</h2>
  <p>Wallet Balance: ${currentUser.balance} PKR</p>
  <p>Total Profit: ${currentUser.profit} PKR</p>
  <p>Plans Bought: ${currentUser.plans.length}</p>`;
}

// Plans
let plans=[
  {name:'Basic',amount:200,daily:20,days:25,coming:false},
  {name:'Silver',amount:500,daily:55,days:30,coming:false},
  {name:'Gold',amount:1000,daily:120,days:28,coming:false},
  {name:'Platinum',amount:2500,daily:350,days:35,coming:false},
  {name:'Diamond',amount:5000,daily:700,days:40,coming:true} // Coming Soon
];

function openPanel(type){
  const panel=document.getElementById('sidePanel');
  const content=document.getElementById('panelContent');
  panel.style.display='block';
  panel.classList.add('active');
  content.innerHTML='';
  if(type==='plans'){
    content.innerHTML='<h2>Available Plans</h2>';
    plans.forEach((p,i)=>{
      let disabled=p.coming?'disabled':'';
      content.innerHTML+=`<div class='plan-card'><b>${p.name}</b> - ${p.amount} PKR - ${p.days} Days - Daily: ${p.daily} PKR ${p.coming?'(Coming Soon)':''}
      <br><button class='btn mt-2' onclick='buyPlan(${i})' ${disabled}>Buy Now</button></div>`;
    });
  } else if(type==='wallet'){
    content.innerHTML=`<h2>Wallet</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p>`;
  } else if(type==='activity'){
    content.innerHTML=`<h2>Plan History</h2>${currentUser.plans.map(d=>`<p>${d.name} - Amount: ${d.amount} - Days: ${d.days} - Daily: ${d.daily} - Started: ${new Date(d.start).toLocaleDateString()}</p>`).join('')}`;
  }
}

function closePanel(){document.getElementById('sidePanel').style.display='none';document.getElementById('sidePanel').classList.remove('active');}

function buyPlan(index){
  let p=plans[index];
  if(p.coming){showNotif("Plan Coming Soon ❌");return;}
  currentUser.plans.push({...p,start:Date.now(),dayCount:0,lastProfit:Date.now()});
  showNotif(`${p.name} Plan Purchased ✅`);
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  openDashboard();
}

// Profit Tracker
function checkProfit(){
  if(!currentUser.plans?.length)return;
  let now=Date.now();
  let updated=false;
  currentUser.plans.forEach(p=>{
    if(!p.lastProfit)p.lastProfit=now;
    while(now - p.lastProfit >= 86400000 && p.dayCount < p.days){
      currentUser.balance += p.daily;
      currentUser.profit += p.daily;
      p.dayCount++;
      p.lastProfit += 86400000;
      updated=true;
    }
  });
  if(updated){
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard();
  }
}

setInterval(checkProfit,60000);
</script>

</body>
</html>
