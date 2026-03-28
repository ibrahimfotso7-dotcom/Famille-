<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Chat Famille</title>
  <style>
    body { font-family: Arial; background:#f2f2f2; margin:0; }
    .chat-box { width:100%; max-width:600px; margin:auto; padding:10px; }
    #messages {
      height: 70vh;
      overflow-y: scroll;
      background:white;
      padding:10px;
      border-radius:10px;
    }
    .msg { margin:5px 0; padding:8px; background:#e1f5fe; border-radius:8px; }
    .input-box { display:flex; margin-top:10px; }
    input { flex:1; padding:10px; }
    button { padding:10px; }
  </style>
</head>
<body>

<div class="chat-box">
  <h2>💬 Chat de Famille</h2>

  <div id="messages"></div>

  <div class="input-box">
    <input id="name" placeholder="Ton nom">
  </div>

  <div class="input-box">
    <input id="message" placeholder="Écris un message...">
    <button onclick="sendMessage()">Envoyer</button>
  </div>
</div>

<!-- Firebase -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
  import { getDatabase, ref, push, onValue } 
  from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

  // 🔴 CONFIG FIREBASE (À REMPLACER)
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT.appspot.com",
    messagingSenderId: "XXXX",
    appId: "XXXX"
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);
  const messagesRef = ref(db, "messages");

  window.sendMessage = function () {
    const name = document.getElementById("name").value;
    const message = document.getElementById("message").value;

    if (name === "" || message === "") return;

    push(messagesRef, {
      name: name,
      message: message,
      time: Date.now()
    });

    document.getElementById("message").value = "";
  }

  onValue(messagesRef, (snapshot) => {
    const data = snapshot.val();
    const box = document.getElementById("messages");
    box.innerHTML = "";

    for (let id in data) {
      const msg = data[id];
      box.innerHTML += `
        <div class="msg">
          <b>${msg.name}</b>: ${msg.message}
        </div>
      `;
    }

    box.scrollTop = box.scrollHeight;
  });

</script>

</body>
</html>
