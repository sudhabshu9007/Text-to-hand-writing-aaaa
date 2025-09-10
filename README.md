<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>🔥 HotLinkUp</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #ec4899, #dc2626);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    .card {
      background: rgba(0,0,0,0.7);
      padding: 20px;
      border-radius: 16px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.3);
      width: 100%;
      max-width: 400px;
    }
    .card h1, .card h2 {
      text-align: center;
    }
    input, button {
      padding: 10px;
      margin-top: 10px;
      border-radius: 10px;
      border: none;
      font-size: 14px;
    }
    input {
      color: black;
    }
    button {
      background: black;
      color: white;
      cursor: pointer;
    }
    .chat-box {
      background: white;
      color: black;
      border-radius: 10px;
      height: 250px;
      overflow-y: auto;
      padding: 10px;
      margin: 10px 0;
    }
    .locked {
      text-align: center;
      color: #dc2626;
      font-weight: bold;
    }
    .disclaimer {
      font-size: 10px;
      text-align: center;
      margin-top: 8px;
      color: #ccc;
    }
    .chat-input {
      display: flex;
      gap: 5px;
      margin-top: 5px;
    }
    .chat-input input[type=text] {
      flex: 1;
    }
    .chat-input input[type=file] {
      flex: 1;
    }
    img.chat-img {
      max-width: 100%;
      border-radius: 8px;
      margin-top: 5px;
    }
  </style>
</head>
<body>
  <div class="card" id="loginCard">
    <h1>🔥 HotLinkUp</h1>
    <p style="text-align:center;">No signup, just vibes ✨</p>
    <input type="text" id="username" placeholder="Enter Nickname"/>
    <button onclick="login()">Enter Chat 🚀</button>
  </div>

  <div class="card" id="chatCard" style="display:none;">
    <h2>Chat Room 💬</h2>
    <div class="chat-box" id="chatBox"></div>
    <div id="lockMsg" class="locked" style="display:none;">
      🚫 Free chats finished. Unlock with ₹30/month.
    </div>
    <div id="chatInput">
      <div class="chat-input">
        <input type="text" id="msgInput" placeholder="Type a message..."/>
        <button onclick="sendMessage()">Send</button>
      </div>
      <div class="chat-input">
        <input type="file" id="imgInput" accept="image/*"/>
        <button onclick="sendImage()">Send Img</button>
      </div>
    </div>
    <p class="disclaimer">First 10 chats free • Data use at your own risk (cookies)</p>
  </div>

  <script>
    let username = "";
    let msgCount = 0;
    const messages = [];

    function login() {
      const input = document.getElementById("username").value.trim();
      if (input !== "") {
        username = input;
        document.getElementById("loginCard").style.display = "none";
        document.getElementById("chatCard").style.display = "block";
      }
    }

    function sendMessage() {
      if (msgCount >= 10) return;
      const input = document.getElementById("msgInput");
      const text = input.value.trim();
      if (text !== "") {
        messages.push({ user: username, text });
        msgCount++;
        updateChat();
        input.value = "";
        checkLimit();
      }
    }

    function sendImage() {
      if (msgCount >= 10) return;
      const fileInput = document.getElementById("imgInput");
      const file = fileInput.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          messages.push({ user: username, img: e.target.result });
          msgCount++;
          updateChat();
          checkLimit();
        };
        reader.readAsDataURL(file);
        fileInput.value = "";
      }
    }

    function updateChat() {
      const chatBox = document.getElementById("chatBox");
      chatBox.innerHTML = messages.map(m => {
        if (m.text) {
          return `<p><b>${m.user}:</b> ${m.text}</p>`;
        } else if (m.img) {
          return `<p><b>${m.user}:</b><br><img src=\"${m.img}\" class=\"chat-img\"/></p>`;
        }
      }).join(" ");
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function checkLimit() {
      if (msgCount >= 10) {
        document.getElementById("chatInput").style.display = "none";
        document.getElementById("lockMsg").style.display = "block";
      }
    }
  </script>
</body>
</html>
