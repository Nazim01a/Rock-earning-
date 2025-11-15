<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium</title>

<!-- Tailwind -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<style>
body{
  margin:0;
  padding:0;
  font-family:'Poppins',sans-serif;
  background:#0d0d0d;
  overflow-x:hidden;
}

/* Premium Brown-Black Gradient Background + Animation */
.bg-animate{
  background: radial-gradient(circle at center, #3b2f2f, #0d0d0d 70%);
  animation: bgMove 10s infinite alternate ease-in-out;
}
@keyframes bgMove{
  0%{background-position: 0% 0%;}
  100%{background-position: 100% 100%;}
}

/* Glass Effect Cards */
.glass{
  background: rgba(255,255,255,0.05);
  backdrop-filter: blur(10px);
  border:1px solid rgba(255,255,255,0.1);
  border-radius:16px;
  padding:16px;
}

/* Sidebar */
#sidebar{
  position:fixed;
  top:0; left:-260px;
  width:260px;
  height:100%;
  background:#1a1a1a;
  transition:0.4s;
  padding:20px;
  z-index:9999;
}
#sidebar.open{left:0;}
#sidebar a{
  display:flex;
  align-items:center;
  padding:12px;
  margin-bottom:10px;
  border-radius:10px;
  color:white;
  transition:0.2s;
}
#sidebar a:hover{
  background:#3b2f2f;
}
.closeBtn{
  position:absolute;
  right:15px;
  top:15px;
  font-size:22px;
  cursor:pointer;
}

/* Main top bar */
.topbar{
  padding:15px;
  display:flex;
  justify-content:space-between;
  align-items:center;
}
</style>
</head>

<body class="bg-animate">

<!-- Sidebar -->
<div id="sidebar">
  <i class="fas fa-times closeBtn" onclick="toggleSidebar()"></i>

  <a onclick="showSection('dashboard')"><i class="fas fa-home mr-3"></i> Dashboard</a>
  <a onclick="showSection('plans')"><i class="fas fa-coins mr-3"></i> Plans</a>
  <a onclick="showSection('deposit')"><i class="fas fa-wallet mr-3"></i> Deposit</a>
  <a onclick="showSection('withdraw')"><i class="fas fa-money-bill-transfer mr-3"></i> Withdraw</a>
  <a onclick="showSection('profile')"><i class="fas fa-user mr-3"></i> Profile</a>
  <a onclick="logout()"><i class="fas fa-right-from-bracket mr-3"></i> Logout</a>
</div>

<!-- Top Bar -->
<div class="topbar glass">
  <i class="fas fa-bars text-xl" onclick="toggleSidebar()"></i>
  <h1 class="text-xl font-bold">ROCK EARN PRO</h1>
</div>

<!-- Main Content -->
<div id="content" class="p-4 text-white"></div>

<script>
function toggleSidebar(){
  document.getElementById("sidebar").classList.toggle("open");
}

function showSection(sec){
  document.getElementById("content").innerHTML = window[sec];
  toggleSidebar();
}

function logout(){
  localStorage.removeItem("user");
  alert("Logout Successful!");
  location.reload();
}
</script><script>
/* 🔥 PREMIUM DASHBOARD SECTION */
var dashboard = `
<div class='glass p-5 rounded-xl'>
  <h2 class='text-2xl font-bold mb-4 text-center'>Welcome User 👑</h2>

  <div class='grid grid-cols-2 gap-4'>
      <div class='glass text-center p-4'>
          <i class='fas fa-wallet text-3xl mb-2'></i>
          <h3 class='text-lg font-bold'>Balance</h3>
          <p class='text-xl'>Rs 0</p>
      </div>

      <div class='glass text-center p-4'>
          <i class='fas fa-chart-line text-3xl mb-2'></i>
          <h3 class='text-lg font-bold'>Total Profit</h3>
          <p class='text-xl'>Rs 0</p>
      </div>

      <div class='glass text-center p-4'>
          <i class='fas fa-users text-3xl mb-2'></i>
          <h3 class='text-lg font-bold'>Team</h3>
          <p class='text-xl'>0 Users</p>
      </div>

      <div class='glass text-center p-4'>
          <i class='fas fa-bolt text-3xl mb-2'></i>
          <h3 class='text-lg font-bold'>Active Plan</h3>
          <p class='text-xl'>None</p>
      </div>
  </div>
</div>
`;

/* 🔥 PREMIUM PLANS SECTION */
var plans = `
<h2 class='text-2xl font-bold mb-4 text-center'>Investment Plans</h2>
<div class='grid grid-cols-1 gap-4'>

  <div class='glass p-4'>
    <h3 class='text-xl font-bold mb-2'>Basic Plan</h3>
    <p>Minimum: 500</p>
    <p>Daily Profit: 50</p>
    <button class='mt-3 w-full bg-[#3b2f2f] p-2 rounded-lg'>Buy Now</button>
  </div>

  <div class='glass p-4'>
    <h3 class='text-xl font-bold mb-2'>Standard Plan</h3>
    <p>Minimum: 1000</p>
    <p>Daily Profit: 120</p>
    <button class='mt-3 w-full bg-[#3b2f2f] p-2 rounded-lg'>Buy Now</button>
  </div>

  <div class='glass p-4'>
    <h3 class='text-xl font-bold mb-2'>Premium Plan</h3>
    <p>Minimum: 3000</p>
    <p>Daily Profit: 400</p>
    <button class='mt-3 w-full bg-[#3b2f2f] p-2 rounded-lg'>Buy Now</button>
  </div>

</div>
`;

/* 🔥 PREMIUM DEPOSIT SECTION */
var deposit = `
<h2 class='text-2xl font-bold mb-4 text-center'>Deposit</h2>

<div class='glass p-4'>
  <label>Amount</label>
  <input type='number' class='w-full mt-2 mb-3 p-2 glass rounded-lg' placeholder='Enter Amount'>

  <label>Select Method</label>
  <select class='w-full mt-2 p-2 glass rounded-lg'>
      <option>JazzCash</option>
      <option>EasyPaisa</option>
  </select>

  <button class='mt-4 w-full bg-[#3b2f2f] p-2 rounded-lg'>Submit</button>
</div>
`;

/* 🔥 PREMIUM WITHDRAW SECTION */
var withdraw = `
<h2 class='text-2xl font-bold mb-4 text-center'>Withdraw</h2>

<div class='glass p-4'>
  <label>Your Name</label>
  <input type='text' class='w-full mt-2 mb-3 p-2 glass rounded-lg' placeholder='Enter Name'>

  <label>Account Number</label>
  <input type='number' class='w-full mt-2 mb-3 p-2 glass rounded-lg' placeholder='Enter Account Number'>

  <label>Amount</label>
  <input type='number' class='w-full mt-2 mb-3 p-2 glass rounded-lg' placeholder='Enter Amount'>

  <button class='mt-4 w-full bg-[#3b2f2f] p-2 rounded-lg'>Withdraw Now</button>
</div>
`;

/* 🔥 PREMIUM PROFILE SECTION */
var profile = `
<h2 class='text-2xl font-bold mb-4 text-center'>Your Profile</h2>

<div class='glass p-4 text-center'>
  <i class='fas fa-user-circle text-6xl mb-4'></i>
  <p class='text-xl font-bold'>User Name</p>
  <p class='opacity-70'>ID: 1001</p>

  <button class='mt-4 w-full bg-[#3b2f2f] p-2 rounded-lg'>Edit Profile</button>
</div>
`;
</script><script>
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
    users.push(u); 
    localStorage.setItem('reUsers',JSON.stringify(users));
    currentUser=u; 
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard(); 
    showNotif('Account created & logged in');
}

function loginUser(){
    const e=document.getElementById('authEmail').value.trim().toLowerCase();
    const p=document.getElementById('authPass').value;
    const u=users.find(x=>x.email===e && x.pass===p);
    if(!u){showNotif('Invalid credentials'); return;}
    currentUser=u; 
    localStorage.setItem('reCurrent',JSON.stringify(currentUser));
    openDashboard(); 
    showNotif('Welcome '+currentUser.name.split(' ')[0]);
}

function logoutUser(){
    currentUser=null; 
    localStorage.removeItem('reCurrent'); 
    location.reload();
}

// ----------------------
// Dashboard & Sections
// ----------------------
function openDashboard(){
    document.getElementById('authBox').style.display='none';
    document.getElementById('sidebar').style.display='flex';
    document.getElementById('welcomeBox').style.display='block';
    document.getElementById('refreshBtn').style.display='inline-block';
    document.getElementById('userNameTxt').innerText=currentUser.name.split(' ')[0];
    document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
    document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
    if(currentUser.role==='admin'){document.getElementById('adminBtn').style.display='block';}
    document.getElementById('backBtn').style.display='none';
    document.getElementById('contentSection').style.display='block';
}

// ----------------------
// Refresh Fix
// ----------------------
function refreshDashboard(){
    if(currentUser){
        currentUser=users.find(u=>u.email===currentUser.email);
        localStorage.setItem('reCurrent',JSON.stringify(currentUser));
        document.getElementById('balValue').innerText=(currentUser.balance||0)+' PKR';
        document.getElementById('profValue').innerText=(currentUser.profit||0)+' PKR';
        showNotif('Dashboard refreshed');
        closeSection();
    }
}

// ----------------------
// Auto Logout Disabled
// ----------------------
// No auto logout on page refresh — user session persists
if(currentUser) openDashboard();

// ----------------------
// Section Navigation & Content Load
// ----------------------
function openSection(type){
    const content=document.getElementById('sectionContent');
    document.getElementById('backBtn').style.display='inline-block';
    content.innerHTML='';

    if(type==='plans'){
        content.innerHTML=plans;
    } else if(type==='deposit'){
        content.innerHTML=deposit;
    } else if(type==='withdraw'){
        content.innerHTML=withdraw;
    } else if(type==='profile'){
        content.innerHTML=profile;
    } else if(type==='admin' && currentUser.role==='admin'){
        let html='<h2 class="text-xl mb-2">Admin — User Activity</h2>';
        users.forEach(u=>{
            html+=`<div class="glass p-2 mb-2">${u.name} (${u.email}) — Balance: ${u.balance} PKR — Profit: ${u.profit} PKR</div>`;
        });
        content.innerHTML=html;
    } else {
        content.innerHTML='<h3>Coming Soon</h3>';
    }
}

function closeSection(){
    document.getElementById('contentSection').style.display='block';
    document.getElementById('backBtn').style.display='none';
}
</script>
