<ROCK><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn — Full Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
body{
  font-family:Arial,sans-serif;
  margin:0;
  padding:20px;
  color:#fff;
  background:linear-gradient(120deg,#0a0227,#24006e,#4e0bb3);
  background-size:400% 400%;
  animation:bgAnim 12s ease infinite;
}
@keyframes bgAnim{
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}
button{padding:8px 12px;margin:4px;cursor:pointer}
#sidebar{display:none;margin-top:20px}
.icon-btn{display:inline-block;width:100px;height:80px;text-align:center;border:1px solid #000;border-radius:8px;padding:8px;margin:4px;cursor:pointer}
#contentSection{margin-top:20px}
.plan-card{border:1px solid #000;padding:10px;margin:10px 0}
</style>
</head>
<body><h2>Rock Earn Dashboard</h2>
<div id="authBox">
<h3>Login / Sign Up / Forgot Password</h3>
<input id="authName" placeholder="Full Name"><br>
<input id="authEmail" placeholder="Email"><br>
<input id="authPass" type="password" placeholder="Password"><br>
<button id="signupBtn">Sign Up</button>
<button id="loginBtn">Login</button>
<button id="forgotBtn">Forgot Password</button>
</div><div id="notif"></div><div id="sidebar">
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><br>Plans</div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><br>Deposit</div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><br>Withdraw</div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><br>Profit</div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><br>History</div>
<div class="icon-btn" onclick="openSection('admin')" id="adminBtn" style="display:none"><i class="fa fa-cog"></i><br>Admin</div>
</div><div id="contentSection"></div><script>
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;

// Ensure default admin
if(!users.find(u => u.email==='admin@rockearn.com')){
  users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
  localStorage.setItem('reUsers',JSON.stringify(users));
}

const plans = [
  {name:'Basic',amount:200,daily:30,days:20},
  {name:'Starter',amount:500,daily:75,days:20},
  {name:'Pro',amount:1000,daily:180,days:20},
  {name:'Advanced',amount:1500,daily:250,days:25},
  {name:'Silver',amount:2000,daily:350,days:30}
];

function showNotif(msg){document.getElementById('notif').innerText=msg;setTimeout(()=>{document.getElementById('notif').innerText=''},3000)}

function signupUser(){
  const n=document.getElementById('authName').value.trim();
  const e=document.getElementById('authEmail').value.trim().toLowerCase();
  const p=document.getElementById('authPass').value;
  if(!n||!e||!p){showNotif('Fill all fields');return;}
  if(users.find(u=>u.email===e)){showNotif('Email exists');return;}
  let u={name:n,email:e,pass:p,role:e==='admin@rockearn.com'?'admin':'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};
  users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  openDashboard();showNotif('Account created & logged in');
}

function loginUser(){
  const e=document.getElementById('authEmail').value.trim().toLowerCase();
  const p=document.getElementById('authPass').value;
  const u=users.find(x=>x.email===e && x.pass===p);
  if(!u){showNotif('Invalid credentials');return;}
  currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  openDashboard();showNotif('Welcome '+currentUser.name.split(' ')[0]);
}

function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}

function openDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('sidebar').style.display='block';
  document.getElementById('contentSection').innerHTML = `<h2 style='text-shadow:0 0 8px #fff;'>🎉 Welcome, <b>${currentUser.name}</b> 🎉</h2>`;
  if(currentUser.role==='admin'){
    document.getElementById('adminBtn').style.display='inline-block';
  }
}
}

function openSection(type){
  const content=document.getElementById('contentSection');
  content.innerHTML='';
  if(type==='plans'){
    plans.forEach(p=>{
      content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="buyPlan('${p.name}',${p.amount},${p.daily},${p.days})">Buy Now</button></div>`;
    });
  } else if(type==='deposit'){content.innerHTML='<h3>Deposit Section</h3>'} 
  else if(type==='withdraw'){content.innerHTML='<h3>Withdraw Section</h3>'}
  else if(type==='profit'){content.innerHTML=`<h3>Profit: ${currentUser.profit} PKR</h3>`}
  else if(type==='history'){content.innerHTML='<h3>History Section</h3>'}
  else if(type==='admin' && currentUser.role==='admin'){
    let html='<h3>Admin Panel</h3>';
    users.forEach(u=>{html+=`<div>${u.name} (${u.email}) - Balance: ${u.balance} PKR</div>`});
    content.innerHTML=html;
  } else {content.innerHTML='<h3>Coming Soon</h3>'}
}

function buyPlan(name,amount,daily,days){
  currentUser.plans.push({name,amount,daily,days,start:Date.now()});
  currentUser.balance-=amount;
  users=users.map(u=>u.email===currentUser.email?currentUser:u);
  localStorage.setItem('reUsers',JSON.stringify(users));
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  showNotif(`Plan ${name} bought successfully`);
}

document.getElementById('signupBtn').onclick=signupUser;
document.getElementById('loginBtn').onclick=loginUser;
</script></body>
</html>
