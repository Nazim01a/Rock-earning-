<ROCK><html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Rock Earn Ultra Premium</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{
  font-family:'Segoe UI',sans-serif;
  background: linear-gradient(120deg,#0b0f1a,#08142a);
  color:white;
  overflow-x:hidden;
}
.neon-card{
  transition:0.4s;
  border:1px solid rgba(255,255,255,0.1);
  background:rgba(255,255,255,0.05);
  backdrop-filter:blur(15px);
  cursor:pointer;
}
.neon-card:hover{
  transform:scale(1.05);
  box-shadow:0 0 25px rgba(0,150,255,0.7);
}
#sidePanel{
  position:fixed;
  top:0;
  right:-400px;
  width:350px;
  height:100vh;
  background:#0f172a;
  padding:20px;
  transition:0.4s;
  overflow-y:auto;
  z-index:999;
}
.btn{transition:0.3s;}
.btn:hover{transform:scale(1.05);}
@keyframes fadeInUp{from{opacity:0;transform:translateY(20px);}to{opacity:1;transform:translateY(0);}}
.animate-fade{animation:fadeInUp 0.8s ease forwards;}
</style>
</head>
<body class="p-4"><!-- AUTH SYSTEM --><div id="authBox" class="card mb-6 p-4 animate-fade">
  <h2 class="text-2xl font-bold mb-4 text-center">Login / Sign Up</h2>
  <input id="authName" placeholder="Full Name" class="w-full p-2 text-black rounded mb-2" />
  <input id="authEmail" placeholder="Email" class="w-full p-2 text-black rounded mb-2" />
  <input id="authPass" type="password" placeholder="Password" class="w-full p-2 text-black rounded mb-2" /><button onclick="signupUser()" class="btn w-full mb-2 bg-blue-600 px-4 py-2 rounded-lg">Sign Up</button> <button onclick="loginUser()" class="btn w-full bg-green-600 hover:bg-green-700 px-4 py-2 rounded-lg">Login</button>

  <p class="mt-2 text-sm opacity-70 text-center cursor-pointer" onclick="forgotPassword()">Forgot Password?</p>
</div><script>
let users = JSON.parse(localStorage.getItem("reUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("reCurrent")) || null;

function saveUsers(){ localStorage.setItem('reUsers', JSON.stringify(users)); }
function saveCurrentUser(){ localStorage.setItem('reCurrent', JSON.stringify(currentUser)); }

function signupUser(){
  let n = document.getElementById('authName').value.trim();
  let e = document.getElementById('authEmail').value.trim();
  let p = document.getElementById('authPass').value.trim();
  if(!n||!e||!p){ alert('Fill all fields'); return; }
  if(users.find(u=>u.email===e)){ alert('Email already registered'); return; }
  let u={name:n,email:e,pass:p,balance:0};
  users.push(u);
  saveUsers();
  currentUser=u;
  saveCurrentUser();
  loadDashboard();
}

function loginUser(){
  let e = document.getElementById('authEmail').value.trim();
  let p = document.getElementById('authPass').value.trim();
  let u = users.find(u=>u.email===e && u.pass===p);
  if(!u){ alert('Invalid credentials'); return; }
  currentUser=u;
  saveCurrentUser();
  loadDashboard();
}

function forgotPassword(){
  let email=prompt('Enter your registered email:');
  let user=users.find(u=>u.email===email);
  if(user){ alert('Your password is: '+user.pass); } else { alert('Email not found'); }
}

window.onload=function(){
  if(currentUser){ loadDashboard(); }
}

function loadDashboard(){
  document.getElementById('authBox').style.display='none';
  document.body.insertAdjacentHTML('afterbegin', `<h1 class='text-4xl font-bold text-center animate-fade'>Welcome ${currentUser.name}!</h1>`);
}
</script></body>
</html>
