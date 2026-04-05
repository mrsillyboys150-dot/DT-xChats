<!DOCTYPE html>
<html>
<head>
  <title>Mini Chat App</title>
  <style>
    body { font-family: Arial; margin: 0; background: #f2f2f2; }
    #login, #chat { padding: 20px; }
    #chat { display: none; }
    #messages { height: 300px; overflow-y: scroll; background: white; padding: 10px; }
    .msg { margin: 5px 0; }
    input { padding: 10px; margin: 5px 0; width: 100%; }
    button { padding: 10px; width: 100%; background: green; color: white; border: none; }
  </style>
</head>
<body>

<div id="login">
  <h2>Login</h2>
  <input type="text" id="username" placeholder="Enter your name">
  <button onclick="login()">Enter Chat</button>
</div>

<div id="chat">
  <h2>Chat Room</h2>
  <div id="messages"></div>
  <input type="text" id="msgInput" placeholder="Type message...">
  <button onclick="sendMessage()">Send</button>
</div>

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js"></script>

<script>
  // 🔥 Replace with your Firebase config
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_DOMAIN",
    databaseURL: "YOUR_DB_URL",
    projectId: "YOUR_PROJECT_ID",
  };

  firebase.initializeApp(firebaseConfig);
  const db = firebase.database();

  let username = "";

  function login() {
    username = document.getElementById("username").value;
    if (!username) return alert("Enter name");

    document.getElementById("login").style.display = "none";
    document.getElementById("chat").style.display = "block";

    loadMessages();
  }

  function sendMessage() {
    const msg = document.getElementById("msgInput").value;
    if (!msg) return;

    db.ref("messages").push({
      name: username,
      text: msg
    });

    document.getElementById("msgInput").value = "";
  }

  function loadMessages() {
    db.ref("messages").on("child_added", snapshot => {
      const data = snapshot.val();

      const div = document.createElement("div");
      div.className = "msg";
      div.innerText = data.name + ": " + data.text;

      document.getElementById("messages").appendChild(div);
    });
  }
</script>

</body>
</html>
