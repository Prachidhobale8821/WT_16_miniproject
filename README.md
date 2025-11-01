<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chat Application</title>
    <link rel="stylesheet" href="style1.css">
</head>
<body>
    <div class="chat-container">
        <div class="chat-header">
            Chat Application
        </div>
        <div class="chat-messages" id="chatMessages">
            <div class="message bot">
                <div class="message-bubble">
                    Hello! Welcome to chat application. Type a message to get started!
                    <div class="message-time" id="welcomeTime"></div>
                </div>
            </div>
        </div>
        <div class="chat-input-container">
            <input 
                type="text" 
                id="messageInput" 
                placeholder="Type your message" 
                autocomplete="off"
            >
            <button id="sendButton">Send</button>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
style1.css

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #e3e1ac 0%, #dddcb0 100%);
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.chat-container {
    width: 100%;
    max-width: 500px;
    height: 600px;
    background: white;
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

.chat-header {
    background:#795a32;
    color: white;
    padding: 20px;
    text-align: center;
    font-size: 24px;
    font-weight: bold;
}

.chat-messages {
    flex: 1;
    padding: 20px;
    overflow-y: auto;
    background: #f5f5f5;
}

.message {
    margin-bottom: 15px;
    display: flex;
    animation: slideIn 0.3s ease;
}

@keyframes slideIn {
    from {
        opacity: 0;
        transform: translateY(10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.message.user {
    justify-content: flex-end;
}

.message.bot {
    justify-content: flex-start;
}

.message-bubble {
    max-width: 70%;
    padding: 12px 16px;
    word-wrap: break-word;
}

.message.user .message-bubble {
    background:white;
    color: black;
}

.message.bot .message-bubble {
    background: white;
    color: #333;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.message-time {
    font-size: 11px;
    opacity: 0.7;
    margin-top: 5px;
}

.chat-input-container {
    padding: 20px;
    background: white;
    border-top: 1px solid #e0e0e0;
    display: flex;
    gap: 10px;
}

#messageInput {
    flex: 1;
    padding: 12px 16px;
    border: 2px solid #e0e0e0;
    font-size: 14px;
    outline: none;
}



#sendButton {
    padding: 12px 24px;
    background: #795a32;
    color: white;
    border: none;
    cursor: pointer;
    font-size: 14px;
    font-weight: bold;
}



#sendButton:active {
    transform: translateY(0);
}

.chat-messages::-webkit-scrollbar {
    width: 8px;
}

.chat-messages::-webkit-scrollbar-track {
    background: #f1f1f1;
}

.chat-messages::-webkit-scrollbar-thumb {
    background: #888;
}

.chat-messages::-webkit-scrollbar-thumb:hover {
    background: #555;
}
const chatMessages = document.getElementById('chatMessages');
const messageInput = document.getElementById('messageInput');
const sendButton = document.getElementById('sendButton');

let messageCount = 0;
const maxMessages = 10;


document.getElementById('welcomeTime').textContent = getCurrentTime();


const botResponses = [
    "That's interesting! Tell me more.",
    "I see what you mean!",
    "Thanks for sharing that with me!",
    "That's a great point!",
    "How has your day been?"
];

function getCurrentTime() {
    const now = new Date();
    return now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
}

function createMessage(text, type) {
    const messageDiv = document.createElement('div');
    messageDiv.className = message ${type};
    
    const bubbleDiv = document.createElement('div');
    bubbleDiv.className = 'message-bubble';
    bubbleDiv.innerHTML = `
        ${text}
        <div class="message-time">${getCurrentTime()}</div>
    `;
    
    messageDiv.appendChild(bubbleDiv);
    return messageDiv;
}


function addMessage(text, type) {
    const message = createMessage(text, type);
    chatMessages.appendChild(message);
    chatMessages.scrollTop = chatMessages.scrollHeight;
}

function getBotResponse() {
    if (messageCount >= maxMessages) {
        return "I've reached my message limit for this session. Please refresh the page to start a new conversation.";
    }
    return botResponses[Math.floor(Math.random() * botResponses.length)];
}


function sendMessage() {
    const messageText = messageInput.value.trim();
    
    if (messageText === '') return;
    
    if (messageCount >= maxMessages) {
        addMessage("Message limit reached. Please refresh the page to continue.", 'bot');
        return;
    }
    
    
    addMessage(messageText, 'user');
    messageInput.value = '';
    messageCount++;
    

    setTimeout(() => {
        addMessage(getBotResponse(), 'bot');
    }, 500);
}


sendButton.addEventListener('click', sendMessage);

messageInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
        sendMessage();
    }
});


messageInput.focus();
