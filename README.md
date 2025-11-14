<ROCK><html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Rock Earn Official — Pro Final Enhanced</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:#0d0f1d;color:white;}
.card{background:#15172b;border-radius:18px;padding:20px;box-shadow:0 0 20px #0005;transition:0.3s;}
.card:hover{transform:scale(1.02);}
.btn{background:#4f46e5;padding:10px 16px;border-radius:10px;font-weight:600;transition:0.2s;}
.btn:hover{background:#4338ca;}
</style>
</head>
<body class="p-4"><!-- AUTH BOX --><div id="authBox" class="card mb-6">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" class="w-full p-2 text-black rounded mb-2" />
  <input id="authEmail" placeholder="Email" class="w-full p-2 text-black rounded mb-2" />
  <input id="authPass" type="password" placeholder="Password" class="w-full p-2 text-black rounded mb-2" /><button onclick="signupUser()" class="btn w-full mb-2">Sign Up</button> <button onclick="loginUser()" class="btn w-full bg-green-600 hover:bg-green-700">Login</button>

  <p onclick="forgotPassword()" class="text-blue-400 text-center mt-3 cursor-pointer">Forgot Password?</p>
</div><!-- WELCOME + LOGOUT --><div id="welcomeBox" style="display:none;" class="card mb-4 p-4 text-center"></div><!-- DASHBOARD --><div id="dashboard" style="display:none;">  <!-- PLANS -->  <h2 class="text-xl font-bold mb-3">Our Plans</h2>
  <div class="grid grid-cols-1 gap-4" id="planList"></div>  <!-- SIDE PANEL (DEPOSIT) -->  <div id="depositPanel" class="card mt-6 hidden">
    <h2 class="text-xl font-bold mb-4">Deposit</h2>
    <p class="mb-2">Amount: <span id="depAmount"></span> PKR</p><h3 class="font-bold mb-1">JazzCash</h3>
<div class="flex mb-3">
  <input id="jcNum" value="03705519562" class="p-2 text-black w-full rounded-l" />
  <button onclick="copyText('jcNum')" class="btn rounded-none rounded-r">Copy</button>
</div>

<h3 class="font-bold mb-1">EasyPaisa</h3>
<div class="flex mb-3">
  <input id="epNum" value="03379827882" class="p-2 text-black w-full rounded-l" />
  <button onclick="copyText('epNum')" class="btn rounded-none rounded-r">Copy</button>
</div>

<input id="trxId" placeholder="Transaction ID" class="w-full p-2 text-black rounded mb-3" />
<button class="btn w-full" onclick="confirmDeposit()">Upload Proof</button>

  </div>  <!-- WITHDRAW -->  <div id="withdrawBox" class="card mt-6">
    <h2 class="text-xl font-bold mb-2">Withdraw</h2>
    <input id="wdAmount" placeholder="Amount" class="w-full p-2 text-black rounded mb-2" />
    <select id="wdMethod" class="w-full p-2 text-black rounded mb-2">
      <option>JazzCash</option>
      <option>EasyPaisa</option>
      <option>Bank</option>
    </select>
    <input id="wdNum" placeholder="Account Number" class="w-full p-2 text-black rounded mb-2" />
    <button onclick="withdrawNow()" class="btn w-full bg-green-600 hover:bg-green-700">Withdraw Now</button>
  </div>  <!-- COMPANY DETAILS -->  <div class="card mt-6">
    <h2 class="text-xl font-bold mb-2">Company Details</h2>
    <p>Founded: 2018</p>
    <p>Working with: Crypto & Binance (as soon)</p>
    <p>Location: 3500 Deer Creek Road, Palo Alto, California, USA</p>
  </div>  <!-- PROFILE -->  <div class="card mt-6">
    <h2 class="text-xl font-bold mb-2">My Profile</h2>
    <p id="profName"></p>
    <p id="profEmail"></p>
  </div></div><script>
let users = JSON.parse(localStorage.getItem("reUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("reCurrent")) || null;
function validateEmail(m){return /^\S+@\S+\.\S+$/.test(m);}   
function signupUser(){
  let n=document.getElementById('authName').value.trim();
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  if(!n||!e||!p) return alert('Fill all fields');
  if(!validateEmail(e)) return alert('Invalid email');
  if(users.find(u=>u.email===e)) return alert('Email already registered');
  let u={name:n,email:e,pass:p};
  users.push(u);
  localStorage.setItem('reUsers',JSON.stringify(users));
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(u));
  openDashboard();
}
function loginUser(){
  let e=document.getElementById('authEmail').value.trim();
  let p=document.getElementById('authPass').value.trim();
  let u=users.find(x=>x.email===e && x.pass===p);
  if(!u) return alert('Invalid credentials');
  currentUser=u;
  localStorage.setItem('reCurrent',JSON.stringify(u));
  openDashboard();
}
function forgotPassword(){
  let e=prompt('Enter your email');
  if(!validateEmail(e)) return alert('Invalid email');
  let u=users.find(x=>x.email===e);
  if(!u) return alert('Email not found');
  alert('Your password: '+u.pass);
}
function openDashboard(){
  document.getElementById('authBox').style.display='none';
  document.getElementById('welcomeBox').style.display='block';
  document.getElementById('dashboard').style.display='block';
  document.getElementById('welcomeBox').innerHTML=`<h2 class='text-2xl font-bold mb-2'>Welcome, ${currentUser.name} 💖</h2><button onclick="logoutUser()" class="btn bg-red-600 hover:bg-red-700 w-full">Logout</button>`;
  document.getElementById('profName').innerText="Name: "+currentUser.name;
  document.getElementById('profEmail').innerText="Email: "+currentUser.email;
  loadPlans();
  notify('Welcome back, '+currentUser.name+' ❤️');
}
function logoutUser(){localStorage.removeItem('reCurrent');location.reload();}
let plans=[{name:"Starter",amount:180},{name:"Basic",amount:350},{name:"Pro",amount:500},{name:"Premium",amount:1000},{name:"Ultra",amount:2500},{name:"Mega",amount:5000},{name:"Elite",amount:7000}];
function loadPlans(){
  let box=document.getElementById('planList'); box.innerHTML="";
  plans.forEach(p=>{box.innerHTML+=`<div class='card'><h3 class='text-xl font-bold mb-1'>${p.name}</h3><p class='mb-3'>Price: ${p.amount} PKR</p><button class='btn' onclick="openDeposit(${p.amount})">Buy Now</button></div>`;});
}
function openDeposit(a){document.getElementById('depositPanel').classList.remove('hidden');document.getElementById('depAmount').innerText=a;}
function copyText(id){let i=document.getElementById(id);i.select();navigator.clipboard.writeText(i.value);alert('Copied');}
function confirmDeposit(){let t=document.getElementById('trxId').value.trim();if(!t)return alert('Enter Transaction ID');alert('Deposit Submitted');}
function withdrawNow(){let a=document.getElementById('wdAmount').value;let m=document.getElementById('wdMethod').value;let n=document.getElementById('wdNum').value;if(!a||!m||!n)return alert('Fill all fields');alert('Withdrawal Request Sent');}

// EXTRA ENHANCED FEATURES
function notify(msg){let box=document.createElement('div');box.className='fixed top-4 right-4 bg-blue-600 p-3 rounded shadow text-white animate-pulse';box.innerText=msg;document.body.appendChild(box);setTimeout(()=>box.remove(),4000);}
if(currentUser){notify('Welcome back, '+currentUser.name+' ❤️');}

// Real-time Earnings Simulator
let totalEarn=0;
let earnBox=document.createElement('div');
earnBox.className='fixed bottom-4 right-4 bg-green-600 p-4 rounded-xl shadow-xl text-black font-bold';
document.body.appendChild(earnBox);
setInterval(()=>{
  totalEarn+=Math.floor(Math.random()*20)+5;
  earnBox.innerHTML=`Daily Profit: ${Math.floor(totalEarn/30)} PKR<br>Total Earnings: ${totalEarn} PKR`;
},5000);

// Loader
let loader=document.createElement('div');loader.style=`position:fixed;top:0;left:0;width:100%;height:100%;background:#000c;display:flex;align-items:center;justify-content:center;font-size:28px;font-weight:bold;z-index:9999;backdrop-filter:blur(4px);`;loader.innerHTML='Loading...';document.body.appendChild(loader);setTimeout(()=>loader.remove(),1200);
if(currentUser) openDashboard();
</script></body>
</html>
