<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Web Completa con Login y Secciones</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<style>
 body {font-family: 'Segoe UI', sans-serif; background: linear-gradient(135deg,#dfe9f3,#ffffff); margin:0; padding:0;}
 .center {display:flex; justify-content:center; align-items:center; height:100vh;}
 .login-box {background:white; padding:30px; border-radius:14px; box-shadow:0 6px 20px rgba(0,0,0,0.1); width:350px;}
 input {width:100%; padding:10px; margin-top:10px; border-radius:8px; border:1px solid #ccc;}
 button {width:100%; padding:12px; margin-top:15px; background:#4a6cf7; color:white; border:none; border-radius:8px; cursor:pointer;}
 #main {display:none; padding:40px;}
 h1{text-align:center;}
 .tabs {display:flex; gap:15px; justify-content:center; margin-top:20px;}
 .tab {background:white; padding:15px 25px; border-radius:12px; cursor:pointer; box-shadow:0 4px 14px rgba(0,0,0,0.1);} 
 .tab:hover{transform:scale(1.05);} 
 .content {max-width:900px; margin:auto; margin-top:30px; display:none;} 
 .content section {background:white; padding:20px; border-radius:12px; margin-bottom:15px; box-shadow:0 3px 12px rgba(0,0,0,0.06);} 
</style>
</head>
<body>
<div id="login" class="center">
 <div class="login-box">
  <h2>Iniciar Sesión</h2>
  <input id="loginEmail" placeholder="Email">
  <input id="loginId" placeholder="ID">
  <input type="password" id="loginPass" placeholder="Contraseña">
  <button onclick="login()">Entrar</button>
 </div>
</div>

<div id="main">
 <h1>Secciones</h1>
 <div class="tabs">
  <div class="tab" onclick="openTab('crist')">Cristianismo</div>
  <div class="tab" onclick="openTab('islam')">Islam</div>
  <div class="tab" onclick="openTab('hindu')">Hinduismo</div>
  <div class="tab" onclick="openTab('budismo')">Budismo</div>
 </div>

 <div id="crist" class="content">REEMPLAZARÁ</div>
 <div id="islam" class="content">REEMPLAZARÁ</div>
 <div id="hindu" class="content">REEMPLAZARÁ</div>
 <div id="budismo" class="content">REEMPLAZARÁ</div>
</div>

<script>
function login(){document.getElementById('login').style.display='none'; document.getElementById('main').style.display='block';}
function openTab(id){document.querySelectorAll('.content').forEach(c=>c.style.display='none'); document.getElementById(id).style.display='block';}
</script>
</body>
</html>
