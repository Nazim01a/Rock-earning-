<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Official</title>
<script src="https://cdn.tailwindcss.com"></script>

<style>
body { font-family: 'Segoe UI', sans-serif; background:#0f1123; color:white; }
.card { background:#16182e; border-radius:14px; padding:18px; color:white; }
.btn { background:#5d5fef; padding:10px 18px; border-radius:10px; color:white; font-weight:600; display:inline-block; }
.btn:hover { opacity:0.8; }
input, select {
    background:#1e213d; border:none; padding:10px;
    width:100%; border-radius:10px; color:white; margin-top:8px;
}
</style>

</head>

<body>

<!-- SIGNUP + LOGIN PAGE -->
<div id="authPage" class="p-5 hidden">
<h2 class="text-2xl font-bold mb-4">Sign Up / Login</h2>

<input id="username" placeholder="Enter Username">
<input id="email" placeholder="Enter Gmail">
<input id="password" placeholder="Enter Password" type="password">

<button class="btn mt-4 w-full" onclick="login()">Continue</button>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="hidden p-4">

<h2 class="text-3xl font-bold mb-4">Rock Earn Dashboard</h2>

<!-- TOP BUTTONS -->
<div class="grid grid-cols-3 gap-3 mb-4">
  <button class="btn w-full" onclick="showShare()">Share Link</button>
  <button class="btn w-full" onclick="showProfile()">Profile</button>
  <button class="btn w-full" onclick="showSupport()">Support</button>
</div>

<h3 class="text-xl font-bold mb-2">Our Plans</h3>
<div id="plansArea"></div>

<button onclick="logout()" class="btn w-full mt-6 bg-red-600">Logout</button>

</div>

<!-- PLAN PAGE -->
<div id="planPage" class="hidden p-4"></div>

<!-- DEPOSIT PAGE -->
<div id="depositPage" class="hidden p-4">
<h2 class="text-2xl font-bold mb-3">Deposit</h2>

<label>Choose Method:</label>
<select id="depositMethod">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
</select>

<div class="mt-3">
<p><b>JazzCash Number:</b> 03705519562</p>
<p><b>EasyPaisa Number:</b> 03379827882</p>
</div>

<input id="trxid" placeholder="Enter Transaction ID">
<input id="proof" type="file">

<button class="btn w-full mt-4" onclick="submitted()">Submit Deposit</button>
</div>

<!-- WITHDRAW PAGE -->
<div id="withdrawPage" class="hidden p-4">
<h2 class="text-2xl font-bold mb-3">Withdrawal</h2>

<select id="withdrawMethod">
<option value="jazz">JazzCash</option>
<option value="easy">EasyPaisa</option>
</select>

<input id="wNumber" placeholder="Enter Your Number">
<input id="wAmount" placeholder="Enter Amount">

<button class="btn w-full mt-4" onclick="submitted()">Submit Withdrawal</button>
</div>

<!-- SHARE PAGE -->
<div id="sharePage" class="hidden p-4">
<h2 class="text-xl font-bold mb-3">Share Your Link</h2>
<input id="shareLink" value="https://yourwebsite.com?ref=ROCK123">
<button class="btn mt-3 w-full" onclick="copyLink()">Copy Link</button>
</div>

<!-- PROFILE PAGE -->
<div id="profilePage" class="hidden p-4">
<h2 class="text-xl font-bold mb-3">Your Profile</h2>
<p id="pUser"></p>
<p id="pMail"></p>
</div>

<!-- SUPPORT PAGE -->
<div id="supportPage" class="hidden p-4">
<h2 class="text-2xl font-bold mb-3">Customer Service</h2>
<p>WhatsApp: <b>0318-7102225</b></p>

<h2 class="text-xl font-bold mt-4 mb-2">Company Details</h2>
<p>Company: <b>Rock Earn Digital Pvt Ltd</b></p>
<p>Launched: <b>2024</b></p>
<p>Service Hours: <b>24/7</b></p>
<p>Head Office: <b>Lahore, Pakistan</b></p>
</div>

<script>

// ---- 20+ DAYS PLANS UPGRADED ----
const plans = [
 {name:"Starter Plan", price:180, profit:20, days:20},
 {name:"Silver Plan", price:350, profit:40, days:25},
 {name:"Gold Plan", price:600, profit:70, days:30},
 {name:"Pro Plan", price:1200, profit:150, days:35},
 {name:"Advance Plan", price:2500, profit:320, days:40},
 {name:"Ultra Plan", price:4000, profit:520, days:45},
 {name:"Premium Plan", price:7000, profit:900, days:50}
];

// -------- AUTO LOGIN SYSTEM --------
window.onload = function(){
    let savedUser = localStorage.getItem("rockUser");
    if(savedUser){
        document.getElementById("authPage").style.display="none";
        document.getElementById("dashboard").style.display="block";
        loadUser(JSON.parse(savedUser));
        loadPlans();
    } else {
        document.getElementById("authPage").style.display="block";
    }
}

function login(){
 let user = {
   name: document.getElementById("username").value,
   email: document.getElementById("email").value
 };

 localStorage.setItem("rockUser", JSON.stringify(user));

 loadUser(user);

 document.getElementById("authPage").style.display="none";
 document.getElementById("dashboard").style.display="block";
 loadPlans();
}

function loadUser(user){
 document.getElementById("pUser").innerText = "Username: " + user.name;
 document.getElementById("pMail").innerText = "Gmail: " + user.email;
}

// ---- LOGOUT ----
function logout(){
 localStorage.removeItem("rockUser");
 location.reload();
}

// ---- LOAD PLANS ----
function loadPlans(){
 let html="";
 plans.forEach((p,i)=>{
   html += `
   <div class='card mb-3'>
     <h3 class='text-xl font-bold mb-1'>${p.name}</h3>
     <p>Price: <b>${p.price} PKR</b></p>
     <p>Daily Profit: <b>${p.profit} Rs/day</b></p>
     <p>Duration: <b>${p.days} Days</b></p>
     <p>Total Return: <b>${p.profit * p.days} Rs</b></p>
     <button class='btn mt-2' onclick='openPlan(${i})'>View Details</button>
   </div>`;
 });
 document.getElementById("plansArea").innerHTML = html;
}

// ---- PLAN DETAILS ----
function openPlan(i){
 let p = plans[i];
 hideAll();
 document.getElementById("planPage").style.display="block";
 document.getElementById("planPage").innerHTML = `
 <h2 class='text-2xl font-bold mb-2'>${p.name}</h2>
 <p>Price: ${p.price} PKR</p>
 <p>Daily Profit: ${p.profit} Rs</p>
 <p>Duration: ${p.days} Days</p>
 <p>Total Profit: ${p.profit * p.days} Rs</p>

 <button class='btn w-full mt-4' onclick='openDeposit()'>Deposit</button>
 <button class='btn w-full mt-2' onclick='openWithdraw()'>Withdraw</button>
 <button class='btn w-full mt-2 bg-gray-500' onclick='backHome()'>Back</button>
 `;
}

// ---- NAVIGATION ----
function openDeposit(){ hideAll(); document.getElementById("depositPage").style.display="block"; }
function openWithdraw(){ hideAll(); document.getElementById("withdrawPage").style.display="block"; }
function showShare(){ hideAll(); document.getElementById("sharePage").style.display="block"; }
function showProfile(){ hideAll(); document.getElementById("profilePage").style.display="block"; }
function showSupport(){ hideAll(); document.getElementById("supportPage").style.display="block"; }

function submitted(){ alert("Submitted Successfully!"); }

function copyLink(){
 let link = document.getElementById("shareLink");
 link.select(); document.execCommand("copy");
 alert("Link Copied!");
}

function backHome(){ hideAll(); document.getElementById("dashboard").style.display="block"; }

function hideAll(){
 document.querySelectorAll("body > div").forEach(d=>d.style.display="none");
}

</script>

</body>
</html>
