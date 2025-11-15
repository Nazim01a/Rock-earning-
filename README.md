<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Pro — Ultimate Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{font-family:'Segoe UI',sans-serif;color:white;overflow-x:hidden;margin:0;height:100vh;background:#0a0f1f;position:relative;}
canvas#bgCanvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;}
.sidebar{position:fixed;top:0;left:0;width:80px;height:100vh;background:rgba(0,0,0,0.3);display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:15px;z-index:10;}
.icon-btn{display:flex;align-items:center;justify-content:center;flex-direction:column;padding:12px;font-weight:700;border-radius:15px;cursor:pointer;transition:0.3s;background:linear-gradient(45deg,#4f46e5,#0ff);box-shadow:0 0 10px #0ff,0 0 20px #4f46e5;}
.icon-btn:hover{transform:scale(1.1);box-shadow:0 0 20px #0ff,0 0 40px #4f46e5;}
.card{background:rgba(255,255,255,0.05);border-radius:20px;padding:20px;backdrop-filter:blur(15px);box-shadow:0 0 30px #0ff6;transition:0.3s;}
.btn{background:linear-gradient(45deg,#4f46e5,#0ff);padding:12px 18px;border-radius:12px;font-weight:700;transition:0.3s;box-shadow:0 0 10px #0ff,0 0 20px #4f46e5;cursor:pointer;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #4f46e5;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #4f46e5;transform:scale(1.04);}
#sidePanel{display:none;position:fixed;top:0;left:0;width:0;height:100vh;background:rgba(15,23,42,0.95);padding:40px 20px 20px 20px;transition:0.5s;overflow-y:auto;z-index:9999;}
#sidePanel.active{width:420px;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#4f46e5,#0ff);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
.countdown{font-weight:700;color:#0ff;}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>

<!-- Sidebar -->
<div class="sidebar">
<div style="font-size:28px;margin-bottom:20px;">🚀</div>
<button class="icon-btn" onclick="openPanel('home')"><i class="fa fa-home"></i></button>
<button class="icon-btn" onclick="openPanel('plans')"><i class="fa fa-gem"></i></button>
<button class="icon-btn" onclick="openPanel('wallet')"><i class="fa fa-wallet"></i></button>
<button class="icon-btn" onclick="openPanel('deposit')"><i class="fa fa-money-bill"></i></button>
<button class="icon-btn" onclick="openPanel('withdraw')"><i class="fa fa-hand-holding-dollar"></i></button>
<button class="icon-btn" onclick="openPanel('history')"><i class="fa fa-clock"></i></button>
<button class="icon-btn" onclick="openPanel('support')"><i class="fa fa-headset"></i></button>
<button class="icon-btn" onclick="openPanel('company')"><i class="fa fa-building"></i></button>
<button class="icon-btn" onclick="openPanel('invite')"><i class="fa fa-gift"></i></button>
<button class="icon-btn" onclick="openPanel('share')"><i class="fa fa-share-alt"></i></button>
<button class="icon-btn" onclick="refreshBalance()"><i class="fa fa-sync"></i></button>
</div>

<!-- Auth Box -->
<div id="authBox" class="card" style="margin-left:100px;margin-top:20px;width:300px;">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" />
<input id="authEmail" placeholder="Email" />
<input id="authPass" type="password" placeholder="Password" />
<button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
<button onclick="loginUser()" class="btn w-full mb-2 bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>

<div id="welcomeBox" style="display:none;margin-left:100px;" class="card mb-4 p-4 text-center"></div>

<div id="sidePanel">
<button id="closePanel" onclick="closePanel()" style="position:absolute;top:10px;right:15px;font-size:24px;background:none;border:none;color:white;cursor:pointer;">&times;</button>
<div id="panelContent"></div>
</div>

<button onclick="logoutUser()" class="btn bg-red-600" style="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);width:200px;">Logout</button>

<script>
// BG Particles
const canvas=document.getElementById('bgCanvas');
const ctx=canvas.getContext('2d');
canvas.width=window.innerWidth;canvas.height=window.innerHeight;
let particles=[];
for(let i=0;i<120;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2+1,s:Math.random()*0.5+0.2});}
function animateParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,2*Math.PI);ctx.fillStyle='rgba(0,255,255,0.5)';ctx.fill();p.y-=p.s;if(p.y<0)p.y=canvas.height;});requestAnimationFrame(animateParticles);}
animateParticles();

// Notifications
function showNotif(msg){const n=document.getElementById('notif');n.innerText=msg;n.classList.add('show');setTimeout(()=>{n.classList.remove('show');},3000);}

// Users + Auth
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(currentUser){openDashboard();}
function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){
  let n=document.getElementById('authName').value.trim();
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  if(!n||!e||!p)return showNotif('Fill all fields');
  if(!validateEmail(e))return showNotif('Invalid email');
  if(users.find(u=>u.email===e))return showNotif('Email already registered');
  let u={name:n,email:e,pass:p,plans:[],referrals:[],balance:0,profit:0};
  users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));
  openDashboard();
}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return showNotif('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function openDashboard(){document.getElementById('authBox').style.display='none';document.getElementById('welcomeBox').style.display='block';document.getElementById('welcomeBox').innerHTML=`<h2>Welcome, ${currentUser.name} 💎</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p>`;}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
function refreshBalance(){openDashboard();showNotif('Balance refreshed!');}

// Plans
let plans=[
{name:'Basic',amount:200,daily:30,days:22},
{name:'Pro',amount:500,daily:80,days:25},
{name:'Premium',amount:1000,daily:180,days:30},
{name:'Gold',amount:2500,daily:450,days:28},
{name:'Platinum',amount:5000,daily:950,days:32},
{name:'Diamond',amount:7000,daily:1350,days:27}
];

function openPanel(type){
  const panel=document.getElementById('sidePanel');const content=document.getElementById('panelContent');panel.style.display='block';panel.classList.add('active');content.innerHTML='';
  switch(type){
    case 'plans':content.innerHTML='<h2>Plans</h2>'+plans.map(p=>`<div class="plan-card"><b>${p.name}</b> - ${p.amount} PKR - Daily Profit: ${p.daily} PKR - <span class="countdown">${p.days} Days</span><br><button class="btn mt-2" onclick="buyPlan(${p.amount},'${p.name}',${p.daily},${p.days})">Buy Now</button></div>`).join('');break;
    case 'wallet':content.innerHTML=`<h2>Wallet</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p>`;break;
    case 'deposit':content.innerHTML=`<h2>Deposit</h2><p>JazzCash: 03705519562 <button class='btn' onclick='copyText("03705519562")'>Copy</button></p><p>EasyPaisa: 03379827882 <button class='btn' onclick='copyText("03379827882")'>Copy</button></p><input type='file'><input type='text' placeholder='Transaction ID'><button class='btn mt-2'>Submit</button>`;break;
    case 'withdraw':content.innerHTML=`<h2>Withdraw</h2><input type='number' placeholder='Amount'><select><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type='text' placeholder='Account Number'><button class='btn mt-2'>Withdraw</button>`;break;
    default:content.innerHTML=`<h2>${type}</h2><p>Coming soon...</p>`;
  }
}
function closePanel(){document.getElementById('sidePanel').style.display='none';document.getElementById('sidePanel').classList.remove('active');}

// Buy plan + profit
function buyPlan(amount,name,daily,days){
  currentUser.plans.push({name:name,amount:amount,daily:daily,days:days,ts:Date.now(),profitAdded:false});
  currentUser.balance+=amount;
  localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  showNotif(`${name} plan purchased! Profit will add every 24h`);
}
setInterval(()=>{
  if(currentUser){
    let now=Date.now();
    currentUser.plans.forEach(p=>{
      if(!p.profitAdded && now-p.ts>=24*60*60*1000){currentUser.balance+=p.daily;currentUser.profit+=p.daily;p.profitAdded=true;showNotif(`Daily profit added for ${p.name} plan: ${p.daily} PKR`);}
      p.days--; if(p.days<0)p.days=0;
    });
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard();
  }
},10000);

function copyText(val){navigator.clipboard.writeText(val);showNotif('Copied: '+val);}
</script>
</body>
</html>
