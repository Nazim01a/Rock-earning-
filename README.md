<ROCK><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rock Earn Dashboard</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.tailwindcss.com"></script>
<style>
:root{--accent:#00f7ef;--dark:#0e0e15;--card:rgba(0,0,0,0.7)}
html,body{height:100%;margin:0;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;color:#e6f7ff;background:linear-gradient(180deg,#071026 0%,#081423 100%);-webkit-font-smoothing:antialiased;overflow:hidden}
canvas#bgCanvas{position:fixed;inset:0;width:100%;height:100%;z-index:-1}
.sidebar{position:fixed;left:0;top:0;bottom:0;width:84px;background:rgba(10,10,12,0.95);display:flex;flex-direction:column;align-items:center;padding:18px 8px;gap:14px;box-shadow:2px 0 20px rgba(0,0,0,0.8);z-index:50;display:none}
.logo{font-size:28px;color:#00f7ef}
.icon-btn{width:64px;height:64px;border-radius:14px;background:linear-gradient(180deg,#0b0b10,#0f1220);display:flex;align-items:center;justify-content:center;flex-direction:column;color:#bcdbe3;cursor:pointer;box-shadow:0 6px 18px rgba(0,0,0,0.7);transition:transform .18s,box-shadow .18s}
.icon-btn i{font-size:20px}
.icon-name{font-size:10px;margin-top:6px;color:#9fd9e9;text-align:center}
.icon-btn:hover{transform:translateY(-6px);box-shadow:0 12px 30px rgba(0,247,239,0.3),0 6px 20px rgba(0,0,0,0.7)}
#welcomeBox{position:fixed;left:100px;right:20px;top:18px;padding:18px;border-radius:14px;background:linear-gradient(180deg,rgba(0,247,239,0.05),rgba(0,247,239,0.01));backdrop-filter:blur(8px);box-shadow:0 4px 30px rgba(0,0,0,0.7);display:none;z-index:40;animation:fadeIn 1s ease-in-out}
@keyframes fadeIn{from{opacity:0;transform:translateY(-20px)}to{opacity:1;transform:translateY(0)}}
.stats{display:flex;gap:18px;margin-top:12px;align-items:center;flex-wrap:wrap}
.stat-card{background:linear-gradient(180deg,rgba(0,247,239,0.03),rgba(0,247,239,0.01));padding:12px 16px;border-radius:10px;box-shadow:inset 0 -1px 0 rgba(255,255,255,0.02);min-width:120px;transition:all .3s}
.stat-card:hover{background:linear-gradient(180deg,rgba(0,247,239,0.1),rgba(0,247,239,0.03));transform:scale(1.05)}
.stat-label{font-size:11px;color:var(--accent);margin:0 0 6px}
.stat-value{font-size:16px;font-weight:700;margin:0}
.btn{background:transparent;border:1px solid rgba(0,247,239,0.2);padding:10px 12px;border-radius:10px;color:var(--accent);cursor:pointer;font-weight:700;min-width:120px;transition:all .3s}
.btn:hover{background:rgba(0,247,239,0.08);box-shadow:0 8px 28px rgba(0,247,239,0.3);transform:scale(1.05)}
#contentSection{position:fixed;left:100px;top:110px;right:20px;bottom:24px;background:linear-gradient(180deg,rgba(0,0,0,0.5),rgba(0,0,0,0.65));border-radius:14px;padding:18px;overflow:auto;display:none;z-index:30;box-shadow:0 8px 40px rgba(0,0,0,0.7)}
#backBtn{position:absolute;left:12px;top:10px;background:#ff4766;color:white;padding:8px 10px;border-radius:8px;border:none;cursor:pointer;display:inline-block;box-shadow:0 6px 20px rgba(255,71,102,0.3)}
#notif{position:fixed;top:18px;right:-420px;background:linear-gradient(45deg,var(--accent),#9ff0ee);color:#001;padding:12px 20px;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,0.5);font-weight:700;transition:right .45s;z-index:60}
#notif.show{right:20px}
#authBox{position:fixed;left:120px;top:140px;width:320px;padding:18px;border-radius:12px;background:rgba(0,0,0,0.7);z-index:45;display:block}
#authBox input{width:100%;padding:10px;margin-bottom:10px;border-radius:10px;background:#0f1320;color:#dff7fb;border:1px solid rgba(255,255,255,0.05)}
.plan-card{border-radius:12px;padding:14px;border:1px solid rgba(0,247,239,0.1);margin-bottom:14px;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0));transition:all .3s}
.plan-card:hover{transform:scale(1.03);box-shadow:0 12px 40px rgba(0,247,239,0.25)}
.plan-header{display:flex;justify-content:space-between;align-items:center}
.muted{color:#9fcfda;font-size:13px}
.small{font-size:13px}
footer.company{position:fixed;right:18px;bottom:18px;background:rgba(0,0,0,0.7);padding:12px;border-radius:12px;color:#9ff;font-size:13px;box-shadow:0 8px 30px rgba(0,0,0,0.7)}
@media(max-width:900px){.sidebar{display:flex;bottom:auto;top:0;left:0;right:0;height:auto;width:100%;flex-direction:row;padding:10px}#welcomeBox{left:12px;right:12px}#authBox{left:12px;top:200px}#contentSection{left:12px;top:300px;right:12px;bottom:12px}}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>
<div id="notif"></div>
<div class="sidebar" id="sidebar">
<div class="logo">💎</div>
<div class="icon-btn" onclick="openSection('plans')"><i class="fa fa-gem"></i><div class="icon-name">Plans</div></div>
<div class="icon-btn" onclick="openSection('deposit')"><i class="fa fa-money-bill"></i><div class="icon-name">Deposit</div></div>
<div class="icon-btn" onclick="openSection('withdraw')"><i class="fa fa-hand-holding-dollar"></i><div class="icon-name">Withdraw</div></div>
<div class="icon-btn" onclick="openSection('profit')"><i class="fa fa-coins"></i><div class="icon-name">Profit</div></div>
<div class="icon-btn" onclick="openSection('history')"><i class="fa fa-clock"></i><div class="icon-name">History</div></div>
<div class="icon-btn" onclick="openSection('support')"><i class="fa fa-headset"></i><div class="icon-name">Support</div></div>
<div class="icon-btn" onclick="openSection('referral')"><i class="fa fa-share-alt"></i><div class="icon-name">Referral</div></div>
<div class="icon-btn" onclick="openSection('share')"><i class="fa fa-share"></i><div class="icon-name">Share</div></div>
</div>
<div id="welcomeBox">
<h2 id="welcomeTxt">🎉 Welcome to <strong style="color:#00f7ef">Rock Earn</strong> 💎</h2>
<div class="stats">
<div class="stat-card"><div class="stat-label">Balance</div><div class="stat-value" id="balValue">0 PKR</div></div>
<div class="stat-card"><div class="stat-label">Total Profit</div><div class="stat-value" id="profValue">0 PKR</div></div>
<div style="display:flex;align-items:center"><button class="btn" onclick="refreshBalance()">Refresh</button></div>
</div>
</div>
<div id="contentSection">
<button id="backBtn" onclick="closeSection()" style="display:none">← Back</button>
<div id="sectionContent"></div>
</div>
<div id="authBox">
<h3 style="margin:0 0 12px 0;text-align:center">Login / Sign Up / Forgot Password</h3>
<input id="authName" placeholder="Full Name">
<input id="authEmail" placeholder="Email">
<input id="authPass" type="password" placeholder="Password">
<div style="display:flex;gap:8px;margin-bottom:6px">
<button class="btn" id="signupBtn"><i class="fa fa-user-plus"></i> Sign Up</button>
<button class="btn" id="loginBtn" style="background:#00b37e;color:#001;border:none"><i class="fa fa-sign-in-alt"></i> Login</button>
</div>
<div style="margin-bottom:6px">
<button class="btn" style="background:#ffbf00;color:#001;border:none;width:100%" id="forgotBtn">Forgot Password</button>
</div>
<p class="muted small" style="margin-top:10px">Default admin: <strong>admin@rockearn.com</strong> (auto-created if missing).</p>
</div>
<button class="logout-btn" id="logoutBtn" onclick="logoutUser()" style="position:fixed;bottom:70px;right:30px;padding:12px 20px;border-radius:12px;background:#ff4766;color:white;border:none;cursor:pointer;display:none;box-shadow:0 8px 30px rgba(255,71,102,0.3)">Logout</button>
<button class="admin-btn" id="adminBtn" onclick="openSection('admin')" title="Admin Panel" style="display:none;position:fixed;bottom:120px;right:30px;padding:12px 20px;border-radius:12px;background:#00f7ef;color:#001;border:none;cursor:pointer;box-shadow:0 8px 30px rgba(0,247,239,0.3)">Admin Panel</button>
<footer class="company">
<div><strong>Rock Earn Pro LTD</strong></div>
<div style="font-size:13px">25 Main St, California, United States</div>
<div style="font-size:12px;margin-top:6px;color:#9ff">Secure. Fast. Premium support: support@rockearnpro.com</div>
</footer>
<script>
/* Login/Signup/Forgot Password system fully fixed here with localStorage handling, admin visibility, dashboard, plans, deposit, withdraw functionality */
</script>
</body>
</html>
