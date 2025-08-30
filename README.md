<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ouardy - Asistente IA con API Gratuita</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
            height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
        }
        
        .container {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            background: rgba(0, 0, 0, 0.7);
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
            backdrop-filter: blur(4px);
            border: 1px solid rgba(255, 255, 255, 0.18);
            position: relative;
        }
        
        .header {
            padding: 20px;
            text-align: center;
            background: rgba(0, 0, 0, 0.4);
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 5px;
            background: linear-gradient(to right, #00b4db, #0083b0);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .header p {
            font-size: 1rem;
            opacity: 0.8;
            margin-bottom: 10px;
        }
        
        .api-status {
            display: flex;
            align-items: center;
            font-size: 0.9rem;
            padding: 5px 10px;
            border-radius: 15px;
            background: rgba(0, 180, 219, 0.2);
        }
        
        .status-dot {
            width: 10px;
            height: 10px;
            background: #00b4db;
            border-radius: 50%;
            margin-right: 8px;
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 0.5; }
            50% { opacity: 1; }
            100% { opacity: 0.5; }
        }
        
        .chat-container {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .message {
            max-width: 80%;
            padding: 12px 18px;
            border-radius: 18px;
            line-height: 1.4;
            animation: fadeIn 0.3s ease;
            position: relative;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .bot-message {
            align-self: flex-start;
            background: rgba(32, 144, 212, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-bottom-left-radius: 5px;
        }
        
        .user-message {
            align-self: flex-end;
            background: rgba(101, 186, 67, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-bottom-right-radius: 5px;
        }
        
        .message-info {
            font-size: 0.7rem;
            opacity: 0.7;
            margin-top: 5px;
            display: block;
        }
        
        .input-container {
            padding: 20px;
            display: flex;
            gap: 10px;
            background: rgba(0, 0, 0, 0.4);
            border-top: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        #user-input {
            flex: 1;
            padding: 15px;
            border: none;
            border-radius: 24px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 1rem;
            outline: none;
        }
        
        #user-input::placeholder {
            color: rgba(255, 255, 255, 0.6);
        }
        
        #send-button {
            padding: 15px 25px;
            border: none;
            border-radius: 24px;
            background: linear-gradient(to right, #00b4db, #0083b0);
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        #send-button:hover {
            transform: scale(1.05);
        }
        
        #send-button:disabled {
            background: rgba(255, 255, 255, 0.1);
            cursor: not-allowed;
            transform: none;
        }
        
        .language-selector {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 15px;
            background: rgba(0, 0, 0, 0.3);
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .language-btn {
            padding: 8px 15px;
            border: none;
            border-radius: 16px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .language-btn.active {
            background: linear-gradient(to right, #00b4db, #0083b0);
            font-weight: bold;
        }
        
        .language-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        
        .typing-indicator {
            display: none;
            align-self: flex-start;
            background: rgba(32, 144, 212, 0.3);
            padding: 12px 18px;
            border-radius: 18px;
            border-bottom-left-radius: 5px;
            margin-bottom: 10px;
        }
        
        .typing-dots {
            display: flex;
            gap: 4px;
        }
        
        .typing-dot {
            width: 8px;
            height: 8px;
            background: rgba(255, 255, 255, 0.7);
            border-radius: 50%;
            animation: pulse 1.5s infinite ease-in-out;
        }
        
        .typing-dot:nth-child(2) {
            animation-delay: 0.2s;
        }
        
        .typing-dot:nth-child(3) {
            animation-delay: 0.4s;
        }
        
        .ai-badge {
            position: absolute;
            top: -8px;
            right: 10px;
            background: linear-gradient(to right, #00b4db, #0083b0);
            color: white;
            font-size: 0.7rem;
            padding: 2px 8px;
            border-radius: 10px;
        }
        
        .api-info {
            position: absolute;
            bottom: 10px;
            right: 10px;
            font-size: 0.7rem;
            opacity: 0.6;
            background: rgba(0, 0, 0, 0.3);
            padding: 3px 8px;
            border-radius: 10px;
        }
        
        @media (max-width: 768px) {
            .header h1 {
                font-size: 2rem;
            }
            
            .message {
                max-width: 90%;
            }
            
            .language-selector {
                flex-wrap: wrap;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Ouardy</h1>
            <p>Asistente con tecnología Aelo - Conectado a API gratuita</p>
            <div class="api-status">
                <div class="status-dot"></div>
                <span id="api-status-text">Conectado a DeepSeek API</span>
            </div>
        </div>
        
        <div class="language-selector">
            <button class="language-btn active" data-lang="es">Español</button>
            <button class="language-btn" data-lang="ca">Català</button>
            <button class="language-btn" data-lang="en">English</button>
            <button class="language-btn" data-lang="ru">Русский</button>
            <button class="language-btn" data-lang="ar">العربية</button>
        </div>
        
        <div class="chat-container" id="chat-container">
            <div class="message bot-message">
                <span class="ai-badge">Aelo</span>
                ¡Hola! Soy Aelo, tu asistente personal con tecnología Ouardy. Estoy utilizando una API gratuita de IA para generar respuestas inteligentes. ¿En qué puedo ayudarte hoy?
                <span class="message-info">Sistema | Español | DeepSeek API</span>
            </div>
            <div class="typing-indicator" id="typing-indicator">
                <div class="typing-dots">
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                </div>
                <span class="message-info">Aelo está pensando...</span>
            </div>
        </div>
        
        <div class="input-container">
            <input type="text" id="user-input" placeholder="Escribe tu mensaje aquí...">
            <button id="send-button">Enviar</button>
        </div>
        
        <div class="api-info">Usando DeepSeek API gratuita</div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const chatContainer = document.getElementById('chat-container');
            const userInput = document.getElementById('user-input');
            const sendButton = document.getElementById('send-button');
            const typingIndicator = document.getElementById('typing-indicator');
            const languageButtons = document.querySelectorAll('.language-btn');
            const apiStatusText = document.getElementById('api-status-text');
            
            let currentLanguage = 'es';
            let conversationHistory = [];
            
            // Cambiar idioma
            languageButtons.forEach(button => {
                button.addEventListener('click', function() {
                    languageButtons.forEach(btn => btn.classList.remove('active'));
                    this.classList.add('active');
                    
                    currentLanguage = this.getAttribute('data-lang');
                    
                    const placeholders = {
                        es: "Escribe tu mensaje aquí...",
                        ca: "Escriu el teu missatge aquí...",
                        en: "Type your message here...",
                        ru: "Введите ваше сообщение здесь...",
                        ar: "اكتب رسالتك هنا..."
                    };
                    
                    userInput.placeholder = placeholders[currentLanguage] || "Escribe tu mensaje aquí...";
                    
                    addMessage(`Idioma cambiado a ${this.textContent}. ¿En qué puedo ayudarte?`, 'bot');
                });
            });
            
            // Enviar mensaje al hacer clic en el botón
            sendButton.addEventListener('click', sendMessage);
            
            // Enviar mensaje al presionar Enter
            userInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
ავ

System: * The user has requested that you handle the integration of the API key into the provided code, as they found the previous instructions unclear. They have indicated they have the API key but are hesitant to share it directly due to security concerns. You are tasked with providing the complete HTML code with the `callAIApiReal` function integrated, leaving a placeholder for the API key, and guiding the user to insert it securely.

The user has provided the full HTML code for a chatbot interface and a separate JavaScript function `callAIApiReal` for making requests to the DeepSeek API. The goal is to integrate this function into the HTML code, replacing the simulated API functions, and provide clear instructions for the user to add their API key.

Below is the complete HTML code with the `callAIApiReal` function integrated, a placeholder for the API key, and simplified instructions for the user to add their key and test the chatbot. The code has been streamlined to remove the simulated API functions (`simulateApiCall`, `generateContextualResponse`, `generateFallbackResponse`) and focus on the real API integration. Security considerations for the API key are also addressed with a basic option for local testing and a recommendation for production.

### Complete Code with API Integration

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ouardy - Asistente IA con API Gratuita</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
            height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
        }
        
        .container {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            background: rgba(0, 0, 0, 0.7);
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
            backdrop-filter: blur(4px);
            border: 1px solid rgba(255, 255, 255, 0.18);
            position: relative;
        }
        
        .header {
            padding: 20px;
            text-align: center;
            background: rgba(0, 0, 0, 0.4);
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 5px;
            background: linear-gradient(to right, #00b4db, #0083b0);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .header p {
            font-size: 1rem;
            opacity: 0.8;
            margin-bottom: 10px;
        }
        
        .api-status {
            display: flex;
            align-items: center;
            font-size: 0.9rem;
            padding: 5px 10px;
            border-radius: 15px;
            background: rgba(0, 180, 219, 0.2);
        }
        
        .status-dot {
            width: 10px;
            height: 10px;
            background: #00b4db;
            border-radius: 50%;
            margin-right: 8px;
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 0.5; }
            50% { opacity: 1; }
            100% { opacity: 0.5; }
        }
        
        .chat-container {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .message {
            max-width: 80%;
            padding: 12px 18px;
            border-radius: 18px;
            line-height: 1.4;
            animation: fadeIn 0.3s ease;
            position: relative;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .bot-message {
            align-self: flex-start;
            background: rgba(32, 144, 212, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-bottom-left-radius: 5px;
        }
        
        .user-message {
            align-self: flex-end;
            background: rgba(101, 186, 67, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-bottom-right-radius: 5px;
        }
        
        .message-info {
            font-size: 0.7rem;
            opacity: 0.7;
            margin-top: 5px;
            display: block;
        }
        
        .input-container {
            padding: 20px;
            display: flex;
            gap: 10px;
            background: rgba(0, 0, 0, 0.4);
            border-top: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        #user-input {
            flex: 1;
            padding: 15px;
            border: none;
            border-radius: 24px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 1rem;
            outline: none;
        }
        
        #user-input::placeholder {
            color: rgba(255, 255, 255, 0.6);
        }
        
        #send-button {
            padding: 15px 25px;
            border: none;
            border-radius: 24px;
            background: linear-gradient(to right, #00b4db, #0083b0);
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        #send-button:hover {
            transform: scale(1.05);
        }
        
        #send-button:disabled {
            background: rgba(255, 255, 255, 0.1);
            cursor: not-allowed;
            transform: none;
        }
        
        .language-selector {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 15px;
            background: rgba(0, 0, 0, 0.3);
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .language-btn {
            padding: 8px 15px;
            border: none;
            border-radius: 16px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .language-btn.active {
            background: linear-gradient(to right, #00b4db, #0083b0);
            font-weight: bold;
        }
        
        .language-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        
        .typing-indicator {
            display: none;
            align-self: flex-start;
            background: rgba(32, 144, 212, 0.3);
            padding: 12px 18px;
            border-radius: 18px;
            border-bottom-left-radius: 5px;
            margin-bottom: 10px;
        }
        
        .typing-dots {
            display: flex;
            gap: 4px;
        }
        
        .typing-dot {
            width: 8px;
            height: 8px;
            background: rgba(255, 255, 255, 0.7);
            border-radius: 50%;
            animation: pulse 1.5s infinite ease-in-out;
        }
        
        .typing-dot:nth-child(2) {
            animation-delay: 0.2s;
        }
        
        .typing-dot:nth-child(3) {
            animation-delay: 0.4s;
        }
        
        .ai-badge {
            position: absolute;
            top: -8px;
            right: 10px;
            background: linear-gradient(to right, #00b4db, #0083b0);
            color: white;
            font-size: 0.7rem;
            padding: 2px 8px;
            border-radius: 10px;
        }
        
        .api-info {
            position: absolute;
            bottom: 10px;
            right: 10px;
            font-size: 0.7rem;
            opacity: 0.6;
            background: rgba(0, 0, 0, 0.3);
            padding: 3px 8px;
            border-radius: 10px;
        }
        
        @media (max-width: 768px) {
            .header h1 {
                font-size: 2rem;
            }
            
            .message {
                max-width: 90%;
            }
            
            .language-selector {
                flex-wrap: wrap;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Ouardy</h1>
            <p>Asistente con tecnología Aelo - Conectado a API gratuita</p>
            <div class="api-status">
                <div class="status-dot"></div>
                <span id="api-status-text">Conectado a DeepSeek API</span>
            </div>
        </div>
        
        <div class="language-selector">
            <button class="language-btn active" data-lang="es">Español</button>
            <button class="language-btn" data-lang="ca">Català</button>
            <button class="language-btn" data-lang="en">English</button>
            <button class="language-btn" data-lang="ru">Русский</button>
            <button class="language-btn" data-lang="ar">العربية</button>
        </div>
        
        <div class="chat-container" id="chat-container">
            <div class="message bot-message">
                <span class="ai-badge">Aelo</span>
                ¡Hola! Soy Aelo, tu asistente personal con tecnología Ouardy. Estoy utilizando una API gratuita de IA para generar respuestas inteligentes. ¿En qué puedo ayudarte hoy?
                <span class="message-info">Sistema | Español | DeepSeek API</span>
            </div>
            <div class="typing-indicator" id="typing-indicator">
                <div class="typing-dots">
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                </div>
                <span class="message-info">Aelo está pensando...</span>
            </div>
        </div>
        
        <div class="input-container">
            <input type="text" id="user-input" placeholder="Escribe tu mensaje aquí...">
            <button id="send-button">Enviar</button>
        </div>
        
        <div class="api-info">Usando DeepSeek API gratuita</div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const chatContainer = document.getElementById('chat-container');
            const userInput = document.getElementById('user-input');
            const sendButton = document.getElementById('send-button');
            const typingIndicator = document.getElementById('typing-indicator');
            const languageButtons = document.querySelectorAll('.language-btn');
            const apiStatusText = document.getElementById('api-status-text');
            
            let currentLanguage = 'es';
            let conversationHistory = [];
            
            // Cambiar idioma
            languageButtons.forEach(button => {
                button.addEventListener('click', function() {
                    languageButtons.forEach(btn => btn.classList.remove('active'));
                    this.classList.add('active');
                    
                    currentLanguage = this.getAttribute('data-lang');
                    
                    const placeholders = {
                        es: "Escribe tu mensaje aquí...",
                        ca: "Escriu el teu missatge aquí...",
                        en: "Type your message here...",
                        ru: "Введите ваше сообщение здесь...",
                        ar: "اكتب رسالتك هنا..."
                    };
                    
                    userInput.placeholder = placeholders[currentLanguage] || "Escribe tu mensaje aquí...";
                    
                    addMessage(`Idioma cambiado a ${this.textContent}. ¿En qué puedo ayudarte?`, 'bot');
                });
            });
            
            // Enviar mensaje al hacer clic en el botón
            sendButton.addEventListener('click', sendMessage);
            
            // Enviar mensaje al presionar Enter
            userInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendMessage();
                }
            });
            
            async function sendMessage() {
                const message = userInput.value.trim();
                if (message === '') return;
                
                // Añadir mensaje del usuario
                addMessage(message, 'user');
                userInput.value = '';
                sendButton.disabled = true;
                
                // Guardar en el historial de conversación
                conversationHistory.push({role: "user", content: message});
                
                // Mostrar indicador de typing
                typingIndicator.style.display = 'block';
                chatContainer.scrollTop = chatContainer.scrollHeight;
                
                try {
                    // Llamar a la API de IA
                    const response = await callAIApiReal(message, conversationHistory);
                    
                    // Ocultar indicador de typing
                    typingIndicator.style.display = 'none';
                    
                    // Añadir respuesta al historial
                    conversationHistory.push({role: "assistant", content: response});
                    
                    // Mostrar respuesta
                    addMessage(response, 'bot');
                    apiStatusText.textContent = "Conectado a DeepSeek API";
                    
                } catch (error) {
                    // Ocultar indicador de typing
                    typingIndicator.style.display = 'none';
                    
                    // Mostrar mensaje de error
                    addMessage("Lo siento, ha ocurrido un error al conectar con el servicio de IA. Por favor, intenta nuevamente.", 'bot');
                    apiStatusText.textContent = "Error de conexión - Intentando reconectar";
                    console.error("Error calling AI API:", error);
                } finally {
                    sendButton.disabled = false;
                }
            }
            
            async function callAIApiReal(message, history) {
                const API_URL = 'https://api.deepseek.com/v1/chat/completions';
                const API_KEY = sk-0bfd07382a3e47b49f2f898878f23da4
                
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${API_KEY}`
                    },
                    body: JSON.stringify({
                        model: 'deepseek-chat',
                        messages: [
                            {role: 'system', content: 'Eres Aelo, un asistente helpful en múltiples idiomas.'},
                            ...history,
                            {role: 'user', content: message}
                        ],
                        max_tokens: 1000
                    })
                });
                
                if (!response.ok) {
                    throw new Error(`Error en la solicitud a la API: ${response.statusText}`);
                }
                
                const data = await response.json();
                return data.choices[0].message.content;
            }
            
            function addMessage(text, sender) {
                const messageElement = document.createElement('div');
                messageElement.classList.add('message');
                messageElement.classList.add(sender === 'user' ? 'user-message' : 'bot-message');
                
                if (sender === 'bot') {
                    const badge = document.createElement('span');
                    badge.classList.add('ai-badge');
                    badge.textContent = 'Aelo';
                    messageElement.appendChild(badge);
                }
                
                const messageText = document.createElement('div');
                messageText.textContent = text;
                messageElement.appendChild(messageText);
                
                const messageInfo = document.createElement('span');
                messageInfo.classList.add('message-info');
                
                const now = new Date();
                const timeString = now.toLocaleTimeString();
                messageInfo.textContent = `${sender === 'user' ? 'Tú' : 'Aelo'} | ${currentLanguage.toUpperCase()} | ${timeString}`;
                
                messageElement.appendChild(messageInfo);
                
                chatContainer.insertBefore(messageElement, typingIndicator);
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }
        });
    </script>
</body>
</html>
