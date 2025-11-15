<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />
<style>
body{font-family:'Segoe UI',sans-serif;color:white;margin:0;overflow-x:hidden;height:100vh;background:#0f0f20;}
canvas#bgCanvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;}

/* Sidebar */
.sidebar{position:fixed;top:0;left:0;width:120px;height:100vh;background:rgba(0,0,0,0.3);display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:15px;z-index:10;opacity:0;transition:1s;}
.sidebar.active{opacity:1;}
.icon-btn{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;transition:0.3s;background:linear-gradient(45deg,#00f,#0ff);box-shadow:0 0 10px #0ff,0 0 20px #00f;}
.icon-btn:hover{transform:scale(1.1);box-shadow:0 0 20px #0ff,0 0 40px #00f;}
.icon-name{margin-top:6px;font-size:12px;color:#0ff;text-shadow:0 0 5px #0ff;}

/* Cards / Buttons */
.card{background:rgba(255,255,255,0.05);border-radius:20px;padding:20px;backdrop-filter:blur(15px);box-shadow:0 0 30px #0ff6;transition:0.3s;}
.btn{background:linear-gradient(45deg,#00f,#0ff);padding:12px 18px;border-radius:12px;font-weight:700;transition:0.3s;box-shadow:0 0 10px #0ff,0 0 20px #00f;cursor:pointer;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #0ff;transform:scale(1.04);}

/* Content Section */
#contentSection{position:fixed;top:20px;left:140px;right:20px;bottom:20px;background:rgba(0,0,0,0.8);backdrop-filter:blur(15px);border-radius:20px;padding:20px;overflow-y:auto;display:none;z-index:999;}
#backBtn{position:absolute;top:10px;left:20px;background:#ff0044;padding:8px 12px;border:none;border-radius:10px;font-weight:700;cursor:pointer;transition:0.3s;}
#backBtn:hover{transform:scale(1.05);background:#ff3366;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#00f,#0ff);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>

<!-- Sidebar -->
<div class="sidebar" id="sidebar">
<div style="font-size:28px;margin-bottom:10px;">🚀</div>
<div class="icon-btn" onclick="openSection('home')"><i class="fa fa-home"></i><span class="icon-name">Dashboard</span></div>
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><span class="icon-name">Plans</span></div>
<div class="icon-btn" onclick="openSection('wallet')"><i class="fa fa-wallet"></i><span class="icon-name">Wallet</span></div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><span class="icon-name">Deposit</span></div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><span class="icon-name">Withdraw</span></div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><span class="icon-name">History</span></div>
<div class="icon-btn" onclick="openSection('support')"><i class="fa fa-headset"></i><span class="icon-name">Support</span></div>
<div class="icon-btn" onclick="openSection('company')"><i class="fa fa-building"></i><span class="icon-name">Company</span></div>
<div class="icon-btn" onclick="openSection('invite')"><i class="fa fa-gift"></i><span class="icon-name">Referral</span></div>
<div class="icon-btn" onclick="openSection('share')"><i class="fa fa-share-alt"></i><span class="icon-name">Share</span></div>
<div class="icon-btn" onclick="refreshBalance()"><i class="fa fa-sync"></i><span class="icon-name">Refresh</span></div>
</div>

<!-- Auth Box -->
<div id="authBox" class="card" style="margin-left:140px;margin-top:20px;width:300px;">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" />
<input id="authEmail" placeholder="Email" />
<input id="authPass" type="password" placeholder="Password" />
<button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
<button onclick="loginUser()" class="btn w-full mb-2 bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>

<div id="welcomeBox" style="display:none;margin-left:140px;" class="card mb-4 p-4 text-center"></div>

<!-- Content Section -->
<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<button onclick="logoutUser()" class="btn bg-red-600" style="position:fixed;bottom:20px;left:50%;transform:translateX(-50%);width:200px;">Logout</button>

<script>
// BG Animation White-Blue
const canvas=document.getElementById('bgCanvas');
const ctx=canvas.getContext('2d');
canvas.width=window.innerWidth;canvas.height=window.innerHeight;
let particles=[];
for(let i=0;i<150;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2+1,s:Math.random()*0.5+0.5,color:`rgba(0,255,255,${Math.random()})`});}
function animateParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,2*Math.PI);ctx.fillStyle=p.color;ctx.fill();p.y-=p.s;if(p.y<0)p.y=canvas.height;});requestAnimationFrame(animateParticles);}
animateParticles();

// Notifications
function showNotif(msg){const n=document.getElementById('notif');n.innerText=msg;n.classList.add('show');setTimeout(()=>{n.classList.remove('show');},3000);}

// Users + Auth
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;

// ✅ Fix: page load par check karo agar currentUser exist karta hai to dashboard open karo
window.addEventListener('load', () => {
  if (currentUser) {
    openDashboard();
    document.getElementById('sidebar').classList.add('active');
  }
});

function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return showNotif('Fill all fields');if(!validateEmail(e))return showNotif('Invalid email');if(users.find(u=>u.email===e))return showNotif('Email already registered');let u={name:n,email:e,pass:p,plans:[],referrals:[],balance:0,profit:0};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return showNotif('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function openDashboard(){document.getElementById('authBox').style.display='none';document.getElementById('sidebar').classList.add('active');document.getElementById('welcomeBox').style.display='block';document.getElementById('welcomeBox').innerHTML=`<h2>Welcome, ${currentUser.name} 💎</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p>`;}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
function refreshBalance(){openDashboard();showNotif('Balance refreshed!');}

// Plans
let plans=[
{name:'Basic',amount:200,daily:30,days:22},
{name:'Pro',amount:500,daily:80,days:25},
{name:'Premium',amount:1000,daily:180,days:30},
{name:'Gold',amount:2500,daily:450,days:28},
{name:'Platinum',amount:5000,daily:950,days:32},
{name:'Diamond',amount:7000,daily:1350,days:27},
{name:'Elite X',amount:10000,daily:2200,days:35,coming:true},
{name:'Mega Booster',amount:15000,daily:3500,days:40,coming:true},
{name:'Ultra Pro',amount:20000,daily:4800,days:45,coming:true},
{name:'Crypto Miner',amount:30000,daily:6500,days:50,coming:true},
{name:'Titanium Plan',amount:40000,daily:8500,days:55,coming:true}
];

// Sections
function openSection(type){
  const sec=document.getElementById('contentSection');const content=document.getElementById('sectionContent');sec.style.display='block';content.innerHTML='';
  switch(type){
    case 'home':content.innerHTML='<h2>Dashboard Home</h2><p>Quick overview of your balance, plans, and profits.</p>';break;
    case 'plans':
      content.innerHTML='<h2>Plans</h2>'+plans.map(p=>{
        if(p.coming){return `<div class="plan-card"><b>${p.name}</b> - ${p.amount} PKR - Daily: ${p.daily} PKR - <span class="countdown">${p.days} Days</span><br><button class="btn mt-2" disabled>Coming Soon</button></div>`;}
        return `<div class="plan-card"><b>${p.name}</b> - ${p.amount} PKR - Daily: ${p.daily} PKR - <span class="countdown">${p.days} Days</span><br><button class="btn mt-2" onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days})">Buy Now</button></div>`;
      }).join('');break;
    case 'wallet':content.innerHTML=`<h2>Wallet</h2><p>Balance: ${currentUser.balance} PKR</p><p>Total Profit: ${currentUser.profit} PKR</p>`;break;
    case 'deposit':openDeposit();break;
    case 'withdraw':content.innerHTML=`<h2>Withdraw</h2><input type='number' placeholder='Amount'><select><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type='text' placeholder='Account Number'><button class='btn mt-2'>Withdraw</button>`;break;
    case 'history':content.innerHTML=`<h2>Activity History</h2><p>• Deposit: 500 PKR</p><p>• Withdraw: 300 PKR</p><p>• Plan Bought: Basic Plan</p>`;break;
    case 'support':content.innerHTML=`<h2>Support</h2><p>Contact us at support@rockearnpro.com</p>`;break;
    case 'company':content.innerHTML=`<h2>Company</h2><p>Since: 2018</p><p>Industry: Crypto & FinTech</p><p>Partner: Binance</p><p>Address: 1234 Crypto Avenue, SF, USA</p>`;break;
    case 'invite':content.innerHTML=`<h2>Invite Bonus</h2><p>Share your referral link and earn bonuses!</p>`;break;
    case 'share':content.innerHTML=`<h2>Share Link</h2><input type='text' value='https://rockearnpro.com?ref=${currentUser.email}' readonly><button class='btn mt-2' onclick='copyText("https://rockearnpro.com?ref=${currentUser.email}")'>Copy Link</button>`;break;
    default:content.innerHTML=`<h2>${type}</h2><p>Coming Soon...</p>`;
  }
}
function closeSection(){document.getElementById('contentSection').style.display='none';}

// Deposit Auto-fill
function openDeposit(amount=0,name='',daily=0,days=0){
  const sec=document.getElementById('contentSection');const content=document.getElementById('sectionContent');sec.style.display='block';
  content.innerHTML=`<h2>Deposit for ${name}</h2>
  <p>Amount: <b>${amount} PKR</b></p>
  <p>JazzCash: 03705519562 <button class='btn' onclick='copyText("03705519562")'>Copy</button></p>
  <p>EasyPaisa: 03379827882 <button class='btn' onclick='copyText("03379827882")'>Copy</button></p>
  <input type='file' id='proofFile'><input type='text' id='txnId' placeholder='Transaction ID'>
  <button class='btn mt-2' onclick='confirmDeposit(${amount},"${name}",${daily},${days})'>Submit Deposit</button>`;
}

// Confirm Deposit → Plan activate
function confirmDeposit(amount,name,daily,days){
  let file=document.getElementById('proofFile').files[0];let txn=document.getElementById('txnId').value.trim();
  if(!file || !txn){showNotif('Please upload proof & enter transaction ID'); return;}
  currentUser.plans.push({name:name,amount:amount,daily:daily,days:days,ts:Date.now(),profitAdded:false});
  currentUser.balance+=amount;localStorage.setItem('reCurrent',JSON.stringify(currentUser));
  showNotif(`${name} plan activated! Profit will add every 24h`);
  openSection('wallet');
}

// Daily Profit Auto Add
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
