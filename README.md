<!DOCTYPE html>
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

.sidebar{position:fixed;top:0;left:0;width:80px;height:100vh;background:rgba(20,20,20,0.95);
display:flex;flex-direction:column;align-items:center;padding-top:20px;gap:15px;z-index:10;display:none;}

.icon-btn{display:flex;flex-direction:column;align-items:center;padding:12px;border-radius:15px;cursor:pointer;
transition:0.3s;background:#111;color:#ccc;box-shadow:0 0 5px #000;}
.icon-btn:hover{transform:scale(1.1);box-shadow:0 0 15px #0ff,0 0 25px #0ff;}
.icon-name{margin-top:6px;font-size:12px;color:#ccc;text-shadow:0 0 3px #000;text-align:center;}

#welcomeBox{position:fixed;top:20px;left:100px;right:20px;padding:25px;border-radius:20px;background:rgba(0,0,0,0.8);
backdrop-filter:blur(12px);text-align:center;display:none;animation:fadeIn 1.5s;}
#welcomeBox h2{color:white;font-size:26px;font-weight:800;animation:glowText 2.5s infinite alternate;}

.stats{display:flex;justify-content:center;gap:30px;margin-top:15px;flex-wrap:wrap;}
.stat-card{background: rgba(0,255,255,0.05); padding: 18px 25px; border-radius: 15px; box-shadow: 0 0 5px #0ff,0 0 10px #0ff; transition: 0.4s;}
.stat-label{font-size:12px;color:#0ff;letter-spacing:1px;margin-bottom:5px;}
.stat-value{font-size:18px;font-weight:700;color:white;}

#contentSection{position:fixed;top:160px;left:100px;right:20px;bottom:20px;background:rgba(0,0,0,0.85);
backdrop-filter:blur(15px);border-radius:20px;padding:20px;overflow-y:auto;display:none;z-index:999;}
#backBtn{position:absolute;top:10px;left:20px;background:#ff0044;padding:8px 12px;border:none;border-radius:10px;font-weight:700;
cursor:pointer;transition:0.3s;}
#backBtn:hover{transform:scale(1.05);background:#ff3366;}

#notif{position:fixed;top:20px;right:-400px;background:linear-gradient(45deg,#0ff,#0ffcc0);padding:15px 25px;border-radius:12px;
box-shadow:0 0 20px #0ff;color:white;font-weight:600;transition:0.5s;z-index:9999;}
#notif.show{right:20px;}

input,select{padding:10px;border-radius:12px;width:100%;margin-bottom:10px;background:#1b1e2f;color:white;border:none;transition:0.3s;}
input:focus, select:focus{outline:none;box-shadow:0 0 12px #0ff;}

.btn{background:#111;color:#0ff;padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;
box-shadow:0 0 5px #0ff,0 0 10px #0ff;transition:0.3s;}
.btn:hover{transform:scale(1.08);box-shadow:0 0 20px #0ff,0 0 40px #0ff;}

.logout-btn{position:fixed;bottom:80px;left:50%;transform:translateX(-50%);background:#ff0044;color:white;
padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 10px #ff3366;transition:0.3s;display:none;}

.admin-btn{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#00ff66;color:#000;
padding:12px 18px;border-radius:12px;font-weight:700;cursor:pointer;box-shadow:0 0 10px #00ff66;display:none;}

@keyframes glowText{0%{text-shadow:0 0 5px #000,0 0 10px #0ff;}50%{text-shadow:0 0 12px #000,0 0 25px #0ff;}100%{text-shadow:0 0 5px #000,0 0 10px #0ff;}}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}

@media(max-width:1024px){
#welcomeBox{left:20px;right:20px;}
#contentSection{top:220px;left:20px;right:20px;bottom:20px;}
.sidebar{width:100%;height:auto;flex-direction:row;justify-content:space-around;padding:10px;display:flex;position:relative;}
}
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

</div><!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn – Final Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>

<style>
body{
    font-family:'Segoe UI',sans-serif;
    background:linear-gradient(120deg,#0f1123,#1c1f3b);
    color:white;
}

/* Smooth Animations */
.fadeIn{
    animation:fadeIn .6s ease;
}
@keyframes fadeIn{
    from{opacity:0;transform:translateY(10px);}
    to{opacity:1;transform:translateY(0);}
}

/* Side Panel */
#sidebar{
    transition:0.4s;
    transform:translateX(-100%);
}
#sidebar.show{
    transform:translateX(0);
}
</style>
</head>

<body class="overflow-x-hidden">

<!-- LOGIN PAGE -->
<div id="loginPage" class="min-h-screen flex flex-col justify-center items-center p-4 fadeIn">
    <h1 class="text-3xl font-bold mb-4">Rock Earn Login</h1>

    <input id="loginEmail" type="text" placeholder="Email" class="w-72 p-2 mb-3 rounded bg-white/20">
    <input id="loginPass" type="password" placeholder="Password" class="w-72 p-2 mb-3 rounded bg-white/20">

    <button onclick="login()" class="w-72 p-2 bg-blue-600 rounded mt-2">Login</button>

    <p class="mt-3">Create new account? 
        <span onclick="showSignup()" class="text-blue-300 underline cursor-pointer">Sign Up</span>
    </p>
</div>

<!-- SIGNUP PAGE -->
<div id="signupPage" class="hidden min-h-screen flex flex-col justify-center items-center p-4 fadeIn">
    <h1 class="text-3xl font-bold mb-4">Create Account</h1>

    <input id="signupName" type="text" placeholder="Full Name" class="w-72 p-2 mb-3 rounded bg-white/20">
    <input id="signupEmail" type="text" placeholder="Email" class="w-72 p-2 mb-3 rounded bg-white/20">
    <input id="signupPass" type="password" placeholder="Password" class="w-72 p-2 mb-3 rounded bg-white/20">

    <button onclick="signup()" class="w-72 p-2 bg-green-600 rounded mt-2">Sign Up</button>

    <p class="mt-3">Already have account?
        <span onclick="showLogin()" class="text-blue-300 underline cursor-pointer">Login</span>
    </p>
</div>

<!-- DASHBOARD -->
<div id="dashboard" class="hidden fadeIn">

    <!-- TOP BAR -->
    <div class="p-4 flex justify-between items-center bg-white/10 backdrop-blur">
        <i class="fas fa-bars text-2xl cursor-pointer" onclick="toggleSidebar()"></i>
        <h2 class="text-xl font-bold">Rock Earn Dashboard</h2>
        <span></span>
    </div>

    <!-- WELCOME -->
    <div class="p-4">
        <h1 class="text-2xl font-bold" id="welcomeUser"></h1>
        <p class="opacity-70">Premium Animated Dashboard</p>
    </div>

    <!-- ICON GRID -->
    <div class="grid grid-cols-3 gap-4 p-4 text-center text-sm">

        <div onclick="openPlans()" class="p-4 bg-white/10 rounded-xl cursor-pointer">
            <i class="fa-solid fa-rocket text-3xl mb-2"></i>
            Plans
        </div>

        <div onclick="openDeposit()" class="p-4 bg-white/10 rounded-xl cursor-pointer">
            <i class="fa-solid fa-wallet text-3xl mb-2"></i>
            Deposit
        </div>

        <div onclick="openWithdraw()" class="p-4 bg-white/10 rounded-xl cursor-pointer">
            <i class="fas fa-money-check text-3xl mb-2"></i>
            Withdraw
        </div>

        <div onclick="openProfit()" class="p-4 bg-white/10 rounded-xl cursor-pointer">
            <i class="fa-solid fa-chart-line text-3xl mb-2"></i>
            Daily Profit
        </div>

        <div onclick="openHistory()" class="p-4 bg-white/10 rounded-xl cursor-pointer">
            <i class="fa-solid fa-clock-rotate-left text-3xl mb-2"></i>
            History
        </div>

        <div onclick="openSettings()" class="p-4 bg-white/10 rounded-xl cursor-pointer">
            <i class="fa-solid fa-gear text-3xl mb-2"></i>
            Settings
        </div>
    </div>

</div>

<!-- SIDEBAR -->
<div id="sidebar" class="fixed left-0 top-0 w-64 h-full bg-black/80 p-6">
    <div class="flex justify-between mb-6">
        <h2 class="text-xl font-bold">Menu</h2>
        <i class="fa-solid fa-x text-xl cursor-pointer" onclick="toggleSidebar()"></i>
    </div>

    <p onclick="openPlans()" class="mb-4 cursor-pointer">🔥 Plans</p>
    <p onclick="openDeposit()" class="mb-4 cursor-pointer">💰 Deposit</p>
    <p onclick="openWithdraw()" class="mb-4 cursor-pointer">🏧 Withdraw</p>
    <p onclick="openHistory()" class="mb-4 cursor-pointer">📜 History</p>
    <p onclick="logout()" class="mt-10 text-red-400 cursor-pointer">Logout</p>
</div>

<script>
// Show signup
function showSignup(){
    loginPage.classList.add("hidden");
    signupPage.classList.remove("hidden");
}

// Show login
function showLogin(){
    signupPage.classList.add("hidden");
    loginPage.classList.remove("hidden");
}

// SIGNUP SYSTEM
function signup(){
    let user = {
        name: signupName.value,
        email: signupEmail.value,
        pass: signupPass.value
    };
    localStorage.setItem("rockUser", JSON.stringify(user));
    alert("Account Created!");
    showLogin();
}

// LOGIN SYSTEM (FIXED)
function login(){
    let user = JSON.parse(localStorage.getItem("rockUser"));
    if(!user){
        alert("Account not found!");
        return;
    }

    if(loginEmail.value === user.email && loginPass.value === user.pass){
        localStorage.setItem("loggedIn", "yes");
        loadDashboard();
    } else {
        alert("Wrong Email or Password");
    }
}

// KEEP LOGGED IN AFTER REFRESH
window.onload = function(){
    if(localStorage.getItem("loggedIn") === "yes"){
        loadDashboard();
    }
};

// LOAD DASHBOARD
function loadDashboard(){
    loginPage.classList.add("hidden");
    signupPage.classList.add("hidden");
    dashboard.classList.remove("hidden");

    let user = JSON.parse(localStorage.getItem("rockUser"));
    welcomeUser.innerHTML = "Welcome, " + user.name;
}

// LOGOUT
function logout(){
    localStorage.setItem("loggedIn","no");
    dashboard.classList.add("hidden");
    loginPage.classList.remove("hidden");
}

// SIDEBAR
function toggleSidebar(){
    document.getElementById("sidebar").classList.toggle("show");
}

// ICON ACTIONS
function openPlans(){ alert("Plans page will open."); }
function openDeposit(){ alert("Deposit section."); }
function openWithdraw(){ alert("Withdraw section."); }
function openProfit(){ alert("Daily profit section."); }
function openHistory(){ alert("History."); }
function openSettings(){ alert("Settings."); }

</script>

</body>
</html>
