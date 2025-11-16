<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Космическая ракета</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: #000;
            overflow: hidden;
            font-family: 'Arial', sans-serif;
            color: white;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            position: relative;
        }
        
        .container {
            width: 100%;
            height: 100%;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-end;
            padding-bottom: 20px;
        }
        
        .rocket-container {
            position: absolute;
            bottom: 150px;
            transition: transform 0.1s linear;
            z-index: 10;
        }
        
        .rocket {
            width: 80px;
            height: 180px;
            position: relative;
            animation: float 2s infinite ease-in-out;
        }
        
        .rocket-body {
            width: 60px;
            height: 150px;
            background: linear-gradient(to right, #ccc, #fff, #ccc);
            margin: 0 auto;
            border-radius: 50% 50% 40% 40%;
            position: relative;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }
        
        .rocket-top {
            width: 0;
            height: 0;
            border-left: 30px solid transparent;
            border-right: 30px solid transparent;
            border-bottom: 40px solid #ff6b6b;
            position: absolute;
            top: -40px;
            left: 0;
        }
        
        .window {
            width: 25px;
            height: 25px;
            background: #87CEEB;
            border-radius: 50%;
            position: absolute;
            top: 30px;
            left: 50%;
            transform: translateX(-50%);
            border: 2px solid #333;
        }
        
        .fins {
            position: absolute;
            bottom: -10px;
            width: 100%;
            display: flex;
            justify-content: space-between;
        }
        
        .fin {
            width: 20px;
            height: 40px;
            background: #ff6b6b;
            border-radius: 5px;
        }
        
        .flame-container {
            position: absolute;
            bottom: -80px;
            left: 50%;
            transform: translateX(-50%);
            width: 40px;
            height: 80px;
        }
        
        .flame {
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 30px;
            height: 70px;
            background: linear-gradient(to top, #ff4500, #ff8c00, #ffd700);
            border-radius: 50% 50% 20% 20%;
            filter: blur(5px);
            animation: flame 0.5s infinite alternate;
        }
        
        .stars {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }
        
        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite ease-in-out;
        }
        
        .controls {
            display: flex;
            gap: 15px;
            margin-top: 20px;
            z-index: 20;
        }
        
        button {
            padding: 10px 20px;
            background: #4a4a9c;
            color: white;
            border: none;
            border-radius: 20px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 0 10px rgba(74, 74, 156, 0.7);
        }
        
        button:hover {
            background: #5a5aac;
            transform: scale(1.05);
        }
        
        .info-panel {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(0, 0, 0, 0.7);
            padding: 15px;
            border-radius: 10px;
            z-index: 20;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .altitude {
            font-size: 18px;
            margin-bottom: 10px;
        }
        
        .speed {
            font-size: 18px;
        }
        
        .planet {
            position: absolute;
            bottom: -300px;
            width: 600px;
            height: 600px;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, #1a5276, #0e2a44);
            z-index: 5;
            box-shadow: 0 0 50px rgba(26, 82, 118, 0.8);
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        
        @keyframes flame {
            0% { height: 60px; opacity: 0.8; }
            100% { height: 80px; opacity: 1; }
        }
        
        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }
        
        @keyframes flyUp {
            0% { bottom: 150px; }
            100% { bottom: 100vh; }
        }
        
        .message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 24px;
            text-align: center;
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 10px;
            z-index: 30;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="stars" id="stars"></div>
        <div class="planet"></div>
        
        <div class="rocket-container" id="rocketContainer">
            <div class="rocket">
                <div class="rocket-top"></div>
                <div class="rocket-body">
                    <div class="window"></div>
                </div>
                <div class="fins">
                    <div class="fin"></div>
                    <div class="fin"></div>
                </div>
                <div class="flame-container">
                    <div class="flame"></div>
                </div>
            </div>
        </div>
        
        <div class="info-panel">
            <div class="altitude">Высота: <span id="altitude">0</span> км</div>
            <div class="speed">Скорость: <span id="speed">0</span> км/с</div>
        </div>
        
        <div class="controls">
            <button id="startBtn">Запуск</button>
            <button id="resetBtn">Сброс</button>
        </div>
        
        <div class="message" id="message">
            Ракета достигла космоса! 🚀
        </div>
    </div>

    <script>
        // Инициализация Telegram Web App
        let tg = window.Telegram.WebApp;
        tg.expand();
        tg.enableClosingConfirmation();
        
        // Создание звездного неба
        const starsContainer = document.getElementById('stars');
        for (let i = 0; i < 200; i++) {
            const star = document.createElement('div');
            star.classList.add('star');
            star.style.left = `${Math.random() * 100}%`;
            star.style.top = `${Math.random() * 100}%`;
            star.style.width = `${Math.random() * 3}px`;
            star.style.height = star.style.width;
            star.style.animationDelay = `${Math.random() * 5}s`;
            starsContainer.appendChild(star);
        }
        
        const rocketContainer = document.getElementById('rocketContainer');
        const startBtn = document.getElementById('startBtn');
        const resetBtn = document.getElementById('resetBtn');
        const altitudeDisplay = document.getElementById('altitude');
        const speedDisplay = document.getElementById('speed');
        const message = document.getElementById('message');
        
        let isFlying = false;
        let altitude = 0;
        let speed = 0;
        let animationId;
        
        // Функция запуска ракеты
        function launchRocket() {
            if (isFlying) return;
            
            isFlying = true;
            startBtn.disabled = true;
            
            // Увеличиваем скорость постепенно
            const accelerationInterval = setInterval(() => {
                if (speed < 10) {
                    speed += 0.1;
                    speedDisplay.textContent = speed.toFixed(1);
                } else {
                    clearInterval(accelerationInterval);
                }
            }, 100);
            
            // Анимация полета
            function fly() {
                altitude += speed / 10;
                altitudeDisplay.textContent = altitude.toFixed(1);
                
                // Перемещаем ракету вверх
                const currentBottom = parseFloat(getComputedStyle(rocketContainer).bottom);
                rocketContainer.style.bottom = `${currentBottom + speed}px`;
                
                // Уменьшаем планету по мере удаления
                const planet = document.querySelector('.planet');
                const currentScale = parseFloat(getComputedStyle(planet).width);
                if (currentScale > 100) {
                    planet.style.width = `${currentScale - speed / 2}px`;
                    planet.style.height = planet.style.width;
                }
                
                // Проверяем достижение космоса
                if (altitude > 100) {
                    clearInterval(accelerationInterval);
                    cancelAnimationFrame(animationId);
                    message.style.display = 'block';
                    
                    // Отправляем событие в Telegram
                    if (tg && tg.sendData) {
                        tg.sendData(JSON.stringify({
                            event: 'rocket_launched',
                            altitude: altitude.toFixed(1)
                        }));
                    }
                } else {
                    animationId = requestAnimationFrame(fly);
                }
            }
            
            animationId = requestAnimationFrame(fly);
        }
        
        // Функция сброса
        function resetRocket() {
            isFlying = false;
            startBtn.disabled = false;
            altitude = 0;
            speed = 0;
            altitudeDisplay.textContent = '0';
            speedDisplay.textContent = '0';
            rocketContainer.style.bottom = '150px';
            message.style.display = 'none';
            
            const planet = document.querySelector('.planet');
            planet.style.width = '600px';
            planet.style.height = '600px';
            
            cancelAnimationFrame(animationId);
        }
        
        // Обработчики событий
        startBtn.addEventListener('click', launchRocket);
        resetBtn.addEventListener('click', resetRocket);
        
        // Обработка данных от Telegram
        if (tg.initDataUnsafe && tg.initDataUnsafe.start_param) {
            const startParam = tg.initDataUnsafe.start_param;
            if (startParam === 'launch') {
                setTimeout(launchRocket, 1000);
            }
        }
        
        // Изменение темы Telegram
        tg.onEvent('themeChanged', () => {
            document.body.style.background = tg.themeParams.bg_color || '#000';
        });
        
        // Изменение размера окна
        tg.onEvent('viewportChanged', () => {
            // Можно добавить адаптацию под изменение размера
        });
    </script>
</body>
</html>
