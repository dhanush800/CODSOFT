<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>AI Intern Chatbot</title>
<style>
  /* Dark background & teal accents */
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #121212;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
  }
  .chat-container {
    width: 450px;
    background: #1E1E1E;
    border-radius: 12px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.5);
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }
  .chat-header {
    background: #009688;
    color: #fff;
    padding: 15px;
    font-size: 20px;
    text-align: center;
  }
  .chat-log {
    flex: 1;
    padding: 15px;
    overflow-y: auto;
  }
  .chat-log .bot, .chat-log .user {
    max-width: 75%;
    padding: 10px 14px;
    margin: 6px 0;
    border-radius: 20px;
    word-wrap: break-word;
  }
  .chat-log .bot {
    background: #004D40;
    color: #fff;
    align-self: flex-start;
  }
  .chat-log .user {
    background: #00796B;
    color: #fff;
    align-self: flex-end;
  }
  .chat-input {
    display: flex;
    border-top: 1px solid #333;
    padding: 10px;
    background: #1E1E1E;
  }
  .chat-input input {
    flex: 1;
    padding: 10px 14px;
    border-radius: 20px;
    border: none;
    outline: none;
    font-size: 16px;
    background: #2C2C2C;
    color: #fff;
  }
  .chat-input button {
    margin-left: 10px;
    padding: 10px 16px;
    border-radius: 20px;
    border: none;
    background: #009688;
    color: #fff;
    font-weight: bold;
    cursor: pointer;
    transition: 0.3s;
  }
  .chat-input button:hover {
    background: #00796B;
  }
</style>
</head>
<body>
<div class="chat-container">
  <div class="chat-header">CodSoft AI Chatbot</div>
  <div id="chat-log" class="chat-log"></div>
  <div class="chat-input">
    <input id="msg" type="text" placeholder="Type your message..." />
    <button id="send">Send</button>
  </div>
</div>

<script>
  const chatLog = document.getElementById('chat-log');
  const msgInput = document.getElementById('msg');
  const sendBtn = document.getElementById('send');

  const rules = [
    {pattern: /hi|hello|hey/i, replies: ["Hello! How can I help you today?", "Hi there! How are you?", "Hey! Ready to chat!"]},
    {pattern: /how are you/i, replies: ["I'm doing great, thanks!", "All systems operational!", "Feeling AImazing!"]},
    {pattern: /your name|who are you/i, replies: ["I am CodSoft AI Chatbot!", "Your friendly AI assistant from CodSoft.", "Chatbot powered by AI internship demo."]},
    {pattern: /thank|thanks/i, replies: ["You're welcome!", "No problem!", "Glad I could help!"]},
    {pattern: /bye|goodbye/i, replies: ["Goodbye! Have a nice day.", "See you later!", "Talk to you soon!"]},
    {pattern: /help|task/i, replies: ["I can answer your questions about the internship tasks.", "Ask me about AI tasks I did.", "I’m here to guide you through my AI internship demo."]},
    {pattern: /chatbot|tic tac toe|recommendation|image/i, replies: ["I worked on a chatbot, Tic Tac Toe AI, recommendation system, and image captioning.", "I can tell you about all my 4 AI tasks!", "Ask me details about any AI project I completed."]}
  ];

  function getReply(input) {
    for (const rule of rules) {
      if (rule.pattern.test(input)) {
        const r = rule.replies[Math.floor(Math.random() * rule.replies.length)];
        return r;
      }
    }
    return "Sorry, I don't understand. Can you rephrase?";
  }

  function appendMessage(text, sender) {
    const div = document.createElement('div');
    div.className = sender;
    div.textContent = text;
    chatLog.appendChild(div);
    chatLog.scrollTop = chatLog.scrollHeight;
  }

  sendBtn.addEventListener('click', () => {
    const msg = msgInput.value.trim();
    if (!msg) return;
    appendMessage(msg, 'user');
    const reply = getReply(msg);
    setTimeout(() => appendMessage(reply, 'bot'), 300);
    msgInput.value = '';
    msgInput.focus();
  });

  msgInput.addEventListener('keydown', (e) => { if (e.key === 'Enter') sendBtn.click(); });

  // initial greeting
  appendMessage("Hello! I'm your CodSoft AI Chatbot. Type 'hi' to start.", 'bot');
</script>
</body>
</html>


