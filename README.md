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
/* Sidebar */
.sidebar{position:fixed;top:0;left:0;width:180px;height:100vh;background:rgba(20,20,20,0.95);display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:15px;z-index:10;display:none;overflow-y:auto;}
.icon-btn{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;transition:0.3s;background:#111;color:#ccc;box-shadow:0 0 5px #000;width:100%;}
.icon-btn:hover{transform:scale(1.05);box-shadow:0 0 15px #0ff,0 0 25px #0ff;}
.icon-name{margin-top:6px;font-size:12px;color:#ccc;text-shadow:0 0 3px #000;}
/* Welcome Box */
#welcomeBox{position:fixed;top:20px;left:200px;right:20px;padding:25px;border-radius:20px;background:rgba(0,0,0,0.8);backdrop-filter:blur(12px);text-align:center;display:none;animation:fadeIn 1.5s;}
#welcomeBox h2{color:white;font-size:26px;font-weight:800;animation:glowText 2.5s infinite alternate;}
.stats{display:flex;justify-content:center;gap:30px;margin-top:15px;flex-wrap:wrap;}
.stat-card{background: rgba(0,255,255,0.05); padding: 18px 25px; border-radius: 15px; box-shadow: 0 0 5px #0ff,0 0 10px #0ff; transition: 0.4s;}
.stat-label{font-size:12px;color:#0ff;letter-spacing:1px;margin-bottom:5px;}
.stat-value{font-size:18px;font-weight:700;color:white;}
.refresh-btn{margin-top:10px;}
/* Content Section */
#contentSection{position:fixed;top:160px;left:200px;right:20px;bottom:20px;background:rgba(0,0,0,0.85);backdrop-filter:blur(15px);border-radius:20px;padding:20px;overflow-y:auto;display:none;z-index:999;}
#backBtn{position:absolute;top:10px;left:20px;background:#ff0044;padding:8px 12px;border:none;border-radius:10px;font-weight:700;cursor:pointer;transition:0.3s;}
#backBtn:hover{transform:scale(1.05);background:#ff3366;}
#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#0ff,#0ffcc0);padding:15px 25px;border-radius:12px;box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}
input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}
.plan-card{border:1px solid #0ff;padding:12px;margin-bottom:12px;border-radius:12px;transition:0.3s;}
.plan-card:hover{box-shadow:0 0 25px #0ff,0 0 50px #0ff;transform:scale(1.04);}
.btn{background:#111;color:#0ff;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 5px #0ff,0 0 10px #0ff;transition:0.3s;width:100%;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}
.logout-btn{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#ff0044;color:white;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 10px #ff3366;transition:0.3s;display:none;}
.logout-btn:hover{transform:scale(1.05);box-shadow:0 0 20px #ff3366,0 0 30px #ff6688;}
@keyframes glowText{0%{text-shadow:0 0 5px #000,0 0 10px #0ff;}50%{text-shadow:0 0 12px #000,0 0 25px #0ff;}100%{text-shadow:0 0 5px #000,0 0 10px #0ff;}}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
@media (max-width: 1024px){
    #welcomeBox{left:20px;right:20px;}
    #contentSection{top:220px;left:20px;right:20px;bottom:20px;}
    .sidebar{width:100%;height:auto;flex-direction:row;justify-content:space-around;padding:10px;display:flex;position:relative;}
}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>

<!-- Sidebar -->
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

<!-- Welcome Box -->
<div id="welcomeBox">
<h2>🎉 Welcome to <span style="color:white;font-weight:900;">Rock Earn</span> Premium 💎</h2>
<div class="stats">
<div class="stat-card"><p class="stat-label">Balance</p><p class="stat-value" id="balValue">0 PKR</p></div>
<div class="stat-card"><p class="stat-label">Total Profit</p><p class="stat-value" id="profValue">0 PKR</p></div>
<button class="refresh-btn btn" onclick="refreshBalance()">Refresh</button>
</div>
</div>

<!-- Content Section -->
<div id="contentSection">
<button id="backBtn" onclick="closeSection()">← Back</button>
<div id="sectionContent"></div>
</div>

<!-- Auth Box -->
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
let particles=[];
for(let i=0;i<150;i++){particles.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2+1,s:Math.random()*0.5+0.5,color:`rgba(0,255,255,${Math.random()})`});}
function animateParticles(){ctx.clearRect(0,0,canvas.width,canvas.height);particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,2*Math.PI);ctx.fillStyle=p.color;ctx.fill();p.y-=p.s;if(p.y<0)p.y=canvas.height;});requestAnimationFrame(animateParticles);}animateParticles();
</script><script>
// Notifications
function showNotif(msg){const n=document.getElementById('notif');n.innerText=msg;n.classList.add('show');setTimeout(()=>{n.classList.remove('show');},3000);}

// Auth System
let users=JSON.parse(localStorage.getItem('reUsers'))||[];
let currentUser=JSON.parse(localStorage.getItem('reCurrent'))||null;

window.addEventListener('load',()=>{
    if(currentUser){
        openDashboard();
    }
});

// Validate Email
function validateEmail(e){return/^\S+@\S+\.\S+$/.test(e);}

// Sign Up
function signupUser(){
    let n=document.getElementById('authName').value.trim();
    let e=document.getElementById('authEmail').value.trim();
    let p=document.getElementById('authPass').value.trim();
    if(!n||!e||!p) return showNotif('Fill all fields');
    if(!validateEmail(e)) return showNotif('Invalid email');
    if(users.find(u=>u.email===e)) return showNotif('Email already registered');
    let u={name:n,email:e,pass:p,plans:[],balance:0,profit:0,history:[]};
    users.push(u);
    localStorage.setItem('reUsers',JSON.stringify(users));
    currentUser=u;
    localStorage.setItem('reCurrent',JSON.stringify(u));
    openDashboard();
}

// Login
function loginUser(){
    let e=document.getElementById('authEmail').value.trim();
    let p=document.getElementById('authPass').value.trim();
    let u=users.find(x=>x.email===e&&x.pass===p);
    if(!u) return showNotif('Invalid credentials');
    currentUser=u;
    localStorage.setItem('reCurrent',JSON.stringify(u));
    openDashboard();
}

// Logout
function logoutUser(){
    currentUser=null;
    localStorage.removeItem('reCurrent');
    location.reload();
}

// Open Dashboard after login
function openDashboard(){
    document.getElementById('authBox').style.display='none';
    document.getElementById('sidebar').style.display='flex';
    document.getElementById('welcomeBox').style.display='block';
    document.getElementById('logoutBtn').style.display='block';
    document.getElementById('balValue').innerText=currentUser.balance+' PKR';
    document.getElementById('profValue').innerText=currentUser.profit+' PKR';
}

// Refresh Balance
function refreshBalance(){
    document.getElementById('balValue').innerText=currentUser.balance+' PKR';
    document.getElementById('profValue').innerText=currentUser.profit+' PKR';
    showNotif('Balance refreshed!');
}

// Plans Setup
let plans=[
    {name:'Basic',amount:200,daily:30,days:1,sold:false},
    {name:'Starter',amount:500,daily:75,days:1,sold:false},
    {name:'Pro',amount:1000,daily:180,days:1,sold:false},
    {name:'Advanced',amount:1500,daily:250,days:2,sold:false},
    {name:'Silver',amount:2000,daily:350,days:3,sold:false},
    {name:'Gold',amount:3000,daily:550,days:3,sold:false},
    {name:'Platinum',amount:5000,daily:950,days:4,sold:false},
    {name:'Diamond',amount:7000,daily:1350,days:5,sold:false},
];

// Open Section
function openSection(type){
    const sec=document.getElementById('contentSection');
    const content=document.getElementById('sectionContent');
    sec.style.display='block';

    if(type==='plans'){
        content.innerHTML='<h2>Available Plans</h2>'+plans.map((p,i)=>{
            if(p.sold) return `<div class="plan-card"><b>${p.name}</b> - SOLD OUT</div>`;
            return `<div class="plan-card"><b>${p.name}</b> - ${p.amount} PKR - Daily: ${p.daily} PKR - <span>${p.days} Days</span><br><button class="btn mt-2" onclick="openDeposit(${i})">Buy Now</button></div>`;
        }).join('');
    }
    else if(type==='deposit'){
        openDeposit();
    }
    else if(type==='withdraw'){
        content.innerHTML=`<h2>Withdraw</h2>
        <input type='number' id='withdrawAmount' placeholder='Amount'>
        <select id='withdrawMethod'>
        <option>JazzCash</option>
        <option>EasyPaisa</option>
        <option>Bank</option>
        </select>
        <input type='text' id='withdrawAcc' placeholder='Account Number'>
        <input type='file' id='withdrawProof'>
        <button class='btn mt-2' onclick='confirmWithdraw()'>Withdraw</button>`;
    }
    else if(type==='profit'){
        content.innerHTML=`<h2>Profit</h2><p>Total profit earned: ${currentUser.profit} PKR</p>`;
    }
    else if(type==='history'){
        let hist=currentUser.history.map(h=>`<p>• ${h}</p>`).join('');
        content.innerHTML=`<h2>Activity History</h2>${hist || '<p>No history yet.</p>'}`;
    }
    else if(type==='support'){
        content.innerHTML=`<h2>Support</h2><p>Contact: support@rockearnpro.com</p>`;
    }
    else if(type==='referral'){
        content.innerHTML=`<h2>Referral</h2><p>Share your link to earn bonus!</p>`;
    }
    else if(type==='share'){
        content.innerHTML=`<h2>Share</h2><input type='text' value='https://rockearnpro.com?ref=${currentUser.email}' readonly><button class='btn mt-2' onclick='copyText("https://rockearnpro.com?ref=${currentUser.email}")'>Copy</button>`;
    }
    else if(type==='admin'){
        // Admin view
        content.innerHTML=`<h2>Admin Panel</h2>
        <p>All Users & Activities</p>
        ${users.map(u=>`<div class="plan-card"><b>${u.name}</b> (${u.email})<br>Balance: ${u.balance} PKR<br>Profit: ${u.profit} PKR<br>History: ${u.history.join(', ') || 'None'}</div>`).join('')}`;
    }
    else{content.innerHTML=`<h2>${type}</h2><p>Coming Soon...</p>`;}
}

function closeSection(){document.getElementById('contentSection').style.display='none';}

// Copy Referral
function copyText(txt){navigator.clipboard.writeText(txt);showNotif('Copied!');}

// Deposit System
function openDeposit(planIndex){
    const p=plans[planIndex];
    const sec=document.getElementById('contentSection');
    const content=document.getElementById('sectionContent');
    sec.style.display='block';
    content.innerHTML=`<h2>Deposit for ${p.name}</h2>
    <p>Amount: <b>${p.amount} PKR</b></p>
    <p>JazzCash: 03705519562 <button class='btn' onclick='copyText("03705519562")'>Copy</button></p>
    <p>EasyPaisa: 03379827882 <button class='btn' onclick='copyText("03379827882")'>Copy</button></p>
    <input type='file' id='proofFile'>
    <input type='text' id='txnId' placeholder='Transaction ID'>
    <button class='btn mt-2' onclick='confirmDeposit(${planIndex})'>Submit Deposit</button>`;
}

function confirmDeposit(planIndex){
    let file=document.getElementById('proofFile').files[0];
    let txn=document.getElementById('txnId').value.trim();
    if(!file||!txn){showNotif('Upload proof & enter transaction ID'); return;}
    const p=plans[planIndex];
    currentUser.plans.push({...p,ts:Date.now(),profitAdded:false});
    currentUser.balance+=p.amount;
    currentUser.history.push(`Deposited ${p.amount} PKR for ${p.name}`);
    plans[planIndex].sold=true; // mark sold
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    localStorage.setItem('reUsers',JSON.stringify(users));
    showNotif(`${p.name} plan activated!`);
    openSection('profit');
}

// Withdraw System
function confirmWithdraw(){
    let amt=parseFloat(document.getElementById('withdrawAmount').value);
    let method=document.getElementById('withdrawMethod').value;
    let acc=document.getElementById('withdrawAcc').value.trim();
    let proof=document.getElementById('withdrawProof').files[0];
    if(!amt||!method||!acc||!proof){showNotif('Fill all withdraw fields');return;}
    if(amt>currentUser.balance){showNotif('Insufficient balance');return;}
    currentUser.balance-=amt;
    currentUser.history.push(`Withdrew ${amt} PKR via ${method}`);
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    localStorage.setItem('reUsers',JSON.stringify(users));
    showNotif(`Withdrawal ${amt} PKR submitted`);
    openSection('profit');
}

// Auto Daily Profit
setInterval(()=>{
    if(currentUser){
        let now=Date.now();
        currentUser.plans.forEach(p=>{
            let elapsed=Math.floor((now-p.ts)/(1000*60*60*24));
            if(elapsed>=1 && !p.profitAdded){
                currentUser.balance+=p.daily;
                currentUser.profit+=p.daily;
                p.profitAdded=true;
                localStorage.setItem('reCurrent',JSON.stringify(currentUser));
                refreshBalance();
            }
        });
    }
},10000); // check every 10 sec for demo
</script><!-- Company Details Section -->
<div id="companyDetails" style="position:fixed;bottom:20px;right:20px;background:rgba(0,0,0,0.85);padding:20px;border-radius:15px;backdrop-filter:blur(12px);color:#0ff;box-shadow:0 0 15px #0ff;">
<h3 style="margin:0 0 10px 0;font-size:18px;">🏢 Rock Earn Pro</h3>
<p style="margin:2px 0;">Launch Location: 25 Main St, California, United States</p>
<p style="margin:2px 0;">Company Name: Rock Earn Pro LTD</p>
<p style="margin:2px 0;">Founded: 2025</p>
<p style="margin:2px 0;">Support: support@rockearnpro.com</p>
</div>

<style>
/* Sidebar icons fixed left, properly spaced */
.sidebar{
    position:fixed;
    top:100px;
    left:0;
    width:80px;
    height:auto;
    display:flex;
    flex-direction:column;
    align-items:center;
    gap:20px;
    padding:10px;
    z-index:999;
}
.icon-btn{
    width:60px;
    height:60px;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    border-radius:15px;
    cursor:pointer;
    transition:0.3s;
    background:#111;
    color:#0ff;
    box-shadow:0 0 5px #000;
}
.icon-btn:hover{
    transform:scale(1.1);
    box-shadow:0 0 15px #0ff,0 0 25px #0ff;
}
.icon-name{font-size:10px;margin-top:5px;text-align:center;color:#0ff;}

/* Welcome Box Stickers & Animations */
#welcomeBox h2{
    display:flex;
    justify-content:center;
    align-items:center;
    gap:10px;
}
#welcomeBox h2 span{
    display:inline-block;
    animation:rotateSticker 3s linear infinite;
}
@keyframes rotateSticker{
    0%{transform:rotate(0deg);}
    50%{transform:rotate(180deg);}
    100%{transform:rotate(360deg);}
}

/* Scroll & HD style for normal screen */
#contentSection{
    max-width:1200px;
    left:100px;
    top:150px;
    right:20px;
    bottom:20px;
    overflow-y:auto;
    padding:25px;
    border-radius:20px;
    backdrop-filter:blur(15px);
}
</style>

<script>
// Fix icons visibility based on login
function updateSidebarIcons(){
    const sidebar=document.getElementById('sidebar');
    if(currentUser){
        sidebar.style.display='flex';
    }else{
        sidebar.style.display='none';
    }
}
updateSidebarIcons();

// Example: Sidebar icons click handling already in Part 2
// Plans / Deposit / Withdraw / Profit / History / Support / Referral / Share / Admin

// Premium Welcome Box Stickers (random stars)
const welcomeBox=document.getElementById('welcomeBox');
const stickers=[];
for(let i=0;i<15;i++){
    const span=document.createElement('span');
    span.innerText='✨';
    span.style.fontSize=(12+Math.random()*10)+'px';
    span.style.position='absolute';
    span.style.top=Math.random()*50+'%';
    span.style.left=Math.random()*50+'%';
    span.style.animationDuration=(2+Math.random()*3)+'s';
    welcomeBox.appendChild(span);
    stickers.push(span);
}
</script>
