<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aelo - Asistente IA Inteligente</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            width: 100%;
            max-width: 900px;
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            height: 90vh;
        }
        
        .header {
            background: linear-gradient(90deg, #4b6cb7, #182848);
            color: white;
            padding: 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .logo-icon {
            font-size: 32px;
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
        
        .logo-text {
            font-size: 28px;
            font-weight: bold;
        }
        
        .language-selector {
            display: flex;
            gap: 10px;
            align-items: center;
        }
        
        select {
            padding: 8px 15px;
            border-radius: 20px;
            border: none;
            background-color: white;
            color: #182848;
            font-weight: bold;
            cursor: pointer;
            outline: none;
        }
        
        .chat-container {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            background-color: #f8f9fa;
        }
        
        .message {
            max-width: 70%;
            padding: 15px;
            border-radius: 15px;
            animation: fadeIn 0.5s;
            position: relative;
            line-height: 1.5;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .user-message {
            align-self: flex-end;
            background: linear-gradient(90deg, #4b6cb7, #182848);
            color: white;
            border-bottom-right-radius: 5px;
        }
        
        .bot-message {
            align-self: flex-start;
            background-color: white;
            color: #333;
            border: 1px solid #e0e0e0;
            border-bottom-left-radius: 5px;
        }
        
        .input-area {
            padding: 20px;
            background-color: white;
            display: flex;
            gap: 10px;
            border-top: 1px solid #e0e0e0;
        }
        
        input {
            flex: 1;
            padding: 15px;
            border-radius: 25px;
            border: 1px solid #ddd;
            outline: none;
            font-size: 16px;
        }
        
        button {
            padding: 15px 25px;
            border-radius: 25px;
            border: none;
            background: linear-gradient(90deg, #4b6cb7, #182848);
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s;
        }
        
        button:hover {
            transform: scale(1.05);
        }
        
        .typing-indicator {
            align-self: flex-start;
            background-color: white;
            color: #333;
            border: 1px solid #e0e0e0;
            border-radius: 15px;
            padding: 15px;
            display: none;
        }
        
        .typing-dots {
            display: flex;
            gap: 5px;
        }
        
        .dot {
            width: 8px;
            height: 8px;
            background-color: #777;
            border-radius: 50%;
            animation: typingAnimation 1.5s infinite;
        }
        
        .dot:nth-child(2) {
            animation-delay: 0.2s;
        }
        
        .dot:nth-child(3) {
            animation-delay: 0.4s;
        }
        
        @keyframes typingAnimation {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }
        
        .welcome-message {
            text-align: center;
            padding: 20px;
            color: #4b6cb7;
            font-size: 18px;
            font-weight: bold;
        }
        
        @media (max-width: 768px) {
            .container {
                height: 95vh;
                width: 95%;
            }
            
            .message {
                max-width: 85%;
            }
            
            .header {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="logo">
                <div class="logo-icon">🤖</div>
                <div class="logo-text">Aelo AI</div>
            </div>
            <div class="language-selector">
                <select id="language-select">
                    <option value="es">Español</option>
                    <option value="en">English</option>
                    <option value="fr">Français</option>
                    <option value="ru">Русский</option>
                    <option value="ca">Català</option>
                </select>
            </div>
        </div>
        
        <div class="chat-container" id="chat-container">
            <div class="welcome-message" id="welcome-message">
                ¡Hola! Soy Aelo, tu asistente de IA. Puedes preguntarme lo que quieras.
            </div>
            <div class="typing-indicator" id="typing-indicator">
                <div class="typing-dots">
                    <div class="dot"></div>
                    <div class="dot"></div>
                    <div class="dot"></div>
                </div>
            </div>
        </div>
        
        <div class="input-area">
            <input type="text" id="user-input" placeholder="Escribe tu mensaje aquí...">
            <button id="send-button">Enviar</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const chatContainer = document.getElementById('chat-container');
            const userInput = document.getElementById('user-input');
            const sendButton = document.getElementById('send-button');
            const languageSelect = document.getElementById('language-select');
            const typingIndicator = document.getElementById('typing-indicator');
            const welcomeMessage = document.getElementById('welcome-message');
            
            let currentLanguage = 'es';
            
            // Mensajes de bienvenida en diferentes idiomas
            const welcomeMessages = {
                'es': '¡Hola! Soy Aelo, tu asistente de IA. Puedes preguntarme lo que quieras.',
                'en': 'Hello! I am Aelo, your AI assistant. You can ask me anything.',
                'fr': 'Bonjour! Je suis Aelo, votre assistant IA. Vous pouvez me demander n\'importe quoi.',
                'ru': 'Привет! Я Aelo, ваш ИИ-ассистент. Вы можете спросить меня о чем угодно.',
                'ca': 'Hola! Sóc Aelo, el teu assistent d\'IA. Pots preguntar-me el que vulguis.'
            };
            
            // Actualizar mensaje de bienvenida cuando cambia el idioma
            languageSelect.addEventListener('change', function() {
                currentLanguage = this.value;
                welcomeMessage.textContent = welcomeMessages[currentLanguage];
                
                // Cambiar placeholder según idioma
                const placeholders = {
                    'es': 'Escribe tu mensaje aquí...',
                    'en': 'Type your message here...',
                    'fr': 'Tapez votre message ici...',
                    'ru': 'Введите ваше сообщение здесь...',
                    'ca': 'Escriu el teu missatge aquí...'
                };
                userInput.placeholder = placeholders[currentLanguage] || 'Escribe tu mensaje aquí...';
            });
            
            // Función para agregar mensaje al chat
            function addMessage(text, isUser) {
                const messageDiv = document.createElement('div');
                messageDiv.classList.add('message');
                messageDiv.classList.add(isUser ? 'user-message' : 'bot-message');
                messageDiv.textContent = text;
                
                // Insertar antes del indicador de escritura
                chatContainer.insertBefore(messageDiv, typingIndicator);
                
                // Hacer scroll al último mensaje
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }
            
            // Función para mostrar/ocultar el indicador de escritura
            function showTypingIndicator(show) {
                typingIndicator.style.display = show ? 'block' : 'none';
                if (show) {
                    chatContainer.scrollTop = chatContainer.scrollHeight;
                }
            }
            
            // Base de conocimientos simulada para respuestas inteligentes
            const knowledgeBase = {
                'ciencia': {
                    'es': 'La ciencia es el conjunto de conocimientos obtenidos mediante la observación y el razonamiento, sistemáticamente estructurados y de los que se deducen principios y leyes generales.',
                    'en': 'Science is the pursuit and application of knowledge and understanding of the natural and social world following a systematic methodology based on evidence.',
                    'fr': 'La science est l\'ensemble des connaissances acquises par l\'observation et l\'expérimentation, systématiquement structurées et dont on déduit des principes et des lois générales.',
                    'ru': 'Наука - это совокупность знаний, полученных путем наблюдения и рассуждений, систематически структурированных и из которых выводятся общие принципы и законы.',
                    'ca': 'La ciència és el conjunt de coneixements obtinguts mitjançant l\'observació i el raonament, sistemàticament estructurats i dels quals es dedueixen principis i lleis generals.'
                },
                'tecnologia': {
                    'es': 'La tecnología se refiere a la aplicación de conocimientos científicos para practical purposes, especialmente en la industria y en la vida cotidiana.',
                    'en': 'Technology refers to the application of scientific knowledge for practical purposes, especially in industry and everyday life.',
                    'fr': 'La technologie fait référence à l\'application de connaissances scientifiques à des fins pratiques, en particulier dans l\'industrie et la vie quotidienne.',
                    'ru': 'Технология относится к применению научных знаний в практических целях, особенно в промышленности и повседневной жизни.',
                    'ca': 'La tecnologia es refereix a l\'aplicació de coneixements científics per a finalitats pràctiques, especialment en la indústria i en la vida quotidiana.'
                },
                'historia': {
                    'es': 'La historia es la disciplina que estudia y narra los acontecimientos pasados de la humanidad, analizando sus causas y consecuencias.',
                    'en': 'History is the discipline that studies and narrates past events of humanity, analyzing their causes and consequences.',
                    'fr': 'L\'histoire est la discipline qui étudie et raconte les événements passés de l\'humanité, en analysant leurs causes et conséquences.',
                    'ru': 'История - это дисциплина, которая изучает и рассказывает о прошлых событиях человечества, анализируя их причины и последствия.',
                    'ca': 'La història és la disciplina que estudia i narra els esdeveniments passats de la humanitat, analitzant les seves causes i conseqüències.'
                },
                'arte': {
                    'es': 'El arte es la expresión o aplicación de la habilidad creativa humana y la imaginación, typically en forma visual como la pintura o la escultura.',
                    'en': 'Art is the expression or application of human creative skill and imagination, typically in a visual form such as painting or sculpture.',
                    'fr': 'L\'art est l\'expression ou l\'application de la compétence créative humaine et de l\'imagination, généralement sous une forme visuelle comme la peinture ou la sculpture.',
                    'ru': 'Искусство - это выражение или применение человеческого творческого мастерства и воображения, обычно в визуальной форме, такой как живопись или скульптура.',
                    'ca': 'L\'art és l\'expressió o aplicació de l\'habilitat creativa humana i la imaginació, normalment en forma visual com la pintura o l\'escultura.'
                }
            };
            
            // Función para generar una respuesta inteligente
            function generateSmartResponse(message, language) {
                const messageLower = message.toLowerCase();
                
                // Respuestas para preguntas comunes
                if (messageLower.includes('hola') || messageLower.includes('hello') || 
                    messageLower.includes('bonjour') || messageLower.includes('привет') || 
                    messageLower.includes('hola')) {
                    const greetings = {
                        'es': '¡Hola! ¿En qué puedo ayudarte hoy?',
                        'en': 'Hello! How can I help you today?',
                        'fr': 'Bonjour! Comment puis-je vous aider aujourd\'hui?',
                        'ru': 'Привет! Как я могу вам помочь сегодня?',
                        'ca': 'Hola! Com et puc ajudar avui?'
                    };
                    return greetings[language];
                }
                
                if (messageLower.includes('cómo estás') || messageLower.includes('how are you') || 
                    messageLower.includes('comment ça va') || messageLower.includes('как дела')) {
                    const responses = {
                        'es': '¡Estoy funcionando perfectamente, gracias por preguntar! ¿En qué puedo ayudarte?',
                        'en': 'I\'m running perfectly, thanks for asking! How can I help you?',
                        'fr': 'Je fonctionne parfaitement, merci de demander! Comment puis-je vous aider?',
                        'ru': 'Я прекрасно работаю, спасибо, что спросили! Как я могу вам помочь?',
                        'ca': 'Estic funcionant perfectament, gràcies per preguntar! Com et puc ajudar?'
                    };
                    return responses[language];
                }
                
                if (messageLower.includes('qué es') || messageLower.includes('what is') || 
                    messageLower.includes('qu\'est-ce') || messageLower.includes('что такое')) {
                    // Buscar en la base de conocimientos
                    for (const [topic, descriptions] of Object.entries(knowledgeBase)) {
                        if (messageLower.includes(topic)) {
                            return descriptions[language];
                        }
                    }
                }
                
                if (messageLower.includes('gracias') || messageLower.includes('thank you') || 
                    messageLower.includes('merci') || messageLower.includes('спасибо') || 
                    messageLower.includes('gràcies')) {
                    const thanks = {
                        'es': '¡De nada! Estoy aquí para ayudarte siempre que lo necesites.',
                        'en': 'You\'re welcome! I\'m here to help whenever you need.',
                        'fr': 'De rien! Je suis là pour vous aider chaque fois que vous en avez besoin.',
                        'ru': 'Пожалуйста! Я здесь, чтобы помочь вам, когда это необходимо.',
                        'ca': 'De res! Estic aquí per ajudar-te sempre que ho necessitis.'
                    };
                    return thanks[language];
                }
                
                // Respuesta por defecto - simula una IA avanzada
                const defaultResponses = {
                    'es': 'Interesante pregunta. Basándome en el conocimiento disponible, puedo decirte que es un tema fascinante que ha evolucionado significativamente en los últimos años. ¿Hay algún aspecto específico sobre el que te gustaría profundizar?',
                    'en': 'Interesting question. Based on available knowledge, I can tell you that it\'s a fascinating topic that has evolved significantly in recent years. Is there any specific aspect you would like to delve deeper into?',
                    'fr': 'Question intéressante. Sur la base des connaissances disponibles, je peux vous dire que c\'est un sujet fascinant qui a considérablement évolué ces dernières années. Y a-t-il un aspect spécifique sur lequel vous aimeriez approfondir?',
                    'ru': 'Интересный вопрос. Основываясь на доступных знаниях, я могу сказать, что это увлекательная тема, которая значительно развивалась в последние годы. Есть ли какой-то конкретный аспект, который вы хотели бы изучить глубже?',
                    'ca': 'Pregunta interessant. Basant-me en el coneixement disponible, puc dir-te que és un tema fascinant que ha evolucionat significativament en els últims anys. Hi ha algun aspecte específic sobre el qual t\'agradaria aprofundir?'
                };
                
                return defaultResponses[language];
            }
            
            // Función para procesar el mensaje del usuario
            function processUserMessage() {
                const message = userInput.value.trim();
                if (message === '') return;
                
                // Agregar mensaje del usuario al chat
                addMessage(message, true);
                userInput.value = '';
                
                // Mostrar indicador de escritura
                showTypingIndicator(true);
                
                // Simular tiempo de procesamiento
                setTimeout(() => {
                    showTypingIndicator(false);
                    
                    // Generar respuesta inteligente
                    const response = generateSmartResponse(message, currentLanguage);
                    
                    // Agregar respuesta al chat
                    addMessage(response, false);
                }, 1500);
            }
            
            // Event listeners
            sendButton.addEventListener('click', processUserMessage);
            userInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    processUserMessage();
                }
            });
        });
    </script>
</body>
</html>
