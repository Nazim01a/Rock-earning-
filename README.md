<!DOCTYPE html><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Rock Earn - Login / Signup</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(120deg,#0f1123,#1c1f3b);color:white;margin:0;}
.btn{background:#5d5fef;padding:10px 18px;border-radius:10px;color:white;font-weight:600;cursor:pointer;display:inline-block;margin-top:5px;width:100%;}
.btn:hover{opacity:0.85;transform: scale(1.05);}
input{background:#1e213d;border:none;padding:10px;width:100%;border-radius:10px;color:white;margin-top:8px;}
.logo{font-size:28px;font-weight:bold;text-align:center;margin:20px 0;}
</style>
</head>
<body><div class="logo">🚀 Rock Earn</div>
<div id="authPage" class="p-5 max-w-md mx-auto">
<h2 class="text-2xl font-bold mb-4 text-center">Sign Up</h2>
<input id="su_name" placeholder="Username">
<input id="su_email" placeholder="Email">
<input id="su_pass" type="password" placeholder="Password">
<button class="btn" onclick="signup()">Sign Up</button><hr class="my-4 border-gray-600">
<h2 class="text-2xl font-bold mb-4 text-center">Login</h2>
<input id="li_email" placeholder="Email">
<input id="li_pass" type="password" placeholder="Password">
<button class="btn" onclick="login()">Login</button>
</div><div id="dashboard" class="hidden p-5 max-w-md mx-auto">
<h2 class="text-2xl font-bold mb-4 text-center">Dashboard</h2>
<p id="welcome"></p>
<button class="btn bg-red-600 mt-4" onclick="logout()">Logout</button>
</div><script>
let users = JSON.parse(localStorage.getItem("rockUsers")) || [];
let currentUser = JSON.parse(localStorage.getItem("rockCurrentUser")) || null;

window.onload = function(){
  if(currentUser){
    showDashboard();
  }
};

function signup(){
  let n = document.getElementById("su_name").value.trim();
  let e = document.getElementById("su_email").value.trim();
  let p = document.getElementById("su_pass").value.trim();
  if(!n||!e||!p){alert("Fill all fields"); return;}
  if(users.find(u=>u.email===e)){alert("Email already registered"); return;}
  let u = {name:n,email:e,password:p};
  users.push(u);
  localStorage.setItem("rockUsers", JSON.stringify(users));
  currentUser = u;
  localStorage.setItem("rockCurrentUser", JSON.stringify(currentUser));
  alert("Signup successful!");
  showDashboard();
}

function login(){
  let e = document.getElementById("li_email").value.trim();
  let p = document.getElementById("li_pass").value.trim();
  if(!e||!p){alert("Fill all fields"); return;}
  let u = users.find(u=>u.email===e && u.password===p);
  if(!u){alert("Invalid credentials"); return;}
  currentUser = u;
  localStorage.setItem("rockCurrentUser", JSON.stringify(currentUser));
  alert("Login successful!");
  showDashboard();
}

function showDashboard(){
  document.getElementById("authPage").style.display = "none";
  document.getElementById("dashboard").style.display = "block";
  document.getElementById("welcome").innerText = "Welcome, "+currentUser.name;
}

function logout(){
  currentUser = null;
  localStorage.removeItem("rockCurrentUser");
  document.getElementById("dashboard").style.display = "none";
  document.getElementById("authPage").style.display = "block";
}
</script></body>
</html>
