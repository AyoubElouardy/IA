<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aelo - Asistente IA</title>
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
                ¡Hola! Soy Aelo, tu asistente de IA. ¿En qué puedo ayudarte hoy?
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
                'es': '¡Hola! Soy Aelo, tu asistente de IA. ¿En qué puedo ayudarte hoy?',
                'en': 'Hello! I am Aelo, your AI assistant. How can I help you today?',
                'fr': 'Bonjour! Je suis Aelo, votre assistant IA. Comment puis-je vous aider aujourd\'hui?',
                'ru': 'Привет! Я Aelo, ваш ИИ-ассистент. Как я могу вам помочь сегодня?',
                'ca': 'Hola! Sóc Aelo, el teu assistent d\'IA. Com et puc ajudar avui?'
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
            
            // Función para buscar información en Google (simulado)
            function searchInformation(query, language) {
                // Simulamos una búsqueda con respuestas predefinidas según el tema
                const queryLower = query.toLowerCase();
                
                // Respuestas en diferentes idiomas
                const responses = {
                    'es': {
                        'clima': 'El clima actual es agradable, con temperaturas suaves y cielos parcialmente nublados. Es un buen día para actividades al aire libre.',
                        'noticias': 'Las noticias más importantes de hoy incluyen avances tecnológicos, eventos culturales y desarrollos económicos positivos en varios países.',
                        'deporte': 'Los eventos deportivos más relevantes de hoy incluyen partidos de fútbol, baloncesto y tenis. ¿Te interesa algún deporte en particular?',
                        'tecnología': 'Los últimos avances en tecnología incluyen inteligencia artificial, computación cuántica y desarrollos en energías renovables. Es un campo en constante evolución.',
                        'default': 'He procesado tu consulta y tengo información relevante al respecto. Basándome en los datos más recientes, puedo decirte que es un tema interesante que está generando mucho debate en la actualidad.'
                    },
                    'en': {
                        'weather': 'The current weather is pleasant, with mild temperatures and partly cloudy skies. It\'s a good day for outdoor activities.',
                        'news': 'The most important news today includes technological advances, cultural events, and positive economic developments in several countries.',
                        'sports': 'The most relevant sporting events today include soccer, basketball and tennis matches. Are you interested in any particular sport?',
                        'technology': 'The latest technological advances include artificial intelligence, quantum computing and developments in renewable energy. It\'s a constantly evolving field.',
                        'default': 'I have processed your query and have relevant information about it. Based on the most recent data, I can tell you that it is an interesting topic that is generating much debate today.'
                    },
                    'fr': {
                        'temps': 'Le temps actuel est agréable, avec des températures douces et un ciel partiellement nuageux. C\'est une bonne journée pour les activités de plein air.',
                        'nouvelles': 'Les nouvelles les plus importantes d\'aujourd\'hui incluent les avancées technologiques, les événements culturels et les développements économiques positifs dans plusieurs pays.',
                        'sport': 'Les événements sportifs les plus pertinents d\'aujourd\'hui incluent des matchs de football, de basket-ball et de tennis. Êtes-vous intéressé par un sport en particulier?',
                        'technologie': 'Les dernières avancées technologiques incluent l\'intelligence artificielle, l\'informatique quantique et les développements dans les énergies renouvelables. C\'est un domaine en constante évolution.',
                        'default': 'J\'ai traité votre requête et j\'ai des informations pertinentes à ce sujet. Sur la base des données les plus récentes, je peux vous dire que c\'est un sujet intéressant qui génère beaucoup de débats aujourd\'hui.'
                    },
                    'ru': {
                        'погода': 'Сейчас погода приятная, с мягкими температурами и переменной облачностью. Это хороший день для активного отдыха на открытом воздухе.',
                        'новости': 'Самые важные новости сегодня включают технологические достижения, культурные события и позитивные экономические developments в нескольких странах.',
                        'спорт': 'Самые relevantные спортивные события сегодня включают футбольные, баскетбольные и теннисные матчи. Вас интересует какой-либо конкретный вид спорта?',
                        'технологии': 'Последние технологические достижения включают искусственный интеллект, квантовые вычисления и developments в области возобновляемых источников энергии. Это постоянно развивающаяся область.',
                        'default': 'Я обработал ваш запрос и у меня есть relevantная информация по этому поводу. На основе самых последних данных я могу сказать вам, что это интересная тема, которая сегодня вызывает много споров.'
                    },
                    'ca': {
                        'temps': 'El temps actual és agradable, amb temperatures suaus i cel parcialment ennuvolat. És un bon dia per a activitats a l\'aire lliure.',
                        'notícies': 'Les notícies més importants d\'avui inclouen avenços tecnològics, esdeveniments culturals i desenvolupaments econòmics positius en diversos països.',
                        'esport': 'Els esdeveniments esportius més rellevants d\'avui inclouen partits de futbol, bàsquet i tennis. T\'interessa algun esport en particular?',
                        'tecnologia': 'Els darrers avenços tecnològics inclouen intel·ligència artificial, computació quàntica i desenvolupaments en energies renovables. És un camp en constant evolució.',
                        'default': 'He processat la vostra consulta i tinc informació rellevant al respecte. Basant-me en les dades més recents, puc dir-vos que és un tema interessant que està generant molt debat en l\'actualitat.'
                    }
                };
                
                // Buscar coincidencias en la consulta
                const langResponses = responses[language] || responses['es'];
                
                if (queryLower.includes('clima') || queryLower.includes('weather') || queryLower.includes('temps') || queryLower.includes('погода')) {
                    return langResponses['clima'] || langResponses['default'];
                } else if (queryLower.includes('noticia') || queryLower.includes('news') || queryLower.includes('nouvelle') || queryLower.includes('новость')) {
                    return langResponses['noticias'] || langResponses['default'];
                } else if (queryLower.includes('deporte') || queryLower.includes('sport') || queryLower.includes('спорт') || queryLower.includes('esport')) {
                    return langResponses['deporte'] || langResponses['default'];
                } else if (queryLower.includes('tecnología') || queryLower.includes('technology') || queryLower.includes('technologie') || queryLower.includes('технология') || queryLower.includes('tecnologia')) {
                    return langResponses['tecnología'] || langResponses['default'];
                } else {
                    return langResponses['default'];
                }
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
                    
                    // Obtener respuesta
                    const response = searchInformation(message, currentLanguage);
                    
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
