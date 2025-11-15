<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn — Premium Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body{
  margin:0;
  font-family:'Segoe UI',sans-serif;
  background:#081423;
  color:#fff;
  overflow-x:hidden;
  height:100vh;
  display:flex;
  flex-direction:column;
  animation: bgAnim 30s linear infinite;
}
@keyframes bgAnim{
  0%{background:linear-gradient(135deg,#081423,#0f1b33);}
  50%{background:linear-gradient(135deg,#0f1b33,#081423);}
  100%{background:linear-gradient(135deg,#081423,#0f1b33);}
}
#rockEarnTitle{
  text-align:center;
  font-size:2.2em;
  font-weight:800;
  margin:20px 0;
  background: linear-gradient(90deg,#00f7ef,#ff4766,#ffd700,#00f7ef);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: textAnim 5s ease-in-out infinite;
}
@keyframes textAnim{
  0%{background-position:0%}
  50%{background-position:100%}
  100%{background-position:0%}
}
/* Notification */
#notif { position:fixed; top:60px; right:-400px; background:#00f7ef; color:#001; padding:12px 20px; border-radius:12px; font-weight:700; transition: right .45s; z-index:60; }
/* Auth Box */
#authBox { position:fixed; left:50%; top:150px; transform:translateX(-50%); width:320px; background:rgba(0,0,0,0.6); padding:20px; border-radius:14px; }
#authBox input { width:100%; margin-bottom:10px; padding:10px; border-radius:10px; border:none; background:#0f1320; color:#dff7fb; }
/* Sidebar */
#sidebar { position:fixed; left:0; top:120px; bottom:0; width:80px; background:rgba(10,10,12,0.9); display:flex; flex-direction:column; align-items:center; padding:20px 0; gap:14px; z-index:50; }
.icon-btn { width:64px; height:64px; border-radius:14px; background:linear-gradient(180deg,#0b0b10,#0f1220); display:flex; flex-direction:column; align-items:center; justify-content:center; color:#bcdbe3; font-size:14px; transition:.2s; cursor:pointer;}
.icon-btn:hover { transform:translateY(-6px); box-shadow:0 12px 30px rgba(0,247,239,0.2); }
/* Content Section */
#contentSection { position:fixed; left:100px; top:120px; right:20px; bottom:20px; background:rgba(0,0,0,0.5); border-radius:14px; padding:20px; overflow:auto; display:none; }
/* Back & Refresh Buttons */
#backBtn,#refreshBtn{position:fixed; top:12px; background:#ff4766; color:white; padding:8px 12px; border-radius:8px; cursor:pointer; z-index:55;}
#backBtn{left:100px; display:none;}
#refreshBtn{left:180px; display:none;}
/* Logout Button */
#logoutBtn { position:fixed; bottom:20px; left:50%; transform:translateX(-50%); background:#ff4766; color:white; padding:10px 14px; border-radius:10px; display:none; cursor:pointer; }
/* Welcome Box */
#welcomeBox { position:fixed; left:100px; top:60px; right:20px; padding:18px; border-radius:14px; background:rgba(255,255,255,0.02); backdrop-filter:blur(8px); box-shadow:0 4px 30px rgba(0,0,0,0.5); display:none; }
.stats { display:flex; gap:18px; margin-top:12px; }
.stat-card { background:rgba(0,247,239,0.03); padding:12px 16px; border-radius:10px; min-width:120px; }
.stat-label { font-size:11px; color:#00f7ef; margin:0 0 6px; }
.stat-value { font-size:16px; font-weight:700; margin:0; }
/* Plan Card */
.plan-card { border:1px solid rgba(0,247,239,0.1); border-radius:10px; padding:12px; margin-bottom:12px; background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0)); transition:.3s;}
.plan-card button{margin-top:6px; padding:6px 10px; border-radius:8px; background:linear-gradient(90deg,#00f7ef,#ff4766); border:none; cursor:pointer; color:#001; font-weight:700; transition:.3s;}
.plan-card button:hover{transform:scale(1.05);}
</style>
</head>
<body>
<div id="rockEarnTitle">💎 Rock Earn 💎</div>
<div id="notif"></div>

<!-- Auth Box -->
<div id="authBox">
<h3 style="text-align:center">Login / Sign Up</h3>
<input id="authName" placeholder="Full Name">
<input id="authEmail" placeholder="Email">
<input id="authPass" type="password" placeholder="Password">
<div style="display:flex; gap:8px;">
<button id="signupBtn"><i class="fa fa-user-plus"></i> Sign Up</button>
<button id="loginBtn" style="background:#00b37e;color:#001;"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>
<p style="margin-top:10px; font-size:13px; color:#9fcfda;">Default admin: <strong>admin@rockearn.com</strong></p>
</div>

<!-- Sidebar -->
<div id="sidebar">
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><br>Plans</div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><br>Deposit</div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><br>Withdraw</div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><br>Profit</div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><br>History</div>
<div class="icon-btn" onclick="openSection('admin')" id="adminBtn" style="display:none"><i class="fa fa-cog"></i><br>Admin</div>
</div>

<!-- Buttons -->
<button id="backBtn" onclick="closeSection()">← Back</button>
<button id="refreshBtn" onclick="refreshDashboard()">⟳ Refresh</button>
<button id="logoutBtn" onclick="logoutUser()">Logout</button>

<!-- Welcome Box -->
<div id="welcomeBox">
<h2 id="welcomeTxt">🎉 Welcome, <strong id="userName"></strong> 💎</h2>
<div class="stats">
<div class="stat-card"><div class="stat-label">Balance</div><div class="stat-value" id="balValue">0 PKR</div></div>
<div class="stat-card"><div class="stat-label">Total Profit</div><div class="stat-value" id="profValue">0 PKR</div></div>
</div>
</div>

<!-- Content Section -->
<div id="contentSection">
</div><script>
// ----------------------
// Users & Storage
// ----------------------
let users = JSON.parse(localStorage.getItem('reUsers')) || [];
let currentUser = JSON.parse(localStorage.getItem('reCurrent')) || null;

// Default admin
if(!users.find(u=>u.email==='admin@rockearn.com')){
    users.push({name:'Administrator',email:'admin@rockearn.com',pass:'admin123',role:'admin',plans:[],deposits:[],withdrawals:[],balance:0,profit:0});
    localStorage.setItem('reUsers',JSON.stringify(users));
}

// Plans
const plans = [
{name:'Basic',amount:200,daily:30,days:20},
{name:'Starter',amount:500,daily:75,days:20},
{name:'Pro',amount:1000,daily:180,days:20},
{name:'Advanced',amount:1500,daily:250,days:25},
{name:'Silver',amount:2000,daily:350,days:30},
{name:'Gold',amount:3000,daily:550,days:30},
{name:'Platinum',amount:5000,daily:950,days:40},
{name:'Diamond',amount:7000,daily:1350,days:50},
{name:'VIP',amount:10000,daily:2000,days:60},
{name:'Elite',amount:12000,daily:2400,days:60},
{name:'Master',amount:15000,daily:3000,days:70},
{name:'Ultimate',amount:18000,daily:3500,days:75},
{name:'Legend',amount:20000,daily:4000,days:80},
{name:'Supreme',amount:25000,daily:5000,days:90},
{name:'Titan',amount:30000,daily:6000,days:100}
];

// ----------------------
// Notifications
// ----------------------
function showNotif(msg){ const el=document.getElementById('notif'); el.innerText=msg; el.classList.add('show'); setTimeout(()=>el.classList.remove('show'),3000); }

// ----------------------
// Auth Functions
// ----------------------
function signupUser(){
    const n=document.getElementById('authName').value.trim();
    const e=document.getElementById('authEmail').value.trim().toLowerCase();
    const p=document.getElementById('authPass').value;
    if(!n||!e||!p){showNotif('Fill all fields'); return;}
    if(users.find(u=>u.email===e)){showNotif('Email exists'); return;}
    let u={name:n,email:e,pass:p,role:e==='admin@rockearn.com'?'admin':'user',plans:[],deposits:[],withdrawals:[],balance:0,profit:0};
    users.push(u); localStorage.setItem('reUsers',JSON.stringify(users));
    currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard(); showNotif('Account created & logged in');
}

function loginUser(){
    const e=document.getElementById('authEmail').value.trim().toLowerCase();
    const p=document.getElementById('authPass').value;
    const u=users.find(x=>x.email===e && x.pass===p);
    if(!u){showNotif('Invalid credentials'); return;}
    currentUser=u; localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard(); showNotif('Welcome '+currentUser.name.split(' ')[0]);
}

function logoutUser(){
    currentUser=null; localStorage.removeItem('reCurrent'); location.reload();
}

// ----------------------
// Dashboard & Sections
// ----------------------
function openDashboard(){
    document.getElementById('authBox').style.display='none';
    document.getElementById('sidebar').style.display='flex';
    document.getElementById('welcomeBox').style.display='block';
    document.getElementById('logoutBtn').style.display='block';
    document.getElementById('refreshBtn').style.display='inline-block';
    document.getElementById('userName').innerText=currentUser.name;
    document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
    document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
    if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';}
    document.getElementById('backBtn').style.display='none';
}

function refreshDashboard(){
    if(currentUser){openDashboard(); if(document.getElementById('contentSection').style.display==='block'){openSection(currentUser.lastSection||'plans');}}
    showNotif('Dashboard refreshed');
}

function openSection(type){
    currentUser.lastSection=type;
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    const content=document.getElementById('sectionContent');
    document.getElementById('backBtn').style.display='inline-block';
    content.style.display='block';
    content.innerHTML='';
    
    if(type==='plans'){
        plans.forEach((p,idx)=>{
            content.innerHTML+=`<div class='plan-card'><b>${p.name}</b><br>Amount: ${p.amount} PKR<br>Daily: ${p.daily} PKR<br>Days: ${p.days}<br><button onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days},${idx})">Buy Now</button></div>`;
        });
    } else if(type==='deposit'){
        content.innerHTML='<h3>Deposit Section</h3>';
    } else if(type==='withdraw'){
        openWithdraw();
    } else if(type==='profit'){
        content.innerHTML=`<h3>Total Profit: ${currentUser.profit} PKR</h3>`;
    } else if(type==='history'){
        let html='<h3>History</h3>';
        (currentUser.deposits||[]).forEach(d=>{html+=`<p>Deposit: ${d.plan} - ${d.amount} PKR - ${d.status}</p>`});
        (currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.method} - ${w.status}</p>`});
        content.innerHTML=html;
    } else if(type==='admin' && currentUser.role==='admin'){
        let html='<h3>Admin Panel</h3>';
        users.forEach((u,ui)=>{ 
            html+=`<div style="border-bottom:1px solid #00f7ef; padding:6px;"><b>${u.name}</b> (${u.email}) - Balance: ${u.balance} PKR
            <br>Deposits:`;
            (u.deposits||[]).forEach((d,di)=>{ 
                html+=`<div style="margin-left:12px;">${d.plan} ${d.amount} PKR - ${d.status} 
                ${d.status==='pending'?`<button onclick="adminApprove('${u.email}',${di})">Approve</button><button onclick="adminReject('${u.email}',${di})">Reject</button>`:''}</div>`; 
            });
            html+=`<br>Withdrawals:`;
            (u.withdrawals||[]).forEach((w,wi)=>{
                html+=`<div style="margin-left:12px;">${w.amount} PKR - ${w.method} - ${w.status} 
                ${w.status==='pending'?`<button onclick="adminApproveWithdraw('${u.email}',${wi})">Approve</button><button onclick="adminRejectWithdraw('${u.email}',${wi})">Reject</button>`:''}</div>`;
            });
            html+='</div>';
        });
        content.innerHTML=html;
    } else {content.innerHTML='<h3>Coming Soon</h3>';}
}

function closeSection(){document.getElementById('contentSection').style.display='none';document.getElementById('backBtn').style.display='none';}

// ----------------------
// Deposit / Buy Plan
// ----------------------
function openDeposit(amount=0,name='',daily=0,days=0,planIndex=0){
    const content=document.getElementById('sectionContent');
    content.innerHTML=`<h3>Deposit — ${name}</h3>
<p>Amount: <strong>${amount} PKR</strong></p>
<p>JazzCash: <strong>03705519562</strong> <button onclick="copyText('03705519562')">Copy</button></p>
<p>EasyPaisa: <strong>03379827882</strong> <button onclick="copyText('03379827882')">Copy</button></p>
<div style="margin-top:10px">
<input id="depositTxn" placeholder="Transaction ID">
<input id="depositProof" type="file"><br>
<button onclick="confirmDeposit(${amount},'${name}',${daily},${days},${planIndex})">Submit Deposit (Pending Admin)</button>
</div>`;
}

// Copy text helper
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}

// ----------------------
// Event Listeners
// ----------------------
document.getElementById('signupBtn').onclick=signupUser;
document.getElementById('loginBtn').onclick=loginUser;

</script><script>
// ----------------------
// Withdraw Section
// ----------------------
function openWithdraw(){
    const content=document.getElementById('sectionContent');
    content.innerHTML=`<h3>Withdraw Funds</h3>
<input id="withdrawAmount" placeholder="Amount" type="number">
<input id="withdrawName" placeholder="Account Holder Name">
<input id="withdrawAcc" placeholder="Account Number">
<select id="withdrawMethod">
<option value="JazzCash">JazzCash</option>
<option value="EasyPaisa">EasyPaisa</option>
<option value="Bank">Bank</option>
<option value="Binance" disabled>Binance (Coming Soon)</option>
</select><br><br>
<button onclick="confirmWithdraw()">Submit Withdrawal</button>`;
}

// Confirm withdrawal request
function confirmWithdraw(){
    const amt=parseFloat(document.getElementById('withdrawAmount').value);
    const name=document.getElementById('withdrawName').value.trim();
    const acc=document.getElementById('withdrawAcc').value.trim();
    const method=document.getElementById('withdrawMethod').value;
    if(!amt || !name || !acc){showNotif('Fill all fields'); return;}
    if(amt>currentUser.balance){showNotif('Insufficient balance'); return;}
    const withdraw={amount:amt,name:name,acc:acc,method:method,status:'pending',ts:Date.now()};
    currentUser.withdrawals=currentUser.withdrawals||[];
    currentUser.withdrawals.push(withdraw);
    users=users.map(u=>u.email===currentUser.email?currentUser:u);
    localStorage.setItem('reUsers',JSON.stringify(users));
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    showNotif('Withdrawal request submitted — pending admin approval');
    openSection('withdraw');
}

// ----------------------
// Admin Approve / Reject
// ----------------------
function adminApprove(userEmail,index){
    let user=users.find(u=>u.email===userEmail);
    if(user && user.deposits[index].status==='pending'){
        user.deposits[index].status='approved';
        // Add deposit amount to user balance
        user.balance += user.deposits[index].amount;
        users=users.map(u=>u.email===userEmail?user:u);
        if(currentUser.email===userEmail){currentUser=user; localStorage.setItem('reCurrent',JSON.stringify(currentUser));}
        localStorage.setItem('reUsers',JSON.stringify(users));
        showNotif('Deposit approved');
        openSection('admin');
    }
}

function adminReject(userEmail,index){
    let user=users.find(u=>u.email===userEmail);
    if(user && user.deposits[index].status==='pending'){
        user.deposits[index].status='rejected';
        users=users.map(u=>u.email===userEmail?user:u);
        localStorage.setItem('reUsers',JSON.stringify(users));
        showNotif('Deposit rejected');
        openSection('admin');
    }
}

function adminApproveWithdraw(userEmail,index){
    let user=users.find(u=>u.email===userEmail);
    if(user && user.withdrawals[index].status==='pending'){
        let w=user.withdrawals[index];
        if(user.balance>=w.amount){
            user.balance -= w.amount;
            w.status='approved';
            users=users.map(u=>u.email===userEmail?user:u);
            if(currentUser.email===userEmail){currentUser=user; localStorage.setItem('reCurrent',JSON.stringify(currentUser));}
            localStorage.setItem('reUsers',JSON.stringify(users));
            showNotif('Withdrawal approved');
        } else showNotif('Insufficient balance to approve');
        openSection('admin');
    }
}

function adminRejectWithdraw(userEmail,index){
    let user=users.find(u=>u.email===userEmail);
    if(user && user.withdrawals[index].status==='pending'){
        user.withdrawals[index].status='rejected';
        users=users.map(u=>u.email===userEmail?user:u);
        localStorage.setItem('reUsers',JSON.stringify(users));
        showNotif('Withdrawal rejected');
        openSection('admin');
    }
}

// ----------------------
// Daily Profit Simulation
// ----------------------
function dailyProfitUpdate(){
    users.forEach(u=>{
        if(u.role==='user'){
            (u.plans||[]).forEach(p=>{
                if(p.status==='approved'){
                    u.balance += p.daily;
                    u.profit += p.daily;
                }
            });
        }
    });
    localStorage.setItem('reUsers',JSON.stringify(users));
    if(currentUser){localStorage.setItem('reCurrent',JSON.stringify(currentUser));}
}
setInterval(dailyProfitUpdate, 3600000); // simulate every hour (for demo; replace with daily in real)
</script>
</body>
</html>
