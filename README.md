<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn — Premium Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;color:#fff;overflow-x:hidden;background:linear-gradient(-45deg,#081423,#0a1b2b,#081423,#0a1b2b);background-size:400% 400%;animation:gradientBG 20s ease infinite;}
@keyframes gradientBG{0%{background-position:0 50%}50%{background-position:100% 50%}100%{background-position:0 50%}}
button{cursor:pointer;padding:10px 14px;border:none;border-radius:12px;font-weight:600;transition:.3s;}
button:hover{transform:translateY(-3px) scale(1.05);box-shadow:0 8px 20px rgba(0,247,239,0.3);}
#notif{position:fixed;top:20px;right:-400px;background:#00f7ef;color:#001;padding:12px 22px;border-radius:14px;font-weight:700;transition: right .45s, transform .3s;z-index:60;}
#notif.show{right:20px;transform:translateY(-5px);}
#authBox{position:fixed;left:50%;top:140px;transform:translateX(-50%);width:350px;background:rgba(0,0,0,0.7);padding:22px;border-radius:16px;box-shadow:0 8px 30px rgba(0,247,239,0.3);}
#authBox input{width:100%;margin-bottom:12px;padding:12px;border-radius:12px;border:none;background:#0f1320;color:#dff7fb;transition:.2s;}
#authBox input:focus{outline:none;box-shadow:0 0 12px #00f7ef;}
#sidebar{position:fixed;left:0;top:0;bottom:0;width:80px;background:rgba(10,10,12,0.95);display:none;flex-direction:column;align-items:center;padding:20px 0;gap:16px;z-index:50;}
.icon-btn{width:64px;height:64px;border-radius:16px;background:linear-gradient(180deg,#0b0b10,#0f1220);display:flex;flex-direction:column;align-items:center;justify-content:center;color:#bcdbe3;font-size:14px;text-align:center;transition:.3s;animation:float 2s infinite alternate;}
.icon-btn:hover{transform:translateY(-8px) scale(1.1);box-shadow:0 12px 30px rgba(0,247,239,0.4);}
@keyframes float{0%{transform:translateY(0)}100%{transform:translateY(-6px)}}
#sidebar i{font-size:22px;margin-bottom:2px;}
#contentSection{position:fixed;left:100px;top:140px;right:20px;bottom:20px;background:rgba(0,0,0,0.6);border-radius:16px;padding:24px;overflow:auto;display:none;transition:.3s;}
#backBtn{position:absolute;left:12px;top:12px;background:#ff4766;color:white;padding:10px 12px;border-radius:10px;cursor:pointer;display:none;transition:.3s;}
#logoutBtn{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#ff4766;color:white;padding:12px 16px;border-radius:12px;cursor:pointer;display:none;animation:pulse 2s infinite;}
#refreshBtn{position:fixed;bottom:20px;right:20px;background:#00f7ef;color:#001;padding:12px 16px;border-radius:12px;cursor:pointer;display:none;animation:pulse 2s infinite;}
@keyframes pulse{0%{transform:translateX(-50%) scale(1)}50%{transform:translateX(-50%) scale(1.05)}100%{transform:translateX(-50%) scale(1)}}
#welcomeBox{position:fixed;left:100px;top:20px;right:20px;padding:20px;border-radius:16px;background:rgba(255,255,255,0.02);backdrop-filter:blur(10px);box-shadow:0 4px 40px rgba(0,0,0,0.5);display:none;animation:fadeIn 1s;}
@keyframes fadeIn{0%{opacity:0;transform:translateY(-20px)}100%{opacity:1;transform:translateY(0)}}
.stats{display:flex;gap:20px;margin-top:16px;}
.stat-card{background:rgba(0,247,239,0.05);padding:14px 18px;border-radius:12px;min-width:140px;transition:.3s;}
.stat-card:hover{background:rgba(0,247,239,0.1);transform:scale(1.05);}
.stat-label{font-size:12px;color:#00f7ef;margin:0 0 8px;}
.stat-value{font-size:18px;font-weight:700;margin:0;}
.plan-card{border:1px solid rgba(0,247,239,0.1);border-radius:12px;padding:12px;margin-bottom:12px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0));}
.muted{color:#9fcfda;font-size:13px;}
</style>
<body>

<div id="notif"></div>

<div id="authBox">
<h3 style="text-align:center">Login / Sign Up</h3>
<input id="authName" placeholder="Full Name">
<input id="authEmail" placeholder="Email">
<input id="authPass" type="password" placeholder="Password">
<div style="display:flex; gap:10px;">
<button onclick="signupUser()"><i class="fa fa-user-plus"></i> Sign Up</button>
<button style="background:#00b37e;color:#001;" onclick="loginUser()"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>
<p class="muted" style="margin-top:12px; font-size:13px;">Default admin: <strong>admin@rockearn.com</strong></p>
</div>

<div id="sidebar">
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><br>Plans</div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><br>Deposit</div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><br>Withdraw</div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><br>Profit</div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><br>History</div>
<div class="icon-btn" onclick="openSection('admin')" id="adminBtn" style="display:none"><i class="fa fa-cog"></i><br>Admin</div>
</div>

<div id="welcomeBox">
<h2 id="welcomeTxt">🎉 Welcome, <strong id="userName"></strong> 💎</h2>
<div class="stats">
<div class="stat-card"><div class="stat-label">Balance</div><div class="stat-value" id="balValue">0 PKR</div></div>
<div class="stat-card"><div class="stat-label">Total Profit</div><div class="stat-value" id="profValue">0 PKR</div></div>
</div>
</div>

<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<button id="logoutBtn" onclick="logoutUser()">Logout</button>
<button id="refreshBtn" onclick="refreshDashboard()"><i class="fa fa-sync"></i> Refresh</button><script>
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
function showNotif(msg){
    const el=document.getElementById('notif');
    el.innerText=msg; 
    el.classList.add('show'); 
    setTimeout(()=>el.classList.remove('show'),3000);
}

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
    document.getElementById('refreshBtn').style.display='block';
    document.getElementById('userName').innerText=currentUser.name;
    document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
    document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
    if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';}
    document.getElementById('backBtn').style.display='none';
    document.getElementById('contentSection').style.display='none';
}

function refreshDashboard(){
    if(currentUser){
        currentUser=users.find(u=>u.email===currentUser.email);
        localStorage.setItem('reCurrent',JSON.stringify(currentUser));
        openDashboard();
        showNotif('Dashboard refreshed');
    }
}

function openSection(type){
    const content=document.getElementById('sectionContent');
    document.getElementById('backBtn').style.display='inline-block';
    document.getElementById('contentSection').style.display='block';
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
        (currentUser.withdrawals||[]).forEach(w=>{html+=`<p>Withdraw: ${w.amount} PKR - ${w.status}</p>`});
        content.innerHTML=html;
    } else if(type==='admin' && currentUser.role==='admin'){
        let html='<h3>Admin Panel — Approve / Reject Deposits & Withdrawals</h3>';
        users.forEach(u=>{
            html+=`<div style="margin-bottom:12px;"><b>${u.name} (${u.email})</b><br>Balance: ${u.balance} PKR<br>`;
            (u.deposits||[]).forEach((d,idx)=>{
                if(d.status==='pending'){
                    html+=`Deposit ${d.plan} ${d.amount} PKR - <button onclick="adminApprove('${u.email}',${idx})">Approve</button> <button onclick="adminReject('${u.email}',${idx})">Reject</button><br>`;
                } else {
                    html+=`Deposit ${d.plan} ${d.amount} PKR - ${d.status}<br>`;
                }
            });
            (u.withdrawals||[]).forEach((w,idx)=>{
                if(w.status==='pending'){
                    html+=`Withdraw ${w.amount} PKR (${w.method}) - <button onclick="adminApproveWithdraw('${u.email}',${idx})">Approve</button> <button onclick="adminRejectWithdraw('${u.email}',${idx})">Reject</button><br>`;
                } else {
                    html+=`Withdraw ${w.amount} PKR (${w.method}) - ${w.status}<br>`;
                }
            });
            html+='</div>';
        });
        content.innerHTML=html;
    } else {content.innerHTML='<h3>Coming Soon</h3>';}
}

function closeSection(){
    document.getElementById('contentSection').style.display='none';
    document.getElementById('backBtn').style.display='none';
}

// ----------------------
// Deposit
function openDeposit(amount=0,name='',daily=0,days=0,planIndex=0){
    const content=document.getElementById('sectionContent');
    content.innerHTML=`<h3>Deposit — ${name}</h3>
<p>Amount: <strong>${amount} PKR</strong></p>
<p>JazzCash: <strong>03705519562</strong> <button onclick="copyText('03705519562')">Copy</button></p>
<p>EasyPaisa: <strong>03379827882</strong> <button onclick="copyText('03379827882')">Copy</button></p>
<div style="margin-top:12px">
<input id="depositTxn" placeholder="Transaction ID"><br>
<input id="depositProof" type="file"><br>
<button onclick="confirmDeposit(${amount},'${name}',${daily},${days},${planIndex})">Submit Deposit (Pending Admin)</button>
</div>`;
}

function confirmDeposit(amount,name,daily,days,planIndex){
    const txn=document.getElementById('depositTxn').value.trim();
    const proof=document.getElementById('depositProof').files[0];
    if(!txn || !proof){showNotif('Upload proof & enter txn'); return;}
    const deposit={plan:name,amount:amount,daily:daily,days:days,txn:txn,name:currentUser.name,status:'pending',ts:Date.now()};
    currentUser.deposits=currentUser.deposits||[]; currentUser.deposits.push(deposit);
    users=users.map(u=>u.email===currentUser.email?currentUser:u);
    localStorage.setItem('reUsers',JSON.stringify(users));
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    showNotif('Deposit submitted — waiting admin approval');
    openSection('plans');
}

// ----------------------
// Withdraw
function openWithdraw(){
    const content=document.getElementById('sectionContent');
    document.getElementById('contentSection').style.display='block';
    let html='<h3>Withdraw Funds</h3>';
    html+=`<p>Available Balance: <strong>${currentUser.balance} PKR</strong></p>`;
    html+=`<p>Choose Method: 
<select id="withdrawMethod">
<option value="jazzcash">JazzCash</option>
<option value="easypaisa">EasyPaisa</option>
<option value="bank">Bank</option>
<option value="binance" disabled>Binance (Coming Soon)</option>
</select></p>`;
    html+=`<input id="withdrawAmount" placeholder="Amount to Withdraw"><br>`;
    html+=`<input id="withdrawName" placeholder="Account Holder Name"><br>`;
    html+=`<input id="withdrawAcc" placeholder="Account Number"><br>`;
    html+=`<button onclick="confirmWithdraw()">Submit Withdraw</button>`;
    content.innerHTML=html;
}

function confirmWithdraw(){
    const amt=parseFloat(document.getElementById('withdrawAmount').value);
    const method=document.getElementById('withdrawMethod').value;
    const name=document.getElementById('withdrawName').value.trim();
    const acc=document.getElementById('withdrawAcc').value.trim();
    if(!amt || !name || !acc){showNotif('Fill all fields'); return;}
    if(amt>currentUser.balance){showNotif('Insufficient balance'); return;}
    const w={amount:amt,method:method,name:name,acc:acc,status:'pending',ts:Date.now()};
    currentUser.withdrawals=currentUser.withdrawals||[]; currentUser.withdrawals.push(w);
    users=users.map(u=>u.email===currentUser.email?currentUser:u);
    currentUser.balance-=amt;
    localStorage.setItem('reUsers',JSON.stringify(users));
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    showNotif('Withdrawal request submitted & balance updated');
    openDashboard();
}

// ----------------------
// Copy Text
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied: '+txt);}
</script><script>
// ----------------------
// Admin Approve / Reject Deposit
// ----------------------
function adminApprove(userEmail,depositIndex){
    let u = users.find(x=>x.email===userEmail);
    if(!u) return;
    let d = u.deposits[depositIndex];
    if(d.status!=='pending') return;
    d.status='approved';
    u.balance += d.amount; // Deposit approved, balance updated
    users=users.map(x=>x.email===u.email?u:x);
    localStorage.setItem('reUsers',JSON.stringify(users));
    if(currentUser && currentUser.email===userEmail){localStorage.setItem('reCurrent',JSON.stringify(u));}
    showNotif(`Deposit of ${d.amount} PKR approved`);
    openSection('admin');
}

function adminReject(userEmail,depositIndex){
    let u = users.find(x=>x.email===userEmail);
    if(!u) return;
    let d = u.deposits[depositIndex];
    if(d.status!=='pending') return;
    d.status='rejected';
    users=users.map(x=>x.email===u.email?u:x);
    localStorage.setItem('reUsers',JSON.stringify(users));
    showNotif(`Deposit of ${d.amount} PKR rejected`);
    openSection('admin');
}

// ----------------------
// Admin Approve / Reject Withdraw
// ----------------------
function adminApproveWithdraw(userEmail,withdrawIndex){
    let u = users.find(x=>x.email===userEmail);
    if(!u) return;
    let w = u.withdrawals[withdrawIndex];
    if(w.status!=='pending') return;
    w.status='approved';
    users=users.map(x=>x.email===u.email?u:x);
    localStorage.setItem('reUsers',JSON.stringify(users));
    showNotif(`Withdrawal of ${w.amount} PKR approved`);
    openSection('admin');
}

function adminRejectWithdraw(userEmail,withdrawIndex){
    let u = users.find(x=>x.email===userEmail);
    if(!u) return;
    let w = u.withdrawals[withdrawIndex];
    if(w.status!=='pending') return;
    w.status='rejected';
    u.balance += w.amount; // Refund balance
    users=users.map(x=>x.email===u.email?u:x);
    localStorage.setItem('reUsers',JSON.stringify(users));
    showNotif(`Withdrawal of ${w.amount} PKR rejected`);
    openSection('admin');
}

// ----------------------
// Daily Profit Scheduler
// ----------------------
function addDailyProfit(){
    let now = new Date().getTime();
    users.forEach(u=>{
        (u.deposits||[]).forEach(d=>{
            if(d.status==='approved' && !d.lastProfit){
                d.lastProfit = now;
                u.profit += d.daily;
                u.balance += d.daily;
            } else if(d.status==='approved'){
                let daysPassed = Math.floor((now - d.lastProfit)/(1000*60*60*24));
                if(daysPassed>0){
                    let totalProfit = d.daily*daysPassed;
                    u.profit += totalProfit;
                    u.balance += totalProfit;
                    d.lastProfit += daysPassed*24*60*60*1000;
                }
            }
        });
    });
    localStorage.setItem('reUsers',JSON.stringify(users));
    if(currentUser){currentUser=users.find(u=>u.email===currentUser.email); localStorage.setItem('reCurrent',JSON.stringify(currentUser));}
}

// Run daily profit scheduler every 1 min (for demo purpose)
setInterval(addDailyProfit,60000);

// ----------------------
// Back button fix on refresh
// ----------------------
window.onload=function(){
    if(currentUser){
        openDashboard();
    } else {
        document.getElementById('authBox').style.display='block';
        document.getElementById('sidebar').style.display='none';
        document.getElementById('welcomeBox').style.display='none';
        document.getElementById('logoutBtn').style.display='none';
    }
};
</script>
