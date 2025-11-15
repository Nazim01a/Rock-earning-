<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium Dashboard</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
<style>
body{margin:0;font-family:'Segoe UI',sans-serif;background:#0e0e15;color:white;overflow-x:hidden;}
canvas#bgCanvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:-1;}
.sidebar{position:fixed;top:100px;left:0;width:180px;height:auto;background:rgba(20,20,20,0.95);display:flex;flex-direction:column;align-items:center;padding:20px 0;gap:15px;z-index:10;display:none;border-radius:0 20px 20px 0;}
.icon-btn{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;transition:0.3s;background:#111;color:#ccc;box-shadow:0 0 5px #000;}
.icon-btn:hover{transform:scale(1.1);box-shadow:0 0 15px #0ff,0 0 25px #0ff;}
.icon-name{margin-top:6px;font-size:12px;color:#ccc;text-shadow:0 0 3px #000;}
#welcomeBox{position:fixed;top:20px;left:200px;right:20px;padding:25px;border-radius:20px;background:rgba(0,0,0,0.8);backdrop-filter:blur(12px);text-align:center;display:none;animation:fadeIn 1.5s;}
#welcomeBox h2{color:white;font-size:26px;font-weight:800;animation:glowText 2.5s infinite alternate;}
.stats{display:flex;justify-content:center;gap:30px;margin-top:15px;flex-wrap:wrap;}
.stat-card{background: rgba(0,255,255,0.05); padding: 18px 25px; border-radius: 15px; box-shadow: 0 0 5px #0ff,0 0 10px #0ff; transition: 0.4s;}
.stat-label{font-size:12px;color:#0ff;letter-spacing:1px;margin-bottom:5px;}
.stat-value{font-size:18px;font-weight:700;color:white;}
.refresh-btn{margin-top:10px;}
#contentSection{position:fixed;top:160px;left:200px;right:20px;bottom:20px;background:rgba(0,0,0,0.85);backdrop-filter:blur(15px);border-radius:20px;padding:20px;overflow-y:auto;display:none;z-index:999;}
#backBtn{position:absolute;top:10px;left:20px;background:#ff0044;padding:8px 12px;border:none;border-radius:10px;font-weight:700;cursor:pointer;transition:0.3s;}
#backBtn:hover{transform:scale(1.05);background:#ff3366;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#0ff,#0ffcc0);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #0ff;transform:scale(1.04);}
.btn{background:#111;color:#0ff;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 5px #0ff,0 0 10px #0ff;transition:0.3s;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}
.logout-btn{position:fixed;bottom:60px;left:50%;transform:translateX(-50%);background:#ff0044;color:white;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 10px #ff3366;transition:0.3s;display:none;}
.logout-btn:hover{transform:scale(1.05);box-shadow:0 0 20px #ff3366,0 0 30px #ff6688;}
.admin-section{background:rgba(10,10,30,0.9);padding:20px;border-radius:15px;}
@keyframes glowText{0%{text-shadow:0 0 5px #000,0 0 10px #0ff;}50%{text-shadow:0 0 12px #000,0 0 25px #0ff;}100%{text-shadow:0 0 5px #000,0 0 10px #0ff;}}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>

<div class="sidebar" id="sidebar">
<div style="font-size:28px;margin-bottom:10px;">🚀</div>
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><span class="icon-name">Plans</span></div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><span class="icon-name">Deposit</span></div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><span class="icon-name">Withdraw</span></div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><span class="icon-name">Profit</span></div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><span class="icon-name">History</span></div>
<div class="icon-btn" onclick="openSection('support')"><i class="fa fa-headset"></i><span class="icon-name">Support</span></div>
<div class="icon-btn" onclick="openSection('referral')"><i class="fa fa-share-alt"></i><span class="icon-name">Referral</span></div>
<div class="icon-btn" onclick="openSection('share')"><i class="fa fa-share"></i><span class="icon-name">Share</span></div>
<div class="icon-btn" onclick="openSection('admin')"><i class="fa fa-user-shield"></i><span class="icon-name">Admin</span></div>
</div>

<div id="welcomeBox">
<h2>🎉 Welcome to <span style="color:white;font-weight:900;">Rock Earn</span> Premium 💎</h2>
<div class="stats">
<div class="stat-card"><p class="stat-label">Balance</p><p class="stat-value" id="balValue">0 PKR</p></div>
<div class="stat-card"><p class="stat-label">Total Profit</p><p class="stat-value" id="profValue">0 PKR</p></div>
<button class="refresh-btn btn" onclick="refreshBalance()">Refresh</button>
</div>
</div>

<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<div id="authBox" class="card" style="margin-left:200px;margin-top:20px;width:320px;">
<h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
<input id="authName" placeholder="Full Name" />
<input id="authEmail" placeholder="Email" />
<input id="authPass" type="password" placeholder="Password" />
<button onclick="signupUser()" class="btn w-full mb-2"><i class="fa fa-user-plus"></i> Sign Up</button>
<button onclick="loginUser()" class="btn w-full mb-2 bg-green-600 hover:bg-green-700"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>

<button class="logout-btn" id="logoutBtn" onclick="logoutUser()">Logout</button>

<script>
// Particle Background
const canvas=document.getElementById('bgCanvas'),ctx=canvas.getContext('2d');
canvas.width=window.innerWidth;canvas.height=window.innerHeight;
let particles=[];for(let i=0;i<150;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2+1,s:Math.random()*0.5+0.5,color:`rgba(0,255,255,${Math.random()})`});}
function animateParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,2*Math.PI);ctx.fillStyle=p.color;ctx.fill();p.y-=p.s;if(p.y<0)p.y=canvas.height;});requestAnimationFrame(animateParticles);}animateParticles();

// Notifications
function showNotif(msg){const n=document.getElementById('notif');n.innerText=msg;n.classList.add('show');setTimeout(()=>{n.classList.remove('show');},3000);}

// Users
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;
if(!currentUser){currentUser=null;}
window.addEventListener('load',()=>{if(currentUser){openDashboard();}});

// Auth functions
function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}
function signupUser(){let n=document.getElementById('authName').value.trim();let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();if(!n||!e||!p)return showNotif('Fill all fields');if(!validateEmail(e))return showNotif('Invalid email');if(users.find(u=>u.email===e))return showNotif('Email already registered');let u={name:n,email:e,pass:p,plans:[],deposits:[],withdrawals:[],balance:0,profit:0,role:'user'};users.push(u);localStorage.setItem('reUsers',JSON.stringify(users));currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function loginUser(){let e=document.getElementById('authEmail').value.trim();let p=document.getElementById('authPass').value.trim();let u=users.find(x=>x.email===e&&x.pass===p);if(!u)return showNotif('Invalid credentials');currentUser=u;localStorage.setItem('reCurrent',JSON.stringify(u));openDashboard();}
function logoutUser(){currentUser=null;localStorage.removeItem('reCurrent');location.reload();}
function openDashboard(){document.getElementById('authBox').style.display='none';document.getElementById('sidebar').style.display='flex';document.getElementById('welcomeBox').style.display='block';document.getElementById('logoutBtn').style.display='block';document.getElementById('balValue').innerText=currentUser.balance+' PKR';document.getElementById('profValue').innerText=currentUser.profit+' PKR';}
function refreshBalance(){document.getElementById('balValue').innerText=currentUser.balance+' PKR';document.getElementById('profValue').innerText=currentUser.profit+' PKR';showNotif('Balance refreshed!');}

// Plans array
let plans=[
{name:'Basic',amount:200,daily:30,days:1},{name:'Starter',amount:500,daily:75,days:1},{name:'Pro',amount:1000,daily:180,days:1},
{name:'Advanced',amount:1500,daily:250,days:2},{name:'Silver',amount:2000,daily:350,days:3},{name:'Gold',amount:3000,daily:550,days:3},
{name:'Platinum',amount:5000,daily:950,days:4},{name:'Diamond',amount:7000,daily:1350,days:5}
];

// Section opening logic
function openSection(type){
const sec=document.getElementById('contentSection');const content=document.getElementById('sectionContent');sec.style.display='block';
if(type==='plans'){content.innerHTML='<h2>Available Plans</h2>'+plans.map(p=>`<div class="plan-card"><b>${p.name}</b> - ${p.amount} PKR - Daily: ${p.daily} PKR - <span>${p.days} Days</span><br><button class="btn mt-2" onclick="openDeposit(${p.amount},\'${p.name}\',${p.daily},${p.days})">Buy Now</button></div>`).join('');}
else if(type==='deposit'){openDeposit();}
else if(type==='withdraw'){content.innerHTML=`<h2>Withdraw</h2><input type='number' placeholder='Amount' id='withdrawAmt'><select id='withdrawType'><option>JazzCash</option><option>EasyPaisa</option><option>Bank</option></select><input type='text' placeholder='Account Number' id='withdrawAcc'><input type='file' id='withdrawProof'><button class='btn mt-2' onclick='confirmWithdraw()'>Withdraw</button>`;}
else if(type==='profit'){content.innerHTML=`<h2>Profit</h2><p>Total profit earned: ${currentUser.profit} PKR</p>`;}
else if(type==='history'){content.innerHTML=`<h2>Activity History</h2><p>Deposits: ${currentUser.deposits.length}</p><p>Withdrawals: ${currentUser.withdrawals.length}</p>`;}
else if(type==='support'){content.innerHTML=`<h2>Support</h2><p>Contact: support@rockearnpro.com</p>`;}
else if(type==='referral'){content.innerHTML=`<h2>Referral</h2><p>Share your link: https://rockearnpro.com?ref=${currentUser.email}</p>`;}
else if(type==='share'){content.innerHTML=`<h2>Share</h2><input type='text' value='https://rockearnpro.com?ref=${currentUser.email}' readonly><button class='btn mt-2' onclick='copyText("https://rockearnpro.com?ref=${currentUser.email}")'>Copy</button>`;}
else if(type==='admin'){openAdmin();}
else{content.innerHTML=`<h2>${type}</h2><p>Coming Soon...</p>`;}
}
function closeSection(){document.getElementById('contentSection').style.display='none';}

// Deposit open
function openDeposit(amount=0,name='',daily=0,days=0){
const sec=document.getElementById('contentSection');const content=document.getElementById('sectionContent');sec.style.display='block';
content.innerHTML=`<h2>Deposit for ${name}</h2>
<p>Amount: <b>${amount} PKR</b></p>
<p>JazzCash: 03705519562 <button class='btn' onclick='copyText("03705519562")'>Copy</button></p>
<p>EasyPaisa: 03379827882 <button class='btn' onclick='copyText("03379827882")'>Copy</button></p>
<input type='file' id='proofFile'><input type='text' id='txnId' placeholder='Transaction ID'>
<button class='btn mt-2' onclick='confirmDeposit(${amount},"${name}",${daily},${days})'>Submit Deposit</button>`;}

function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}<script>
// Confirm deposit
function confirmDeposit(amount,name,daily,days){
    let file=document.getElementById('proofFile').files[0];
    let txn=document.getElementById('txnId').value.trim();
    if(!file || !txn){showNotif('Please upload proof & enter transaction ID'); return;}
    let deposit={plan:name,amount:amount,daily:daily,days:days,txn:txn,file:file.name,status:'pending',user:currentUser.email,time:Date.now()};
    currentUser.deposits.push(deposit);
    currentUser.plans.push({name:name,amount:amount,daily:daily,days:days,start:Date.now(),profitAdded:false});
    currentUser.balance+=amount;
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    let allUsers=JSON.parse(localStorage.getItem('reUsers'));
    let idx=allUsers.findIndex(u=>u.email===currentUser.email);
    allUsers[idx]=currentUser;
    localStorage.setItem('reUsers',JSON.stringify(allUsers));
    showNotif(`${name} plan activated! Profit will add every 24h`);
    openSection('profit');
}

// Confirm withdraw
function confirmWithdraw(){
    let amt=parseInt(document.getElementById('withdrawAmt').value);
    let type=document.getElementById('withdrawType').value;
    let acc=document.getElementById('withdrawAcc').value.trim();
    let proof=document.getElementById('withdrawProof').files[0];
    if(!amt || !type || !acc || !proof){showNotif('Fill all withdrawal fields'); return;}
    if(amt>currentUser.balance){showNotif('Insufficient balance'); return;}
    let withdraw={amount:amt,type:type,acc:acc,proof:proof.name,status:'pending',time:Date.now(),user:currentUser.email};
    currentUser.withdrawals.push(withdraw);
    currentUser.balance-=amt;
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    let allUsers=JSON.parse(localStorage.getItem('reUsers'));
    let idx=allUsers.findIndex(u=>u.email===currentUser.email);
    allUsers[idx]=currentUser;
    localStorage.setItem('reUsers',JSON.stringify(allUsers));
    showNotif('Withdrawal request submitted!');
    openSection('withdraw');
}

// Auto daily profit
setInterval(()=>{
    if(currentUser && currentUser.plans){
        let now=Date.now();
        currentUser.plans.forEach(p=>{
            let elapsed=Math.floor((now-p.start)/(1000*60*60*24));
            if(elapsed>=1 && !p.profitAdded){
                currentUser.balance+=p.daily;
                currentUser.profit+=p.daily;
                p.profitAdded=true;
                localStorage.setItem('reCurrent',JSON.stringify(currentUser));
                let allUsers=JSON.parse(localStorage.getItem('reUsers'));
                let idx=allUsers.findIndex(u=>u.email===currentUser.email);
                allUsers[idx]=currentUser;
                localStorage.setItem('reUsers',JSON.stringify(allUsers));
                refreshBalance();
                showNotif('Daily profit added!');
            }
        });
    }
},10000);

// Admin panel
function openAdmin(){
    if(currentUser.role!=='admin'){showNotif('Access denied!'); return;}
    const content=document.getElementById('sectionContent');
    content.innerHTML=`<h2>Admin Panel</h2><div class='admin-section'>
    <h3>Pending Deposits</h3>
    <div id='adminDeposits'></div>
    <h3>Pending Withdrawals</h3>
    <div id='adminWithdrawals'></div>
    </div>`;
    renderAdmin();
}

function renderAdmin(){
    let usersList=JSON.parse(localStorage.getItem('reUsers'));
    let depHTML='';
    let witHTML='';
    usersList.forEach(u=>{
        u.deposits.forEach(d=>{
            if(d.status==='pending'){
                depHTML+=`<p>${u.name} - ${d.plan} - ${d.amount} PKR <button class='btn' onclick='approveDeposit("${u.email}",${u.deposits.indexOf(d)})'>Approve</button> <button class='btn' onclick='rejectDeposit("${u.email}",${u.deposits.indexOf(d)})'>Reject</button></p>`;
            }
        });
        u.withdrawals.forEach(w=>{
            if(w.status==='pending'){
                witHTML+=`<p>${u.name} - ${w.amount} PKR - ${w.type} <button class='btn' onclick='approveWithdraw("${u.email}",${u.withdrawals.indexOf(w)})'>Approve</button> <button class='btn' onclick='rejectWithdraw("${u.email}",${u.withdrawals.indexOf(w)})'>Reject</button></p>`;
            }
        });
    });
    document.getElementById('adminDeposits').innerHTML=depHTML || '<p>No pending deposits</p>';
    document.getElementById('adminWithdrawals').innerHTML=witHTML || '<p>No pending withdrawals</p>';
}

function approveDeposit(email,idx){let usersList=JSON.parse(localStorage.getItem('reUsers'));usersList.forEach(u=>{if(u.email===email){u.deposits[idx].status='approved';}});localStorage.setItem('reUsers',JSON.stringify(usersList));showNotif('Deposit approved');renderAdmin();}
function rejectDeposit(email,idx){let usersList=JSON.parse(localStorage.getItem('reUsers'));usersList.forEach(u=>{if(u.email===email){u.deposits[idx].status='rejected';}});localStorage.setItem('reUsers',JSON.stringify(usersList));showNotif('Deposit rejected');renderAdmin();}
function approveWithdraw(email,idx){let usersList=JSON.parse(localStorage.getItem('reUsers'));usersList.forEach(u=>{if(u.email===email){u.withdrawals[idx].status='approved';}});localStorage.setItem('reUsers',JSON.stringify(usersList));showNotif('Withdrawal approved');renderAdmin();}
function rejectWithdraw(email,idx){let usersList=JSON.parse(localStorage.getItem('reUsers'));usersList.forEach(u=>{if(u.email===email){u.withdrawals[idx].status='rejected';}});localStorage.setItem('reUsers',JSON.stringify(usersList));showNotif('Withdrawal rejected');renderAdmin();}
</script>
</body>
</html>
