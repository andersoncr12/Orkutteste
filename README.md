<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Orkut CR</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>

<style>
body{margin:0;font-family:Arial;background:#e5e9f0}
header{background:#3b5998;color:#fff;padding:10px;display:flex;justify-content:space-between;align-items:center}
#login,#app{padding:15px}
input,button{width:100%;padding:10px;margin:5px 0}
button{background:#3b5998;color:#fff;border:0;border-radius:5px}
.user{background:#fff;padding:10px;margin:5px 0;border-radius:5px}
#chat{position:fixed;bottom:0;left:0;right:0;background:#fff;border-top:1px solid #ccc}
#messages{height:150px;overflow:auto}
</style>
</head>

<body>

<header>
  <strong>Orkut CR</strong>
  <span id="userEmail"></span>
</header>

<div id="login">
  <h3>Entrar</h3>
  <input id="email" placeholder="Email">
  <input id="senha" type="password" placeholder="Senha">
  <button onclick="entrar()">Entrar / Criar conta</button>
</div>

<div id="app" style="display:none">
  <h3>👥 Pessoas Online</h3>
  <div id="usuarios"></div>

  <div id="chat">
    <div id="messages"></div>
    <input id="msg" placeholder="Digite sua mensagem">
    <button onclick="enviar()">Enviar</button>
  </div>
</div>

<script>
const firebaseConfig = {
 apiKey: "AIzaSyBfT8t5ULX1ts1obxmxVw8LB6KYKhvcmtE",
 authDomain: "cr-3010d.firebaseapp.com",
 projectId: "cr-3010d",
 storageBucket: "cr-3010d.firebasestorage.app",
 messagingSenderId: "533505316102",
 appId: "1:533505316102:web:451032ecf61be0162b7a87"
};

firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.firestore();

function entrar(){
 const email=email.value;
 const senha=senha.value;
 auth.signInWithEmailAndPassword(email,senha)
 .catch(()=>auth.createUserWithEmailAndPassword(email,senha));
}

auth.onAuthStateChanged(user=>{
 if(user){
   login.style.display="none";
   app.style.display="block";
   userEmail.innerText=user.email;
   db.collection("online").doc(user.uid).set({email:user.email});
   listarUsuarios();
   ouvirMensagens();
 }
});

function listarUsuarios(){
 db.collection("online").onSnapshot(s=>{
  usuarios.innerHTML="";
  s.forEach(doc=>{
   usuarios.innerHTML+=`<div class="user">${doc.data().email}</div>`;
  });
 });
}

function enviar(){
 if(msg.value.trim()=="")return;
 db.collection("chat").add({
  email:auth.currentUser.email,
  texto:msg.value,
  time:Date.now()
 });
 msg.value="";
}

function ouvirMensagens(){
 db.collection("chat").orderBy("time").onSnapshot(s=>{
  messages.innerHTML="";
  s.forEach(d=>{
   messages.innerHTML+=`<div><b>${d.data().email}:</b> ${d.data().texto}</div>`;
  });
  messages.scrollTop=messages.scrollHeight;
 });
}
</script>

</body>
</html>
 # Orkutteste
