<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ouardy - Asistente IA</title>
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
        
        #send-button, #reset-button, #history-button {
            padding: 15px 25px;
            border: none;
            border-radius: 24px;
            background: linear-gradient(to right, #00b4db, #0083b0);
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        #send-button:hover, #reset-button:hover, #history-button:hover {
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
        
        /* Estilos para la modal de historial */
        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
        }
        
        .modal-content {
            background-color: #203a43;
            margin: 15% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 80%;
            max-height: 80%;
            overflow-y: auto;
        }
        
        .close {
            color: white;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }
        
        .close:hover, .close:focus {
            color: #00b4db;
            text-decoration: none;
            cursor: pointer;
        }
        
        .history-chat {
            margin-bottom: 20px;
            padding: 10px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 10px;
        }
        
        .history-message {
            margin-bottom: 10px;
            padding: 8px;
            border-radius: 10px;
        }
        
        .history-user {
            background: rgba(101, 186, 67, 0.3);
            align-self: flex-end;
        }
        
        .history-bot {
            background: rgba(32, 144, 212, 0.3);
            align-self: flex-start;
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
            <p>Asistente con tecnología Aelo</p>
            <div class="api-status">
                <div class="status-dot"></div>
                <span id="api-status-text">Conectado a Aelo</span>
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
                <div>¡Hola! Soy Aelo, tu asistente personal con tecnología Ouardy. ¿En qué puedo ayudarte hoy?</div>
                <span class="message-info">Sistema | Español</span>
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
            <button id="reset-button">Reiniciar Chat</button>
            <button id="history-button">Historial</button>
        </div>
        
        <div class="api-info">Desarrollado por Ouardy</div>
    </div>

    <!-- Modal para historial de chats -->
    <div id="history-modal" class="modal">
        <div class="modal-content">
            <span class="close" id="close-modal">&times;</span>
            <h2>Historial de Chats</h2>
            <div id="history-list"></div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const chatContainer = document.getElementById('chat-container');
            const userInput = document.getElementById('user-input');
            const sendButton = document.getElementById('send-button');
            const resetButton = document.getElementById('reset-button');
            const historyButton = document.getElementById('history-button');
            const typingIndicator = document.getElementById('typing-indicator');
            const languageButtons = document.querySelectorAll('.language-btn');
            const apiStatusText = document.getElementById('api-status-text');
            const historyModal = document.getElementById('history-modal');
            const closeModal = document.getElementById('close-modal');
            const historyList = document.getElementById('history-list');
            
            let currentLanguage = 'es';
            let conversationHistory = [];
            let currentMessages = []; // Para guardar mensajes mostrados en el chat actual
            
            // Cargar historial de localStorage
            let chatHistory = JSON.parse(localStorage.getItem('chatHistory')) || [];
            
            // Mapa de nombres de idiomas
            const languageNames = {
                es: 'Español',
                ca: 'Català',
                en: 'English',
                ru: 'Русский',
                ar: 'العربية'
            };
            
            // Mapa de mensajes de bienvenida
            const welcomeMessages = {
                es: '¡Hola! Soy Aelo, tu asistente personal con tecnología Ouardy. ¿En qué puedo ayudarte hoy?',
                ca: 'Hola! Sóc Aelo, el teu assistent personal amb tecnologia Ouardy. Com et puc ajudar avui?',
                en: 'Hello! I am Aelo, your personal assistant with Ouardy technology. How can I help you today?',
                ru: 'Здравствуйте! Я Аэло, ваш личный помощник с технологией Ouardy. Чем могу помочь?',
                ar: 'مرحبًا! أنا أيلو، مساعدك الشخصي بتقنية Ouardy. كيف يمكنني مساعدتك اليوم؟'
            };
            
            // Mapa de placeholders
            const placeholders = {
                es: 'Escribe tu mensaje aquí...',
                ca: 'Escriu el teu missatge aquí...',
                en: 'Type your message here...',
                ru: 'Введите ваше сообщение здесь...',
                ar: 'اكتب رسالتك هنا...'
            };
            
            // Mapa de mensajes de cambio de idioma
            const languageChangeMessages = {
                es: 'Idioma cambiado a Español. ¿En qué puedo ayudarte?',
                ca: 'Idioma canviat a Català. Com et puc ajudar?',
                en: 'Language changed to English. How can I help you?',
                ru: 'Язык изменён на Русский. Чем могу помочь?',
                ar: 'تم تغيير اللغة إلى العربية. كيف يمكنني مساعدتك؟'
            };
            
            // Mapa de instrucciones de idioma para la IA
            const languageInstructions = {
                es: 'Responde en español.',
                ca: 'Respon en català.',
                en: 'Respond in English.',
                ru: 'Отвечай на русском языке.',
                ar: 'الرد بالعربية.'
            };
            
            // Mapa de mensajes de "pensando"
            const thinkingMessages = {
                es: 'Aelo está pensando...',
                ca: 'Aelo està pensant...',
                en: 'Aelo is thinking...',
                ru: 'Аэло думает...',
                ar: 'أيلو يفكر...'
            };
            
            // Mapa de mensajes de error
            const errorMessages = {
                es: 'Lo siento, ha ocurrido un error al conectar con Aelo. Por favor, intenta nuevamente.',
                ca: 'Ho sento, hi ha hagut un error en connectar amb Aelo. Si us plau, intenta de nou.',
                en: 'Sorry, an error occurred while connecting to Aelo. Please try again.',
                ru: 'Извините, произошла ошибка при подключении к Aelo. Пожалуйста, попробуйте снова.',
                ar: 'عذرًا، حدث خطأ أثناء الاتصال بـ Aelo. حاول مرة أخرى.'
            };
            
            // Actualizar mensaje inicial según el idioma
            const initialMessageElement = document.querySelector('.bot-message');
            initialMessageElement.querySelector('div').textContent = welcomeMessages[currentLanguage];
            initialMessageElement.querySelector('.message-info').textContent = `Sistema | ${languageNames[currentLanguage]}`;
            currentMessages.push({sender: 'bot', text: welcomeMessages[currentLanguage]});
            
            // Cambiar idioma
            languageButtons.forEach(button => {
                button.addEventListener('click', function() {
                    const newLanguage = this.getAttribute('data-lang');
                    if (newLanguage === currentLanguage) return;
                    
                    languageButtons.forEach(btn => btn.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Guardar chat actual en historial si tiene mensajes
                    if (currentMessages.length > 1) { // Ignorar solo el mensaje inicial
                        const timestamp = new Date().toLocaleString();
                        chatHistory.push({timestamp, language: languageNames[currentLanguage], messages: currentMessages});
                        localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                    }
                    
                    // Reiniciar chat para el nuevo idioma
                    resetChat(true);
                    
                    currentLanguage = newLanguage;
                    userInput.placeholder = placeholders[currentLanguage];
                    
                    // Añadir mensaje de cambio de idioma
                    addMessage(languageChangeMessages[currentLanguage], 'bot');
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
                conversationHistory.push({role: 'user', content: message});
                
                // Mostrar indicador de typing
                typingIndicator.style.display = 'block';
                typingIndicator.querySelector('.message-info').textContent = thinkingMessages[currentLanguage];
                chatContainer.scrollTop = chatContainer.scrollHeight;
                
                try {
                    // Llamar a la API de IA
                    const response = await callAIApiReal(message, conversationHistory);
                    
                    // Ocultar indicador de typing
                    typingIndicator.style.display = 'none';
                    
                    // Añadir respuesta al historial
                    conversationHistory.push({role: 'assistant', content: response});
                    
                    // Mostrar respuesta
                    addMessage(response, 'bot');
                    apiStatusText.textContent = 'Conectado a Aelo';
                    
                } catch (error) {
                    // Ocultar indicador de typing
                    typingIndicator.style.display = 'none';
                    
                    // Mostrar mensaje de error
                    addMessage(errorMessages[currentLanguage], 'bot');
                    apiStatusText.textContent = 'Error de conexión - Intentando reconectar';
                    console.error('Error calling AI API:', error.message); // Más detalle para depuración
                } finally {
                    sendButton.disabled = false;
                }
            }
            
            async function callAIApiReal(message, history) {
                const API_URL = 'https://api.deepseek.com/v1/chat/completions';
                const API_KEY = 'sk-502452ecbf4a4903b0a52d137e8c8db3';
                
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${API_KEY}`
                    },
                    body: JSON.stringify({
                        model: 'deepseek-chat',
                        messages: [
                            {role: 'system', content: `Eres Aelo, un asistente útil. ${languageInstructions[currentLanguage]}`},
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
                messageInfo.textContent = `${sender === 'user' ? 'Tú' : 'Aelo'} | ${languageNames[currentLanguage]} | ${timeString}`;
                
                messageElement.appendChild(messageInfo);
                
                chatContainer.insertBefore(messageElement, typingIndicator);
                chatContainer.scrollTop = chatContainer.scrollHeight;
                
                // Guardar en currentMessages
                currentMessages.push({sender, text});
            }
            
            function resetChat(isLanguageChange = false) {
                conversationHistory = [];
                currentMessages = [];
                chatContainer.innerHTML = '';
                chatContainer.appendChild(typingIndicator); // Reañadir el indicador de typing
                
                // Añadir mensaje inicial si no es cambio de idioma (para cambio de idioma, se añade después)
                if (!isLanguageChange) {
                    addMessage(welcomeMessages[currentLanguage], 'bot');
                }
            }
            
            // Botón de reiniciar chat
            resetButton.addEventListener('click', function() {
                // Guardar chat actual en historial si tiene mensajes
                if (currentMessages.length > 1) {
                    const timestamp = new Date().toLocaleString();
                    chatHistory.push({timestamp, language: languageNames[currentLanguage], messages: currentMessages});
                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                }
                
                resetChat();
            });
            
            // Botón de historial
            historyButton.addEventListener('click', function() {
                historyList.innerHTML = '';
                if (chatHistory.length === 0) {
                    historyList.innerHTML = '<p>No hay chats en el historial.</p>';
                } else {
                    chatHistory.forEach((chat, index) => {
                        const chatDiv = document.createElement('div');
                        chatDiv.classList.add('history-chat');
                        chatDiv.innerHTML = `<h3>Chat ${index + 1} - ${chat.timestamp} (${chat.language})</h3>`;
                        
                        chat.messages.forEach(msg => {
                            const msgDiv = document.createElement('div');
                            msgDiv.classList.add('history-message');
                            msgDiv.classList.add(msg.sender === 'user' ? 'history-user' : 'history-bot');
                            msgDiv.textContent = `${msg.sender === 'user' ? 'Tú' : 'Aelo'}: ${msg.text}`;
                            chatDiv.appendChild(msgDiv);
                        });
                        
                        historyList.appendChild(chatDiv);
                    });
                }
                historyModal.style.display = 'block';
            });
            
            // Cerrar modal
            closeModal.addEventListener('click', function() {
                historyModal.style.display = 'none';
            });
            
            window.addEventListener('click', function(event) {
                if (event.target === historyModal) {
                    historyModal.style.display = 'none';
                }
            });
        });
    </script>
</body>
</html>
