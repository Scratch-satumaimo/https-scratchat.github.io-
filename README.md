# https-scratchat.github.io-
chat site
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>リアルタイムチャット</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
      margin: 0;
      padding: 0;
    }

    #chat-container {
      max-width: 600px;
      margin: 20px auto;
      background-color: #ffffff;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      padding: 20px;
    }

    .message {
      padding: 10px;
      border-bottom: 1px solid #e0e0e0;
    }

    .message p {
      margin: 0;
    }

    .message .time {
      font-size: 0.8rem;
      color: #888888;
    }

    .input-container {
      display: flex;
      align-items: center;
      background-color: #f0f0f0;
      padding: 10px;
    }

    .input-container input {
      flex: 1;
      padding: 8px;
      border: none;
      border-radius: 5px;
    }

    .input-container button {
      margin-left: 10px;
      padding: 8px 15px;
      background-color: #4CAF50;
      color: #ffffff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .input-container button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>
  <div id="chat-container">
    <div id="messages">
      <!-- Chat messages will appear here -->
    </div>

    <div class="input-container">
      <input type="text" id="username-input" placeholder="ユーザー名を入力してください...">
      <input type="text" id="message-input" placeholder="メッセージを入力してください...">
      <button onclick="signInWithGoogle()">Googleアカウントでサインイン</button>
      <button onclick="sendMessage()" disabled>送信</button>
    </div>
  </div>

  <script src="https://apis.google.com/js/platform.js" async defer></script>
  <script>
    let userDisplayName = null;
    const messagesContainer = document.getElementById('messages');
    const usernameInput = document.getElementById('username-input');
    const messageInput = document.getElementById('message-input');
    const signInButton = document.querySelector('button[onclick="signInWithGoogle()"]');
    const sendButton = document.querySelector('button[onclick="sendMessage()"]');

    function addMessage(text, sender) {
      const messageDiv = document.createElement('div');
      messageDiv.classList.add('message');

      const senderTag = sender ? `<strong>${sender}</strong>: ` : '';
      const messageText = `<p>${senderTag}${text}</p>`;
      const messageTime = `<span class="time">${getTime()}</span>`;

      messageDiv.innerHTML = messageText + messageTime;
      messagesContainer.appendChild(messageDiv);
    }

    function getTime() {
      const now = new Date();
      return `${now.getHours()}:${String(now.getMinutes()).padStart(2, '0')}`;
    }

    function signInWithGoogle() {
      gapi.load('auth2', function() {
        gapi.auth2.init({
          client_id: 'YOUR_GOOGLE_CLIENT_ID'
        }).then(function(auth2) {
          auth2.signIn().then(function(googleUser) {
            const profile = googleUser.getBasicProfile();
            userDisplayName = profile.getName();
            usernameInput.value = userDisplayName;
            signInButton.disabled = true;
            sendButton.disabled = false;
            messageInput.focus();
          });
        });
      });
    }

    function sendMessage() {
      const messageText = messageInput.value.trim();
      if (messageText !== '') {
        const sender = usernameInput.value.trim() || 'Guest';
        addMessage(messageText, sender);
        messageInput.value = '';
        messageInput.focus();
      }
    }

    messageInput.addEventListener('keydown', (event) => {
      if (event.key === 'Enter') {
        sendMessage();
      }
    });
  </script>
</body>
</html>
