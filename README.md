
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Yard Parking Manager v3.0</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif;background:linear-gradient(135deg,#0f0c29,#302b63,#24243e);color:#fff;min-height:100vh;overflow-x:hidden}
.login-overlay{position:fixed;top:0;left:0;width:100%;height:100%;background:linear-gradient(135deg,#0f0c29,#302b63,#24243e);display:flex;align-items:center;justify-content:center;z-index:9999;transition:opacity 0.5s}
.login-overlay.hidden{opacity:0;pointer-events:none}
.login-card{background:rgba(255,255,255,0.07);backdrop-filter:blur(20px);border:1px solid rgba(255,255,255,0.15);border-radius:24px;padding:48px 40px;width:420px;max-width:95vw;box-shadow:0 25px 60px rgba(0,0,0,0.5);text-align:center}
.login-card .logo{font-size:56px;margin-bottom:10px}
.login-card h1{font-size:26px;margin-bottom:4px;background:linear-gradient(135deg,#b8a4ff,#7c5cfc);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.login-card .version{font-size:11px;color:#7c5cfc;font-weight:700;letter-spacing:2px;margin-bottom:4px}
.login-card .subtitle{color:#888;font-size:12px;margin-bottom:32px}
.input-group{position:relative;margin-bottom:18px;text-align:left}
.input-group label{display:block;font-size:11px;color:#999;margin-bottom:5px;font-weight:700;letter-spacing:0.5px;text-transform:uppercase}
.input-group .icon{position:absolute;right:14px;top:34px;font-size:16px;color:#666}
.input-group input{width:100%;padding:14px 40px 14px 14px;background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.12);border-radius:12px;color:#fff;font-size:15px;outline:none;transition:border 0.3s}
.input-group input:focus{border-color:#7c5cfc;box-shadow:0 0 0 3px rgba(124,92,252,0.15)}
.input-group input::placeholder{color:#555}
.login-btn{width:100%;padding:15px;margin-top:14px;background:linear-gradient(135deg,#7c5cfc,#5a3fd6);border:none;border-radius:14px;color:#fff;font-size:16px;font-weight:700;cursor:pointer;transition:transform 0.2s,box-shadow 0.3s;letter-spacing:0.5px}
.login-btn:hover{transform:translateY(-2px);box-shadow:0 8px 25px rgba(124,92,252,0.4)}
.login-error{color:#ff6b6b;font-size:13px;margin-top:12px;display:none;background:rgba(255,107,107,0.1);padding:8px 14px;border-radius:10px}
.login-footer{margin-top:28px;font-size:11px;color:#555}
.login-footer .owner{font-weight:700;color:#aaa;font-size:12px}
.divider{height:1px;background:rgba(255,255,255,0.08);margin:22px 0}
.app-container{display:none}
.app-container.active{display:block}
header{background:rgba(255,255,255,0.06);backdrop-filter:blur(12px);border-bottom:1px solid rgba(255,255,255,0.08);padding:12px 24px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100}
.header-left{display:flex;align-items:center;gap:12px}
.header-left .app-title{font-size:18px;font-weight:700}
.live-badge{background:#00e676;color:#000;font-size:9px;font-weight:800;padding:3px 10px;border-radius:20px;animation:blink 2s infinite;letter-spacing:1px}
@keyframes blink{0%,100%{opacity:1}50%{opacity:0.4}}
.sync-label{font-size:11px;color:#888;display:flex;align-items:center;gap:5px}
.sync-dot{width:7px;height:7px;background:#00e676;border-radius:50%;display:inline-block;animation:blink 2s infinite}
.header-right{display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.user-pill{display:flex;align-items:center;gap:10px;background:rgba(255,255,255,0.08);padding:5px 14px 5px 5px;border-radius:30px;font-size:13px}
.user-avatar{width:32px;height:32px;border-radius:50%;background:linear-gradient(135deg,#7c5cfc,#5a3fd6);display:flex;align-items:center;justify-content:center;font-weight:700;font-size:12px}
.user-info span{display:block}
.user-info .user-name{font-weight:600;font-size:12px}
.user-info .user-role{font-size:10px;color:#aaa}
.hbtn{padding:7px 14px;border-radius:10px;border:none;font-size:11px;font-weight:700;cursor:pointer;transition:0.3s}
.mbtn{background:rgba(124,92,252,0.2);color:#b8a4ff}
.mbtn:hover{background:rgba(124,92,252,0.4)}
.rbtn{background:rgba(0,230,118,0.15);color:#00e676}
.rbtn:hover{background:rgba(0,230,118,0.3)}
.lbtn{background:rgba(255,107,107,0.15);color:#ff6b6b}
.lbtn:hover{background:rgba(255,107,107,0.3)}
.dbar{padding:16px 24px 0;display:flex;align-items:center;gap:14px;flex-wrap:wrap}
.dbar label{font-size:12px;color:#aaa;font-weight:700;letter-spacing:0.5px}
.dsel{padding:9px 16px;background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.15);border-radius:10px;color:#fff;font-size:13px;outline:none;cursor:pointer;min-width:200px}
.dsel:focus{border-color:#7c5cfc}
.dsel option{background:#302b63;color:#fff}
.dsaved{font-size:11px;color:#00e676;font-weight:600;opacity:0;transition:opacity 0.3s}
.dsaved.show{opacity:1}
.zone-selector{padding:22px 24px 10px}
.zone-selector h2{font-size:17px;margin-bottom:14px}
.zcards{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:14px}
.zcard{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.1);border-radius:16px;padding:18px;cursor:pointer;transition:all 0.3s;text-align:center}
.zcard:hover{transform:translateY(-3px);border-color:#7c5cfc;box-shadow:0 6px 24px rgba(124,92,252,0.2)}
.zcard.active{border-color:#7c5cfc;background:rgba(124,92,252,0.12)}
.zcard .zi{font-size:30px;margin-bottom:6px}
.zcard .zn{font-weight:700;font-size:14px}
.zcard .zd{font-size:11px;color:#aaa;margin-top:3px}
.zcard .zc{font-size:18px;font-weight:800;color:#7c5cfc;margin-top:6px}
.sbar{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;padding:18px 24px}
.sc{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.08);border-radius:14px;padding:16px;text-align:center;transition:0.3s}
.sc:hover{transform:translateY(-2px)}
.sc .sv{font-size:30px;font-weight:800}
.sc .sl{font-size:11px;color:#aaa;margin-top:3px;font-weight:600}
.sc1 .sv{color:#7c5cfc}
.sc2 .sv{color:#ff1744}
.sc3 .sv{color:#00e676}
.sc4 .sv{color:#ffd740}
.legend{padding:10px 24px;display:flex;align-items:center;gap:20px;flex-wrap:wrap}
.legend-item{display:flex;align-items:center;gap:8px;font-size:12px}
.lbox{width:60px;height:30px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:9px;font-weight:800}
.lv{background:linear-gradient(135deg,#00e676,#00c853);color:#003d00}
.lp{background:linear-gradient(135deg,#ff1744,#d50000);color:#fff}
.psec{padding:10px 24px 28px}
.zhl{grid-column:1/-1;padding:14px 0 6px;font-size:15px;font-weight:700;color:#b8a4ff;border-bottom:1px solid rgba(255,255,255,0.1);margin-bottom:4px;display:flex;align-items:center;gap:8px}
.pgrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(92px,1fr));gap:9px}
.pspot{position:relative;border-radius:12px;padding:8px 6px;text-align:center;cursor:pointer;transition:all 0.25s;font-weight:800;min-height:82px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;user-select:none;border:2px solid transparent}
.pspot:hover{transform:scale(1.1);z-index:2}
.pspot:active{transform:scale(0.97)}
.pspot.vacant{background:linear-gradient(135deg,#00e676,#00c853);color:#003d00;border-color:rgba(0,230,118,0.5);box-shadow:0 3px 12px rgba(0,230,118,0.25)}
.pspot.parked{background:linear-gradient(135deg,#ff1744,#d50000);color:#fff;border-color:rgba(255,23,68,0.5);box-shadow:0 3px 12px rgba(255,23,68,0.25)}
.sicon{font-size:18px;line-height:1}
.sid{font-size:13px;font-weight:900;letter-spacing:0.5px}
.sst{font-size:8px;font-weight:700;text-transform:uppercase;letter-spacing:1.2px;opacity:0.85}
.asec{padding:10px 24px 28px}
.asec h3{margin-bottom:10px;font-size:15px}
.alog{max-height:240px;overflow-y:auto;background:rgba(255,255,255,0.04);border-radius:14px;padding:14px;border:1px solid rgba(255,255,255,0.08)}
.le{padding:7px 0;border-bottom:1px solid rgba(255,255,255,0.05);font-size:12px;color:#ccc}
.le:last-child{border-bottom:none}
.lt{color:#7c5cfc;font-weight:600;margin-right:6px;font-size:11px}
footer{text-align:center;padding:16px;border-top:1px solid rgba(255,255,255,0.06);font-size:11px;color:#555}
footer .fs{font-size:10px;color:#444;margin-top:3px}
.toast{position:fixed;bottom:28px;left:50%;transform:translateX(-50%);background:rgba(30,27,58,0.95);color:#fff;padding:12px 26px;border-radius:14px;font-size:14px;font-weight:600;box-shadow:0 8px 30px rgba(0,0,0,0.5);border:1px solid rgba(124,92,252,0.3);z-index:10000;display:none}
.mo{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.7);z-index:9998;display:none;align-items:center;justify-content:center}
.mo.open{display:flex}
.mb{background:#1e1b3a;border:1px solid rgba(255,255,255,0.1);border-radius:20px;padding:30px;width:520px;max-width:95vw;max-height:90vh;overflow-y:auto}
.mb h2{font-size:17px;margin-bottom:4px}
.mb .mn{color:#888;font-size:11px;margin-bottom:16px}
.mc{float:right;background:none;border:none;color:#fff;font-size:22px;cursor:pointer}
.ul{max-height:200px;overflow-y:auto;margin-bottom:16px}
.uli{display:flex;justify-content:space-between;align-items:center;padding:10px 12px;background:rgba(255,255,255,0.05);border-radius:10px;margin-bottom:7px;font-size:12px}
.uli .ud{color:#aaa;font-size:10px}
.uli .ru{background:rgba(255,107,107,0.2);color:#ff6b6b;border:none;border-radius:8px;padding:4px 12px;cursor:pointer;font-size:10px;font-weight:700}
.auf{display:flex;gap:7px;flex-wrap:wrap}
.auf input,.auf select{flex:1;min-width:90px;padding:10px;border-radius:10px;background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.12);color:#fff;font-size:12px;outline:none}
.auf select option{background:#1e1b3a}
.auf button{padding:10px 18px;border-radius:10px;border:none;background:#7c5cfc;color:#fff;font-weight:700;cursor:pointer;font-size:12px}
@media(max-width:768px){.sbar{grid-template-columns:repeat(2,1fr)}.pgrid{grid-template-columns:repeat(auto-fill,minmax(78px,1fr))}header{flex-wrap:wrap;gap:8px}.zcards{grid-template-columns:repeat(2,1fr)}}
@media(max-width:480px){.login-card{padding:32px 24px}.zcards{grid-template-columns:1fr}.pgrid{grid-template-columns:repeat(auto-fill,minmax(68px,1fr))}.sbar{grid-template-columns:repeat(2,1fr);gap:8px}}
</style>
</head>
<body>

<div class="login-overlay" id="LO">
<div class="login-card">
<div class="logo">&#x1F17F;&#xFE0F;</div>
<h1>Yard Parking Manager</h1>
<div class="version">VERSION 3.0</div>
<p class="subtitle">Secure Yard Supervisor Access Portal</p>
<div class="input-group">
<label>Login ID</label>
<span class="icon">&#x1F464;</span>
<input type="text" id="lid" placeholder="Enter your Login ID" autocomplete="off">
</div>
<div class="input-group">
<label>Password</label>
<span class="icon">&#x1F512;</span>
<input type="password" id="lpw" placeholder="Enter your Password">
</div>
<button class="login-btn" onclick="doLogin()">&#x1F510; Sign In</button>
<p class="login-error" id="lerr"></p>
<div class="divider"></div>
<div class="login-footer">
<p class="owner">&#x1F464; Owner: B. Rajesh Patnaik</p>
<p style="margin-top:4px">&#x1F512; Authorized personnel only</p>
</div>
</div>
</div>

<div class="app-container" id="AC">
<header>
<div class="header-left">
<span class="app-title">&#x1F17F;&#xFE0F; Yard Parking Manager</span>
<span class="live-badge">&#x25CF; LIVE</span>
<span class="sync-label"><span class="sync-dot"></span> Auto-Sync</span>
</div>
<div class="header-right">
<button class="hbtn mbtn" onclick="openUM()">&#x1F465; Manage Users</button>
<button class="hbtn rbtn" onclick="doRefresh()">&#x1F504; Refresh</button>
<div class="user-pill">
<div class="user-avatar" id="uav">RP</div>
<div class="user-info">
<span class="user-name" id="uname">&#8212;</span>
<span class="user-role" id="urole">&#8212;</span>
</div>
</div>
<button class="hbtn lbtn" onclick="doLogout()">&#x23FB; Logout</button>
</div>
</header>

<div class="dbar">
<label>&#x1F3F7;&#xFE0F; DESIGNATION:</label>
<select class="dsel" id="mdesg" onchange="saveDsg()">
<option value="" disabled selected>Select your Designation</option>
<option value="Yard Manager">Yard Manager</option>
<option value="Shift Supervisor">Shift Supervisor</option>
<option value="Dock Coordinator">Dock Coordinator</option>
<option value="Transport Lead">Transport Lead</option>
<option value="Operations Head">Operations Head</option>
<option value="Security Incharge">Security Incharge</option>
</select>
<span class="dsaved" id="dsvd">&#x2705; Saved</span>
</div>

<div class="zone-selector">
<h2>&#x1F4CD; Select Parking Zone</h2>
<div class="zcards">
<div class="zcard active" onclick="selZ('A')" id="zcA">
<div class="zi">&#x1F69B;</div>
<div class="zn">Dock Parking &#8212; Zone A</div>
<div class="zd">Dock Doors DD-1 to DD-56</div>
<div class="zc" id="znA">56</div>
</div>
<div class="zcard" onclick="selZ('B')" id="zcB">
<div class="zi">&#x1F69A;</div>
<div class="zn">External Parking &#8212; Zone B</div>
<div class="zd">Yard Parking YP-1 to YP-58</div>
<div class="zc" id="znB">58</div>
</div>
<div class="zcard" onclick="selZ('C')" id="zcC">
<div class="zi">&#x1F690;</div>
<div class="zn">Internal Parking &#8212; Zone C</div>
<div class="zd">Internal Parking IP-1 to IP-8</div>
<div class="zc" id="znC">8</div>
</div>
<div class="zcard" onclick="selZ('X')" id="zcX">
<div class="zi">&#x1F4CA;</div>
<div class="zn">Real-Time Dashboard</div>
<div class="zd">All Zones Combined</div>
<div class="zc" id="znX">122</div>
</div>
</div>
</div>

<div class="sbar">
<div class="sc sc1"><div class="sv" id="s1">0</div><div class="sl">&#x1F4E6; TOTAL SPACES</div></div>
<div class="sc sc2"><div class="sv" id="s2">0</div><div class="sl">&#x1F69B; PARKED</div></div>
<div class="sc sc3"><div class="sv" id="s3">0</div><div class="sl">&#x2705; VACANT</div></div>
<div class="sc sc4"><div class="sv" id="s4">0%</div><div class="sl">&#x1F4CA; OCCUPANCY</div></div>
</div>

<div class="legend">
<div class="legend-item"><div class="lbox lv">&#x1F7E2; DD-1</div><span>Vacant &#8212; Click to Park</span></div>
<div class="legend-item"><div class="lbox lp">&#x1F69B; DD-1</div><span>Parked &#8212; Click to Vacate</span></div>
<div style="margin-left:auto;font-size:11px;color:#666">&#x1F464; Owner: B. Rajesh Patnaik</div>
</div>

<div class="psec"><div class="pgrid" id="PG"></div></div>

<div class="asec">
<h3>&#x1F4CB; Activity Log</h3>
<div class="alog" id="AL"></div>
</div>

<footer>
<div>&#x1F17F;&#xFE0F; Yard Parking Management System v3.0 &#8212; Owner: B. Rajesh Patnaik &#8212; &#x1F504; Real-Time Auto-Sync &#x2705;</div>
<div class="fs">Last synced: <span id="LS">&#8212;</span></div>
</footer>
</div>

<div class="toast" id="TT"></div>

<div class="mo" id="UM">
<div class="mb">
<button class="mc" onclick="closeUM()">&#x2715;</button>
<h2>&#x1F465; User Access Management</h2>
<p class="mn">Only the owner (B. Rajesh Patnaik) can add or remove users.</p>
<div class="ul" id="UL"></div>
<h4 style="margin-bottom:10px;font-size:13px">&#x2795; Add New User</h4>
<div class="auf">
<input type="text" id="nuid" placeholder="Login ID">
<input type="password" id="nupw" placeholder="Password">
<select id="nudg">
<option value="" disabled selected>Designation</option>
<option value="Yard Manager">Yard Manager</option>
<option value="Shift Supervisor">Shift Supervisor</option>
<option value="Dock Coordinator">Dock Coordinator</option>
<option value="Transport Lead">Transport Lead</option>
<option value="Operations Head">Operations Head</option>
<option value="Security Incharge">Security Incharge</option>
</select>
<button onclick="addUsr()">Add</button>
</div>
</div>
</div>

<script>
(function(){
"use strict";
var SK='ypm_s',SH=24,DK='ypm_d',LK='ypm_l',UK='ypm_u';
var ZS={A:{px:'DD',ct:56,ic:'\u{1F69B}',tt:'Zone A \u2014 Dock Parking'},B:{px:'YP',ct:58,ic:'\u{1F69A}',tt:'Zone B \u2014 External (Yard) Parking'},C:{px:'IP',ct:8,ic:'\u{1F690}',tt:'Zone C \u2014 Internal Parking'}};
var cZ='A';

function ls(k){try{return localStorage.getItem(k)}catch(e){return null}}
function lss(k,v){try{localStorage.setItem(k,v)}catch(e){}}
function lsr(k){try{localStorage.removeItem(k)}catch(e){}}

function gU(){var u=ls(UK);if(u){try{return JSON.parse(u)}catch(e){}}var d=[{id:'rajbalpt',pass:'[PASSWORD]',designation:'Operations Head',name:'B. Rajesh Patnaik',isOwner:true}];lss(UK,JSON.stringify(d));return d}
function sU(u){lss(UK,JSON.stringify(u))}

function gS(){var s=ls(SK);if(!s)return null;try{var p=JSON.parse(s);if(Date.now()-p.ts>SH*3600000){lsr(SK);return null}return p}catch(e){lsr(SK);return null}}
function sS(u){lss(SK,JSON.stringify({uid:u.id,nm:u.name||u.id,dg:u.designation||'',ow:u.isOwner||false,ts:Date.now()}))}
function uDg(d){var s=gS();if(s){s.dg=d;lss(SK,JSON.stringify(s))}}

function gD(){var d=ls(DK);if(d){try{return JSON.parse(d)}catch(e){}}var data={},zk=['A','B','C'];for(var z=0;z<zk.length;z++){var k=zk[z],info=ZS[k];data[k]={};for(var i=1;i<=info.ct;i++)data[k][i]='v';}lss(DK,JSON.stringify(data));return data}
function sD(d){lss(DK,JSON.stringify(d))}

function gL(){var l=ls(LK);if(l){try{return JSON.parse(l)}catch(e){}}return[]}
function aLog(m){var logs=gL(),now=new Date();var t=now.toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit',second:'2-digit'});var d=now.toLocaleDateString('en-IN');logs.unshift({t:t,d:d,m:m});if(logs.length>300)logs.length=300;lss(LK,JSON.stringify(logs))}

function toast(m){var t=document.getElementById('TT');t.innerHTML=m;t.style.display='block';if(t._tm)clearTimeout(t._tm);t._tm=setTimeout(function(){t.style.display='none'},2800)}

window.doLogin=function(){
  var id=document.getElementById('lid').value.trim();
  var pw=document.getElementById('lpw').value;
  var err=document.getElementById('lerr');
  if(!id||!pw){err.textContent='\[PASSWORD] Please enter both Login ID and Password.';err.style.display='block';return}
  var users=gU(),user=null;
  for(var i=0;i<users.length;i++){if(users[i].id===id&&users[i].pass===pw){user=users[i];break}}
  if(!user){err.textContent='\[PASSWORD] Invalid Login ID or Password. Contact admin.';err.style.display='block';return}
  sS(user);aLog('\u{1F510} <b>'+user.name+'</b> logged in');showApp()
};

window.doLogout=function(){var s=gS();if(s)aLog('\u23FB <b>'+s.nm+'</b> logged out');lsr(SK);location.reload()};

window.saveDsg=function(){
  var dg=document.getElementById('mdesg').value;if(!dg)return;
  uDg(dg);var s=gS();
  if(s){var users=gU();for(var i=0;i<users.length;i++){if(users[i].id===s.uid){users[i].designation=dg;break}}sU(users);
  document.getElementById('urole').textContent=dg;
  var sv=document.getElementById('dsvd');sv.classList.add('show');setTimeout(function(){sv.classList.remove('show')},2000);
  aLog('\u{1F3F7}\u{FE0F} <b>'+s.nm+'</b> set designation to <b>'+dg+'</b>');rLogs();toast('\u{1F3F7}\u{FE0F} Designation: '+dg)}
};

window.doRefresh=function(){rZone();rLogs();uSync();uZC();toast('\u{1F504} Dashboard refreshed!')};

function showApp(){
  document.getElementById('LO').classList.add('hidden');
  document.getElementById('AC').classList.add('active');
  var s=gS();if(s){
    document.getElementById('uname').textContent=s.nm;
    document.getElementById('urole').textContent=s.dg||'Select designation below';
    var w=s.nm.split(' '),ini='';for(var i=0;i<w.length&&i<2;i++){if(w[i])ini+=w[i][0]}
    document.getElementById('uav').textContent=ini.toUpperCase();
    if(s.dg)document.getElementById('mdesg').value=s.dg;
  }
  rZone();rLogs();uSync();uZC()
}

function uZC(){
  var data=gD(),zk=['A','B','C'],tA=0,pA=0;
  for(var z=0;z<zk.length;z++){var k=zk[z],info=ZS[k],p=0;for(var i=1;i<=info.ct;i++){if(data[k][i]==='p')p++}
  var el=document.getElementById('zn'+k);if(el)el.textContent=p+'/'+info.ct+' parked';tA+=info.ct;pA+=p}
  var eA=document.getElementById('znX');if(eA)eA.textContent=pA+'/'+tA+' parked'
}

window.selZ=function(zone){
  cZ=zone;var ids=['A','B','C','X'];
  for(var i=0;i<ids.length;i++){var el=document.getElementById('zc'+ids[i]);if(el){if(ids[i]===zone)el.classList.add('active');else el.classList.remove('active')}}
  rZone()
};

function rZone(){
  var grid=document.getElementById('PG');grid.innerHTML='';
  var data=gD();var ztr=cZ==='X'?['A','B','C']:[cZ];
  var tS=0,tP=0;
  for(var z=0;z<ztr.length;z++){
    var zK=ztr[z],info=ZS[zK];
    if(cZ==='X'){var hdr=document.createElement('div');hdr.className='zhl';hdr.innerHTML=info.ic+' '+info.tt;grid.appendChild(hdr)}
    for(var i=1;i<=info.ct;i++){
      var isP=data[zK][i]==='p',sId=info.px+'-'+i;
      var vI=isP?'\u{1F69B}':'\u{1F7E2}';
      var el=document.createElement('div');
      el.className='pspot '+(isP?'parked':'vacant');
      el.innerHTML='<div class="sicon">'+vI+'</div><div class="sid">'+sId+'</div><div class="sst">'+(isP?'PARKED':'VACANT')+'</div>';
      el.setAttribute('data-z',zK);el.setAttribute('data-n',String(i));el.setAttribute('data-s',sId);
      el.onclick=function(){tgSpot(this.getAttribute('data-z'),parseInt(this.getAttribute('data-n')),this.getAttribute('data-s'))};
      grid.appendChild(el);tS++;if(isP)tP++
    }
  }
  var tV=tS-tP,occ=tS>0?Math.round((tP/tS)*100):0;
  document.getElementById('s1').textContent=tS;
  document.getElementById('s2').textContent=tP;
  document.getElementById('s3').textContent=tV;
  document.getElementById('s4').textContent=occ+'%'
}

function tgSpot(zone,num,spotId){
  var data=gD(),s=gS(),uN=s?s.nm:'Unknown';
  if(data[zone][num]==='v'){
    data[zone][num]='p';
    aLog('\u{1F69B} <b>'+spotId+'</b> \u2192 <b style="color:#ff1744">PARKED</b> by '+uN);
    toast('\u{1F69B} '+spotId+' \u2192 PARKED')
  }else{
    data[zone][num]='v';
    aLog('\u2705 <b>'+spotId+'</b> \u2192 <b style="color:#00e676">VACANT</b> by '+uN);
    toast('\u2705 '+spotId+' \u2192 VACANT')
  }
  sD(data);rZone();rLogs();uSync();uZC()
}

function rLogs(){
  var logs=gL(),el=document.getElementById('AL');
  if(logs.length===0){el.innerHTML='<div style="color:#555;text-align:center;padding:18px">No activity yet \u2014 click a spot to begin</div>';return}
  var html='',max=Math.min(logs.length,60);
  for(var i=0;i<max;i++)html+='<div class="le"><span class="lt">['+logs[i].d+' '+logs[i].t+']</span>'+logs[i].m+'</div>';
  el.innerHTML=html
}

function uSync(){var now=new Date();document.getElementById('LS').textContent=now.toLocaleDateString('en-IN')+' '+now.toLocaleTimeString('en-IN')}

window.openUM=function(){var s=gS();if(!s||!s.ow){toast('\u26A0\uFE0F Only the owner can manage users');return}rUL();document.getElementById('UM').classList.add('open')};
window.closeUM=function(){document.getElementById('UM').classList.remove('open')};

function rUL(){
  var users=gU(),html='';
  for(var i=0;i<users.length;i++){var u=users[i];html+='<div class="uli"><div><b>'+u.id+'</b> \u2014 '+u.name+'<br><span class="ud">'+u.designation+(u.isOwner?' \u{1F451} Owner':'')+'</span></div>';
  if(!u.isOwner)html+='<button class="ru" onclick="rmUsr('+i+')">Remove</button>';html+='</div>'}
  document.getElementById('UL').innerHTML=html
}

window.addUsr=function(){
  var id=document.getElementById('nuid').value.trim(),pw=document.getElementById('nupw').value,dg=document.getElementById('nudg').value;
  if(!id||!pw||!dg){toast('\u26A0\uFE0F Fill all fields');return}
  var users=gU();for(var i=0;i<users.length;i++){if(users[i].id===id){toast('\u26A0\uFE0F User exists');return}}
  users.push({id:id,pass:pw,designation:dg,name:id,isOwner:false});sU(users);
  aLog('\u{1F464} New user <b>'+id+'</b> ('+dg+') added');
  document.getElementById('nuid').value='';document.getElementById('nupw').value='';document.getElementById('nudg').selectedIndex=0;
  rUL();rLogs();toast('\u2705 User "'+id+'" added!')
};

window.rmUsr=function(index){
  var users=gU(),removed=users.splice(index,1)[0];sU(users);
  aLog('\u{1F5D1}\uFE0F User <b>'+removed.id+'</b> removed');rUL();rLogs();toast('\u{1F5D1}\uFE0F User "'+removed.id+'" removed')
};

var session=gS();
if(session)showApp();
})();
</script>
</body>
</html>

