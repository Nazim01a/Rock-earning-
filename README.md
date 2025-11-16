<ROCK>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn Premium</title>

<!-- Tailwind -->
<script src="https://cdn.tailwindcss.com"></script>
<!-- Icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />

<style>
/* ---------------- PREMIUM ANIMATED BACKGROUND ---------------- */
body {
    margin: 0;
    padding: 0;
    font-family: 'Segoe UI', sans-serif;
    color: white;
    overflow-x: hidden;
    background: black;
}

body::before {
    content:"";
    position: fixed;
    width: 200%;
    height: 200%;
    top: -50%;
    left: -50%;
    background: radial-gradient(circle, #0047ff55, #000000 70%);
    animation: bgMove 10s linear infinite;
    z-index:-1;
}
@keyframes bgMove {
    0% { transform: rotate(0deg) scale(1.2); }
    100% { transform: rotate(360deg) scale(1.2); }
}

/* ---------------- LOGO ANIMATION ---------------- */
.glow {
    font-size: 40px;
    font-weight: bold;
    color: #00bfff;
    text-shadow: 0 0 15px #00bfff, 0 0 30px #0077aa;
    animation: glowAnim 2s infinite alternate;
}
@keyframes glowAnim {
    from { text-shadow: 0 0 10px #00bfff; }
    to   { text-shadow: 0 0 25px #00eaff; }
}

/* Fade Animations */
.fade { animation: fadeIn 1s; }
@keyframes fadeIn { from {opacity:0;} to {opacity:1;} }

/* Sidebar */
#sidebar {
    display:none;
    position:fixed;
    left:0; top:0;
    width:250px;
    height:100vh;
    background:#111;
    padding:20px;
    overflow-y:auto;
    animation: fadeIn 0.5s;
}
#sidebar button {
    width:100%;
    padding:12px;
    text-align:left;
    border-radius:10px;
    margin:5px 0;
    background:#1f1f1f;
    transition:0.3s;
}
#sidebar button:hover { background:#2a2a2a; }

/* Cards */
.plan-card {
    background:#1a1a1a;
    padding:15px;
    border-radius:10px;
    margin:10px 0;
    border:1px solid #222;
    transition:0.3s;
}
.plan-card:hover {
    transform: scale(1.05);
    background:#222;
}

/* Notif */
#notif {
    position:fixed;
    top:20px;
    right:20px;
    padding:12px 20px;
    background:#00bfff;
    color:white;
    border-radius:8px;
    opacity:0;
    transition:0.4s;
}
#notif.show { opacity:1; }

</style>
</head>

<body>

<div id="notif"></div>

<!-- ---------------- LOGIN / SIGNUP ---------------- -->
<div id="authBox" class="flex flex-col justify-center items-center h-screen fade">
    <h1 class="glow mb-4">Rock Earn</h1>

    <input id="authName" class="p-2 m-1 rounded bg-[#222] w-64" placeholder="Your Name (Signup)">
    <input id="authEmail" class="p-2 m-1 rounded bg-[#222] w-64" placeholder="Email">
    <input id="authPass" type="password" class="p-2 m-1 rounded bg-[#222] w-64" placeholder="Password">

    <div class="mt-3">
        <button onclick="signupUser()" class="bg-blue-500 px-4 py-2 rounded mr-2">Sign Up</button>
        <button onclick="loginUser()" class="bg-green-500 px-4 py-2 rounded">Login</button>
    </div>
</div>

<!-- ---------------- SIDEBAR ---------------- -->
<div id="sidebar">
    <h2 class="text-xl mb-4 glow">Rock Earn</h2>

    <button onclick="openSection('plans')"><i class="fas fa-list"></i> Plans</button>
    <button onclick="openSection('deposit')"><i class="fas fa-wallet"></i> Deposit</button>
    <button onclick="openSection('withdraw')"><i class="fas fa-money-bill"></i> Withdraw</button>
    <button onclick="openSection('profit')"><i class="fas fa-chart-line"></i> Profit</button>
    <button onclick="openSection('history')"><i class="fas fa-history"></i> History</button>

    <button id="adminBtn" style="display:none" onclick="openSection('admin')">
        <i class="fas fa-shield-alt"></i> Admin Panel
    </button>

    <button onclick="openProfile()"><i class="fas fa-user"></i> Profile</button>
    <button onclick="logoutUser()"><i class="fas fa-sign-out-alt"></i> Logout</button>
</div>

<!-- ---------------- WELCOME BOX ---------------- -->
<div id="welcomeBox" style="display:none; margin-left:270px; margin-top:20px;" class="fade">
    <h2 class="text-3xl glow" id="userNameTxt"></h2>
    <p class="mt-2">Balance: <span id="balValue">0</span> PKR | Profit: <span id="profValue">0</span> PKR</p>
</div>

<!-- ---------------- CONTENT ---------------- -->
<div id="contentSection" style="margin-left:270px; margin-top:90px;" class="p-4"></div>

<!-- ---------------- PROFILE MODAL ---------------- -->
<div id="profileModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:#000a;" class="flex justify-center items-center fade">
    <div class="bg-[#111] p-5 rounded-xl w-72">
        <h3 class="text-xl mb-3">Edit Profile</h3>
        <input id="profileName" class="p-2 m-1 rounded bg-[#222] w-full">
        <input id="profileEmail" class="p-2 m-1 rounded bg-[#222] w-full">
        <input id="profilePass" type="password" class="p-2 m-1 rounded bg-[#222] w-full" placeholder="New Password">
        <div class="mt-3">
            <button onclick="updateProfile()" class="bg-blue-500 px-3 py-2 rounded">Update</button>
            <button onclick="closeProfile()" class="bg-red-500 px-3 py-2 rounded">Close</button>
        </div>
    </div>
</div>

<script>
/* ---------------- STORAGE ---------------- */
let users = JSON.parse(localStorage.getItem("reUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("reCurrent")) || null;

/* Default Admin */
if (!users.find(u => u.email === "admin@rockearn.com")) {
    users.push({
        name:"Administrator",
        email:"admin@rockearn.com",
        pass:"admin123",
        role:"admin",
        balance:0,
        profit:0,
        deposits:[],
        withdrawals:[],
        plans:[]
    });
    localStorage.setItem("reUsers", JSON.stringify(users));
}

/* ---------------- NOTIF ---------------- */
function showNotif(msg){
    const n=document.getElementById("notif");
    n.innerText=msg;
    n.classList.add("show");
    setTimeout(()=>n.classList.remove("show"),2500);
}

/* ---------------- SIGNUP ---------------- */
function signupUser(){
    let n = authName.value.trim();
    let e = authEmail.value.toLowerCase().trim();
    let p = authPass.value.trim();

    if(!n||!e||!p) return showNotif("Fill all fields!");
    if(users.find(u=>u.email===e)) return showNotif("Email exists!");

    let newU = {
        name:n,email:e,pass:p,
        role:"user",
        balance:0,profit:0,
        deposits:[],withdrawals:[],plans:[]
    };

    users.push(newU);
    currentUser=newU;

    localStorage.setItem("reUsers",JSON.stringify(users));
    localStorage.setItem("reCurrent",JSON.stringify(currentUser));

    openDashboard();
    showNotif("Account created");
}

/* ---------------- LOGIN ---------------- */
function loginUser(){
    let e = authEmail.value.toLowerCase().trim();
    let p = authPass.value.trim();

    let u = users.find(x=>x.email===e && x.pass===p);
    if(!u) return showNotif("Invalid login!");

    currentUser=u;
    localStorage.setItem("reCurrent",JSON.stringify(u));

    openDashboard();
    showNotif("Welcome " + u.name);
}

/* ---------------- LOGOUT ---------------- */
function logoutUser(){
    currentUser=null;
    localStorage.removeItem("reCurrent");
    location.reload();
}

/* ---------------- DASHBOARD ---------------- */
function openDashboard(){
    authBox.style.display="none";
    sidebar.style.display="block";
    welcomeBox.style.display="block";
    contentSection.innerHTML="";

    userNameTxt.innerText=currentUser.name;
    balValue.innerText=currentUser.balance+" PKR";
    profValue.innerText=currentUser.profit+" PKR";

    if(currentUser.role==="admin") adminBtn.style.display="block";
}

/* ---------------- OPEN SECTIONS ---------------- */
const plans = [
    {name:"Basic",amount:200,daily:30,days:20},
    {name:"Starter",amount:500,daily:75,days:20},
    {name:"Pro",amount:1000,daily:180,days:20},
    {name:"Silver",amount:2000,daily:350,days:30},
    {name:"Gold",amount:3000,daily:550,days:30},
    {name:"Diamond",amount:7000,daily:1350,days:50},
    {name:"VIP",amount:10000,daily:2000,days:60}
];

function openSection(type){
    let c = contentSection;
    c.innerHTML="";

    if(type==="plans"){
        plans.forEach(p=>{
            c.innerHTML+=`
                <div class="plan-card">
                    <b>${p.name}</b><br>
                    Amount: ${p.amount} PKR<br>
                    Daily: ${p.daily} PKR<br>
                    Days: ${p.days}<br>
                    <button class="bg-blue-600 px-3 py-1 mt-2 rounded" 
                        onclick="openDeposit(${p.amount},'${p.name}',${p.daily},${p.days})">
                        Buy Now
                    </button>
                </div>`;
        });
    }

    else if(type==="deposit"){ openDeposit(); }

    else if(type==="withdraw"){ openWithdraw(); }

    else if(type==="profit"){
        c.innerHTML=`<h2 class='text-xl'>Total Profit: ${currentUser.profit} PKR</h2>`;
    }

    else if(type==="history"){
        let h="<h2 class='text-xl mb-3'>History</h2>";

        currentUser.deposits.forEach(d=>{
            h+=`<p>Deposit: ${d.amount} - ${d.plan}</p>`;
        });

        currentUser.withdrawals.forEach(w=>{
            h+=`<p>Withdraw: ${w.amount} | ${w.method}</p>`;
        });

        c.innerHTML=h;
    }

    else if(type==="admin" && currentUser.role==="admin"){
        let h="<h2 class='text-xl mb-3'>Admin Panel</h2>";
        users.forEach(u=>{
            h+=`<p>${u.name} - Balance: ${u.balance} PKR</p>`;
        });
        c.innerHTML=h;
    }
}

/* ---------------- DEPOSIT ---------------- */
function openDeposit(amount=0,name="",daily=0,days=0){
    contentSection.innerHTML=`
        <h2 class="text-xl mb-2">Deposit - ${name}</h2>

        <p>Amount: <b>${amount} PKR</b></p>

        <p>JazzCash: <b>03705519562</b>
            <button onclick="copyText('03705519562')" class="bg-blue-600 px-2 py-1 rounded">Copy</button>
        </p>

        <p>EasyPaisa: <b>03379827882</b>
            <button onclick="copyText('03379827882')" class="bg-blue-600 px-2 py-1 rounded">Copy</button>
        </p>

        <input id="depositTxn" class="p-2 m-1 rounded bg-[#222]" placeholder="Transaction ID">
        <input id="depositProof" type="file" class="p-2 m-1 rounded bg-[#222]"><br>

        <button class="bg-green-600 px-4 py-2 rounded mt-2"
        onclick="confirmDeposit(${amount},'${name}',${daily},${days})">Submit</button>
    `;
}

function confirmDeposit(amount,name,daily,days){
    let txn = depositTxn.value.trim();
    let proof = depositProof.files[0];

    if(!txn || !proof) return showNotif("Upload proof + txn ID!");

    currentUser.balance += amount;
    currentUser.profit += daily;

    currentUser.deposits.push({
        plan:name, amount, txn, proof:proof.name, status:"approved"
    });

    users = users.map(u => u.email===currentUser.email ? currentUser : u);

    localStorage.setItem("reUsers",JSON.stringify(users));
    localStorage.setItem("reCurrent",JSON.stringify(currentUser));

    showNotif("Deposit successful!");
    openSection("plans");
}

/* ---------------- WITHDRAW ---------------- */
function openWithdraw(){
    contentSection.innerHTML=`
        <h2 class="text-xl mb-2">Withdraw</h2>

        <input id="wName" class="p-2 m-1 rounded bg-[#222]" placeholder="Name">
        <input id="wAcc" class="p-2 m-1 rounded bg-[#222]" placeholder="Account Number">
        <input id="wAmt" class="p-2 m-1 rounded bg-[#222]" placeholder="Amount">

        <select id="wMethod" class="p-2 m-1 rounded bg-[#222]">
            <option>JazzCash</option>
            <option>EasyPaisa</option>
            <option>Bank</option>
        </select>

        <button class="bg-green-600 px-4 py-2 rounded mt-3"
        onclick="confirmWithdraw()">Submit</button>
    `;
}

function confirmWithdraw(){
    let n=wName.value.trim();
    let a=wAcc.value.trim();
    let amt=parseInt(wAmt.value.trim());
    let m=wMethod.value;

    if(!n||!a||!amt) return showNotif("Fill all fields!");
    if(amt>currentUser.balance) return showNotif("Low balance!");

    currentUser.balance -= amt;
    currentUser.withdrawals.push({name:n,account:a,amount:amt,method:m});

    users = users.map(u => u.email===currentUser.email ? currentUser : u);

    localStorage.setItem("reUsers",JSON.stringify(users));
    localStorage.setItem("reCurrent",JSON.stringify(currentUser));

    showNotif("Withdraw successful!");
}

/* ---------------- PROFILE ---------------- */
function openProfile(){
    profileModal.style.display="flex";
    profileName.value=currentUser.name;
    profileEmail.value=currentUser.email;
}
function closeProfile(){ profileModal.style.display="none"; }

function updateProfile(){
    if(profileName.value.trim()) currentUser.name = profileName.value.trim();
    if(profileEmail.value.trim()) currentUser.email = profileEmail.value.trim();
    if(profilePass.value.trim()) currentUser.pass = profilePass.value.trim();

    users = users.map(u => u.email===currentUser.email ? currentUser : u);

    localStorage.setItem("reUsers",JSON.stringify(users));
    localStorage.setItem("reCurrent",JSON.stringify(currentUser));

    showNotif("Profile updated!");
    closeProfile();
}

/* ---------------- COPY ---------------- */
function copyText(t){
    navigator.clipboard.writeText(t);
    showNotif("Copied!");
}

/* ---------------- AUTO LOGIN ---------------- */
if(currentUser) openDashboard();
</script>

</body>
</html>
