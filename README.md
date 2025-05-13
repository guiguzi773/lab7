<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>七彩灯光交替闪烁效果</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: #1a1a1a;
            font-family: 'Arial', sans-serif;
            overflow: hidden;
        }
        
        .title {
            color: white;
            margin-bottom: 30px;
            text-shadow: 0 0 10px rgba(255,255,255,0.5);
            font-size: 28px;
        }
        
        .light-container {
            position: relative;
            width: 400px;
            height: 400px;
            margin-bottom: 30px;
        }
        
        .light {
            position: absolute;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            box-shadow: 0 0 20px 5px currentColor;
            opacity: 0.7;
            transition: opacity 0.3s, box-shadow 0.3s;
        }
        
        .light.active {
            opacity: 1;
            box-shadow: 0 0 30px 10px currentColor;
        }
        
        .controls {
            display: flex;
            gap: 20px;
        }
        
        button {
            padding: 12px 30px;
            font-size: 18px;
            border: none;
            border-radius: 50px;
            background: linear-gradient(45deg, #6a11cb, #2575fc);
            color: white;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        
        button:active {
            transform: translateY(1px);
        }
        
        button:disabled {
            background: #555;
            transform: none;
            box-shadow: none;
            cursor: not-allowed;
        }
    </style>
</head>
<body>
    <h1 class="title">七彩灯光交替闪烁效果</h1>
    <div class="light-container" id="lightContainer"></div>
    <div class="controls">
        <button id="startBtn">启动</button>
        <button id="stopBtn" disabled>停止</button>
    </div>

    <script>
        const colors = ['#FF0000', '#FF7F00', '#FFFF00', '#00FF00', '#0000FF', '#4B0082', '#9400D3'];
        const lightContainer = document.getElementById('lightContainer');
        const startBtn = document.getElementById('startBtn');
        const stopBtn = document.getElementById('stopBtn');
        let animationInterval;
        let isAnimating = false;
        
        // 创建7个灯光
        function createLights() {
            const radius = 150;
            const centerX = 200;
            const centerY = 200;
            
            for (let i = 0; i < 7; i++) {
                const angle = (i * (2 * Math.PI / 7)) - Math.PI/2;
                const x = centerX + radius * Math.cos(angle);
                const y = centerY + radius * Math.sin(angle);
                
                const light = document.createElement('div');
                light.className = 'light';
                light.style.left = `${x}px`;
                light.style.top = `${y}px`;
                light.style.color = colors[i];
                
                lightContainer.appendChild(light);
            }
        }
        
        // 随机激活灯光
        function animateLights() {
            const lights = document.querySelectorAll('.light');
            
            // 确保7个灯7个颜色各不相同
            const shuffledColors = [...colors].sort(() => Math.random() - 0.5);
            
            lights.forEach((light, index) => {
                light.style.color = shuffledColors[index];
                light.classList.remove('active');
            });
            
            // 随机选择7个灯中的几个激活
            const activeCount = Math.floor(Math.random() * 4) + 3; // 3-6个灯激活
            const activeIndices = [];
            
            while (activeIndices.length < activeCount) {
                const randomIndex = Math.floor(Math.random() * 7);
                if (!activeIndices.includes(randomIndex)) {
                    activeIndices.push(randomIndex);
                }
            }
            
            activeIndices.forEach(index => {
                lights[index].classList.add('active');
            });
        }
        
        // 启动动画
        function startAnimation() {
            if (isAnimating) return;
            
            isAnimating = true;
            startBtn.disabled = true;
            stopBtn.disabled = false;
            
            // 初始状态
            animateLights();
            
            // 设置定时器
            animationInterval = setInterval(animateLights, 500);
        }
        
        // 停止动画
        function stopAnimation() {
            if (!isAnimating) return;
            
            isAnimating = false;
            startBtn.disabled = false;
            stopBtn.disabled = true;
            
            clearInterval(animationInterval);
            
            // 重置所有灯光
            const lights = document.querySelectorAll('.light');
            lights.forEach(light => {
                light.classList.remove('active');
            });
        }
        
        // 初始化
        createLights();
        
        // 事件监听
        startBtn.addEventListener('click', startAnimation);
        stopBtn.addEventListener('click', stopAnimation);
    </script>
</body>
</html>
