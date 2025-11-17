<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn — Full HD Premium</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600&display=swap');
:root{--neon1:#00ffff;--neon2:#ff00ff;}
body{margin:0;font-family:'Orbitron',sans-serif;color:#fff;overflow-x:hidden;animation:neonBG 20s linear infinite;background:#07080b;}
@keyframes neonBG{0%{background:#07080b}25%{background:#0b0a15}50%{background:#07080b}75%{background:#0a070f}100%{background:#07080b}}
header{position:fixed;left:50%;transform:translateX(-50%);top:8px;font-size:22px;color:var(--neon1);text-shadow:0 0 8px var(--neon1);z-index:100}
.container{max-width:1100px;margin:0 auto;padding:20px}
#notif{position:fixed;right:20px;top:20px;background:var(--neon1);color:#000;padding:10px 14px;border-radius:8px;opacity:0;z-index:9999;transition:all .25s}
#sidebar{position:fixed;left:10px;top:70px;width:62px;background:rgba(0,0,0,.6);border-radius:12px;padding:8px;display:flex;flex-direction:column;gap:8px;z-index:50;display:none;}
#sidebar button{width:46px;height:46px;border-radius:10px;border:1px solid var(--neon1);background:transparent;color:var(--neon1);display:flex;align-items:center;justify-content:center;cursor:pointer;}
#sidebar button:hover{background:var(--neon1);color:#000}
#welcome{position:fixed;left:100px;top:18px;display:flex;gap:10px;z-index:40;display:none;}
.card{background:rgba(0,255,255,0.03);border:1px solid rgba(0,255,255,0.07);padding:10px;border-radius:10px;min-width:130px;text-align:center}
.container-main{margin-left:100px;padding-top:100px;padding-bottom:60px}
.neon-btn{padding:8px 12px;border-radius:8px;border:none;background:linear-gradient(90deg,var(--neon1),var(--neon2));color:#000;font-weight:700;cursor:pointer;transition:0.3s}
.neon-btn:hover{filter:brightness(1.2)}
.neon-input{width:100%;padding:10px;margin:6px 0;border-radius:8px;border:2px solid var(--neon2);background:#111;color:#fff}
.plan-grid{display:flex;flex-wrap:wrap;gap:12px}
.plan-card{background:#0d0d10;padding:12px;border-radius:8px;border:1px solid rgba(0,255,255,0.06);width:220px;transition:0.3s;position:relative}
.plan-card:hover{transform:scale(1.05);box-shadow:0 0 12px var(--neon1)}
.buy-btn{padding:6px 10px;border-radius:6px;border:none;background:var(--neon1);color:#000;cursor:pointer;margin-top:6px}
.offerTag{background:#ff6600;color:#fff;padding:4px 8px;border-radius:8px;font-weight:700;display:inline-block;margin-top:6px}
.countdown{font-size:12px;color:#ff0;margin-top:4px}
.small{font-size:13px;color:#bbb}
.modal-back{position:fixed;inset:0;background:rgba(0,0,0,0.85);display:flex;align-items:center;justify-content:center;z-index:999;display:none}
.modal{background:#0d0d10;padding:16px;border-radius:10px;width:92%;max-width:520px}
.footer{position:fixed;bottom:0;left:0;right:0;background:#050506;padding:10px;text-align:center;border-top:1px solid rgba(0,255,255,0.03);font-size:13px}
.about-content{line-height:1.5;font-size:14px;color:#0ff;margin-top:8px}
.copy-btn{padding:4px 8px;background:var(--neon2);color:#000;border:none;border-radius:6px;cursor:pointer;margin-left:6px}
@media(max-width:800px){.container-main{margin-left:80px}}
</style>
</head>
<body>
<header>Rock Earn Premium</header>
<div id="notif"></div>

<!-- Sidebar -->
<div id="sidebar">
  <button onclick="openSection('plans')" title="Plans"><i class="fas fa-list"></i></button>
  <button onclick="openSection('deposit')" title="Deposit"><i class="fas fa-wallet"></i></button>
  <button onclick="openSection('withdraw')" title="Withdraw"><i class="fas fa-money-bill-wave"></i></button>
  <button onclick="openSection('profit')" title="Profit"><i class="fas fa-chart-line"></i></button>
  <button onclick="openSection('history')" title="History"><i class="fas fa-history"></i></button>
  <button onclick="openSection('settings')" title="Settings"><i class="fas fa-cogs"></i></button>
  <button onclick="openSection('support')" title="Support"><i class="fas fa-headset"></i></button>
  <button id="adminBtn" style="display:none" onclick="openAdmin()" title="Admin"><i class="fas fa-user-shield"></i></button>
  <button onclick="openSection('about')" title="About"><i class="fas fa-info-circle"></i></button>
  <button onclick="openProfile()" title="Profile"><i class="fas fa-user"></i></button>
  <button onclick="logoutUser()" title="Logout"><i class="fas fa-sign-out-alt"></i></button>
</div>

<!-- Header Cards -->
<div id="welcome">
  <div class="card"><div class="small">Balance</div><div id="balValue">0 PKR</div></div>
  <div class="card"><div class="small">Profit</div><div id="profValue">0 PKR</div></div>
</div>

<!-- Main -->
<div class="container-main container" id="mainArea">
  <div id="authBox" style="max-width:460px;margin:30px auto">
    <div style="text-align:center;margin-bottom:8px" class="small">Welcome — Signup or Login</div>
    <input id="authName" class="neon-input" placeholder="Your Name (Signup only)">
    <input id="authEmail" class="neon-input" placeholder="Email">
    <input id="authPass" class="neon-input" placeholder="Password" type="password">
    <div style="display:flex;gap:8px;justify-content:center">
      <button class="neon-btn" onclick="signupUser()">Sign Up</button>
      <button class="neon-btn" onclick="loginUser()">Login</button>
    </div>
  </div>
  <div id="mainContent"></div>
</div>

<!-- Plans container -->
<div id="plansContainer" class="plan-grid" style="margin:20px;"></div>

<!-- Deposit Modal -->
<div id="depositModal" class="modal-back">
  <div class="modal">
    <h3>Make Deposit</h3>
    <input id="depositAmount" class="neon-input" type="number" placeholder="Amount (PKR)">
    <select id="depositMethod" class="neon-input" onchange="updateDepositInfo()">
      <option>JazzCash</option>
      <option>EasyPaisa</option>
      <option>Bank</option>
      <option>Binance</option>
    </select>
    <div id="depositInfo" class="small about-content" style="margin:6px 0"></div>
    <div id="copyDiv" style="display:flex;align-items:center;"></div>
    <input id="transId" class="neon-input" placeholder="Transaction ID / Upload Proof">
    <div style="display:flex;gap:8px;justify-content:center;margin-top:8px">
      <button class="neon-btn" onclick="confirmDeposit()">Submit & Auto-Add</button>
      <button class="neon-btn" onclick="closeDeposit()">Close</button>
    </div>
  </div>
</div>

<!-- Withdraw Modal -->
<div id="withdrawModal" class="modal-back">
  <div class="modal">
    <h3>Request Withdrawal</h3>
    <input id="withdrawAmount" class="neon-input" type="number" placeholder="Amount (PKR)">
    <select id="withdrawMethod" class="neon-input" onchange="updateWithdrawInfo()">
      <option>JazzCash</option>
      <option>EasyPaisa</option>
      <option>Bank</option>
      <option>Binance</option>
    </select>
    <input id="withdrawUsername" class="neon-input" placeholder="Payee Username">
    <input id="withdrawAcc" class="neon-input" placeholder="Account / Mobile Number">
    <div id="withdrawInfo" class="small" style="margin:6px 0"></div>
    <div style="display:flex;gap:8px;justify-content:center;margin-top:8px">
      <button class="neon-btn" onclick="confirmWithdraw()">Request Withdraw</button>
      <button class="neon-btn" onclick="closeWithdraw()">Close</button>
    </div>
  </div>
</div>

<!-- About Modal -->
<div id="aboutModal" class="modal-back">
  <div class="modal">
    <h3>About Rock Earn Premium</h3>
    <div class="about-content">
      Rock Earn Premium — <strong>Your Trusted Earning Platform</strong> 💎<br>
      <strong>Official Email:</strong> rock.earn92@gmail.com ✉️<br>
      <strong>Customer Support:</strong> 24/7 available<br>
      <strong>Deposit Numbers:</strong><br>
      &nbsp;&nbsp;- JazzCash: 03705591562 📲<br>
      &nbsp;&nbsp;- EasyPaisa: 03379827882 📲<br>
      <strong>Safe & Secure Platform</strong> 🔒<br>
      <strong>Start Plans from:</strong> 200 PKR 💰<br>
      <strong>Fully Premium Animated Dashboard</strong> ✨
    </div>
    <div style="display:flex;gap:8px;justify-content:center;margin-top:12px">
      <button class="neon-btn" onclick="closeAbout()">Close</button>
    </div>
  </div>
</div>

<div class="footer">Rock Earn Premium © 2025</div>

<script>
// ======== Data ========
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUserEmail = localStorage.getItem('reCurrentEmail') || null;
let depositRequests = JSON.parse(localStorage.getItem('reDeposit')) || [];
let withdrawRequests = JSON.parse(localStorage.getItem('reWithdraw')) || [];
let selectedPlan = null;
if(!users.find(u=>u.email==='admin@rockearn.com')){
  users.push({name:'Admin',email:'admin@rockearn.com',pass:'admin123',role:'admin',balance:0,profit:0});
  localStorage.setItem('reUsers',JSON.stringify(users));
}
const paymentNumbers = {JazzCash:'03705591562',EasyPaisa:'03379827882'};

// ======== UI ========
function showNotif(msg,time=2500){const n=document.getElementById('notif');n.innerText=msg;n.style.opacity='1';setTimeout(()=>n.style.opacity='0',time);}
function getCurrentUser(){return users.find(u=>u.email===currentUserEmail)||null;}
function setLoggedInUI(){
  const u=getCurrentUser();
  if(u){
    document.getElementById('sidebar').style.display='flex';
    document.getElementById('welcome').style.display='flex';
    document.getElementById('authBox').style.display='none';
    document.getElementById('adminBtn').style.display = u.role==='admin'?'block':'none';
    document.getElementById('balValue').innerText = (u.balance||0)+' PKR';
    document.getElementById('profValue').innerText = (u.profit||0)+' PKR';
    loadPlans();
  }else{
    document.getElementById('sidebar').style.display='none';
    document.getElementById('welcome').style.display='none';
    document.getElementById('authBox').style.display='block';
  }
}

// ======== Auth ========
function signupUser(){
  const name=document.getElementById('authName').value.trim();
  const email=document.getElementById('authEmail').value.trim().toLowerCase();
  const pass=document.getElementById('authPass').value;
  if(!name||!email||!pass){showNotif('Fill all fields');return;}
  if(users.find(u=>u.email===email)){showNotif('Email exists');return;}
  users.push({name,email,pass,role:'user',balance:0,profit:0});
  localStorage.setItem('reUsers',JSON.stringify(users));
  currentUserEmail=email;
  localStorage.setItem('reCurrentEmail',currentUserEmail);
  showNotif('Signed up & logged in');
  setLoggedInUI();
}
function loginUser(){
  const email=document.getElementById('authEmail').value.trim().toLowerCase();
  const pass=document.getElementById('authPass').value;
  const u=users.find(x=>x.email===email && x.pass===pass);
  if(!u){showNotif('Invalid credentials');return;}
  currentUserEmail=u.email;
  localStorage.setItem('reCurrentEmail',currentUserEmail);
  showNotif('Welcome '+u.name.split(' ')[0]);
  setLoggedInUI();
}
function logoutUser(){currentUserEmail=null;localStorage.removeItem('reCurrentEmail');setLoggedInUI();showNotif('Logged out');}

// ======== Plans ========
let specialOffers = {1:{profit:1200,end:Date.now()+24*60*60*1000},2:{profit:1300,end:Date.now()+24*60*60*1000},3:{profit:1400,end:Date.now()+24*60*60*1000},4:{profit:1500,end:Date.now()+24*60*60*1000},5:{profit:1600,end:Date.now()+24*60*60*1000}};
function calculateBonus(amount,index){if(specialOffers[index]&&Date.now()<specialOffers[index].end)return specialOffers[index].profit;if(amount===200)return 550;return Math.round(amount*2.75);}
function formatTime(ms){let s=Math.floor(ms/1000);let h=Math.floor(s/3600);s%=3600;let m=Math.floor(s/60);s%=60;return `${h}h ${m}m ${s}s`;}
function loadPlans(){
  const planBox=document.getElementById('plansContainer');planBox.innerHTML='';
  for(let i=1;i<=15;i++){
    let amount=i===1?200:i<=5?200*i*5:5000*i;let days=20+i*10;let profit=calculateBonus(amount,i);
    let tag='';let cd='';
    if(specialOffers[i] && Date.now()<specialOffers[i].end){tag='<span class="offerTag">🔥 Special Offer</span>';cd='<div class="countdown" id="cd'+i+'"></div>';}
    planBox.innerHTML+=`<div class="plan-card"><h3>Plan ${i}</h3><p>Amount: <strong>${amount} PKR</strong></p><p>Days: <strong>${days}</strong></p><p>Profit: <strong>${profit} PKR</strong></p>${tag}${cd}<button class="buy-btn" onclick="openDepositFromPlan(${amount})">Buy Now</button></div>`;
    if(cd){updateCountdown(i);}
  }
}
function updateCountdown(i){
  const cd=document.getElementById('cd'+i);if(!cd)return;
  let interval=setInterval(()=>{
    let remain=specialOffers[i].end-Date.now();
    if(remain<=0){cd.innerText='Offer expired';clearInterval(interval);loadPlans();return;}
    cd.innerText='Offer ends in: '+formatTime(remain);
  },1000);
}

// ======== Buy → Deposit ========
function openDepositFromPlan(amount){
  document.getElementById('depositAmount').value=amount;
  document.getElementById('depositModal').style.display='flex';
  updateDepositInfo();
}

// ======== Deposit Info + Copy ========
function updateDepositInfo(){
    const method=document.getElementById('depositMethod').value;
    const infoDiv=document.getElementById('depositInfo');
    const copyDiv=document.getElementById('copyDiv');
    copyDiv.innerHTML='';
    if(method==='JazzCash' || method==='EasyPaisa'){
        infoDiv.innerText=`Send your amount to ${method}: ${paymentNumbers[method]}`;
        let btn=document.createElement('button');
        btn.innerText='Copy Number';
        btn.className='copy-btn';
        btn.onclick=()=>{navigator.clipboard.writeText(paymentNumbers[method]);showNotif('Number copied');}
        copyDiv.appendChild(btn);
    } else {
        infoDiv.innerText = method==='Bank' || method==='Binance' ? method + " coming soon" : '';
    }
}

function confirmDeposit(){
    const amount = parseFloat(document.getElementById('depositAmount').value);
    const method = document.getElementById('depositMethod').value;
    const transId = document.getElementById('transId').value.trim();
    if(!amount || !method || !transId){showNotif('Fill all deposit fields');return;}
    depositRequests.push({user:currentUserEmail,amount,method,transId,date:new Date().toLocaleString()});
    localStorage.setItem('reDeposit',JSON.stringify(depositRequests));
    let user = getCurrentUser();
    user.balance += amount;
    localStorage.setItem('reUsers',JSON.stringify(users));
    document.getElementById('balValue').innerText = user.balance+' PKR';
    showNotif('Deposit added successfully');
    closeDeposit();
}

function closeDeposit(){document.getElementById('depositModal').style.display='none';}

// ======== Withdraw Info ========
function updateWithdrawInfo(){
    const method = document.getElementById('withdrawMethod').value;
    const infoDiv = document.getElementById('withdrawInfo');
    if(method==='Bank' || method==='Binance'){
        infoDiv.innerText = method + " coming soon";
    } else infoDiv.innerText='';
    const user = getCurrentUser();
    document.getElementById('withdrawUsername').value = user ? user.name : '';
    document.getElementById('withdrawAcc').value = method==='JazzCash'?paymentNumbers.JazzCash:(method==='EasyPaisa'?paymentNumbers.EasyPaisa:'');
}

function confirmWithdraw(){
    const amount = parseFloat(document.getElementById('withdrawAmount').value);
    const method = document.getElementById('withdrawMethod').value;
    const username = document.getElementById('withdrawUsername').value.trim();
    const acc = document.getElementById('withdrawAcc').value.trim();
    if(!amount || !method || !username || !acc){showNotif('Fill all withdraw fields');return;}
    let user = getCurrentUser();
    if(amount>user.balance){showNotif('Insufficient balance');return;}
    withdrawRequests.push({user:currentUserEmail,amount,method,username,acc,date:new Date().toLocaleString()});
    localStorage.setItem('reWithdraw',JSON.stringify(withdrawRequests));
    user.balance -= amount;
    localStorage.setItem('reUsers',JSON.stringify(users));
    document.getElementById('balValue').innerText = user.balance+' PKR';
    showNotif('Withdraw request submitted');
    closeWithdraw();
}

function closeWithdraw(){document.getElementById('withdrawModal').style.display='none';}

// ======== About ========
function openAbout(){document.getElementById('aboutModal').style.display='flex';}
function closeAbout(){document.getElementById('aboutModal').style.display='none';}

// ======== Sections ========
function openSection(section){showNotif(section+' section opened');}

// ======== Profile ========
function openProfile(){showNotif('Profile opened');}

// ======== Admin ========
function openAdmin(){showNotif('Admin panel opened');}

// ======== Init ========
setLoggedInUI();
</script>
</body>
</html>
