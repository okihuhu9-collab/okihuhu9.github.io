<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Valorant Randomizer - Weapons & Agents</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #0f0f23, #1a1a2e, #16213e);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: white;
            overflow-x: hidden;
        }

        .container {
            text-align: center;
            position: relative;
            max-width: 900px;
            width: 100%;
            padding: 20px;
        }

        .title {
            font-size: 2.5rem;
            margin-bottom: 2rem;
            background: linear-gradient(45deg, #ff4655, #ff6b7a);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-weight: bold;
            text-shadow: 0 0 30px rgba(255, 70, 85, 0.5);
        }

        .randomizer-section {
            display: flex;
            justify-content: space-around;
            gap: 2rem;
            flex-wrap: wrap;
            margin-bottom: 2rem;
        }

        .randomizer-box {
            flex: 1;
            min-width: 300px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 20px;
            padding: 2rem;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 70, 85, 0.3);
        }

        .section-title {
            font-size: 1.5rem;
            margin-bottom: 1rem;
            color: #ff4655;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .display-area {
            min-height: 100px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            margin-bottom: 1.5rem;
        }

        .item-name {
            font-size: 2rem;
            font-weight: bold;
            color: #fff;
            margin-bottom: 0.5rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.5s ease;
        }

        .item-name.show {
            opacity: 1;
            transform: translateY(0);
        }

        .item-type {
            font-size: 1rem;
            color: #888;
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.5s ease 0.2s;
        }

        .item-type.show {
            opacity: 1;
            transform: translateY(0);
        }

        .mystery-box {
            width: 150px;
            height: 150px;
            background: linear-gradient(145deg, #2a2a3e, #1a1a2e);
            border: 3px solid #ff4655;
            border-radius: 20px;
            position: relative;
            margin: 0 auto 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 
                0 0 30px rgba(255, 70, 85, 0.3),
                inset 0 0 20px rgba(0, 0, 0, 0.5);
            overflow: hidden;
        }

        .mystery-box:hover {
            transform: scale(1.05);
            box-shadow: 
                0 0 50px rgba(255, 70, 85, 0.5),
                inset 0 0 20px rgba(0, 0, 0, 0.5);
        }

        .mystery-box:active {
            transform: scale(0.95);
        }

        .mystery-box::before {
            content: '?';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 4rem;
            font-weight: bold;
            color: rgba(255, 70, 85, 0.8);
            text-shadow: 0 0 15px rgba(255, 70, 85, 0.5);
        }

        .mystery-box::after {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(
                45deg,
                transparent 30%,
                rgba(255, 255, 255, 0.1) 50%,
                transparent 70%
            );
            animation: shimmer 3s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
        }

        .click-text {
            font-size: 0.9rem;
            color: #888;
            opacity: 0.8;
        }

        .random-all-btn {
            background: linear-gradient(45deg, #ff4655, #ff6b7a);
            border: none;
            color: white;
            padding: 1rem 2rem;
            font-size: 1.2rem;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            margin-top: 2rem;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
            box-shadow: 0 5px 20px rgba(255, 70, 85, 0.4);
        }

        .random-all-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 30px rgba(255, 70, 85, 0.6);
        }

        .random-all-btn:active {
            transform: translateY(0);
        }

        .shake {
            animation: shake 0.5s ease;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0) scale(1.05); }
            25% { transform: translateX(-10px) scale(1.05); }
            75% { transform: translateX(10px) scale(1.05); }
        }

        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: #ff4655;
            border-radius: 50%;
            opacity: 0;
            animation: float 6s infinite;
        }

        @keyframes float {
            0% {
                opacity: 0;
                transform: translateY(100vh) scale(0);
            }
            10% {
                opacity: 1;
                transform: translateY(90vh) scale(1);
            }
            90% {
                opacity: 1;
                transform: translateY(10vh) scale(1);
            }
            100% {
                opacity: 0;
                transform: translateY(0) scale(0);
            }
        }

        .agent-box {
            border-color: #4ecdc4 !important;
            box-shadow: 
                0 0 30px rgba(78, 205, 196, 0.3),
                inset 0 0 20px rgba(0, 0, 0, 0.5) !important;
        }

        .agent-box:hover {
            box-shadow: 
                0 0 50px rgba(78, 205, 196, 0.5),
                inset 0 0 20px rgba(0, 0, 0, 0.5) !important;
        }

        .agent-box::before {
            color: rgba(78, 205, 196, 0.8) !important;
            text-shadow: 0 0 15px rgba(78, 205, 196, 0.5) !important;
        }

        @media (max-width: 768px) {
            .title {
                font-size: 2rem;
            }
            
            .randomizer-section {
                flex-direction: column;
                align-items: center;
            }
            
            .randomizer-box {
                min-width: 280px;
            }
        }
    </style>
</head>
<body>
    <div class="particles" id="particles"></div>
    
    <div class="container">
        <h1 class="title">VALORANT RANDOMIZER</h1>
        
        <div class="randomizer-section">
            <!-- Weapons Section -->
            <div class="randomizer-box">
                <h2 class="section-title">Vũ Khí</h2>
                <div class="display-area">
                    <div class="item-name" id="weaponName">CLICK THE BOX</div>
                    <div class="item-type" id="weaponType">Để random vũ khí</div>
                </div>
                <div class="mystery-box" id="weaponBox"></div>
                <div class="click-text">Nhấn để random vũ khí!</div>
            </div>

            <!-- Agents Section -->
            <div class="randomizer-box">
                <h2 class="section-title">Đặc Vụ</h2>
                <div class="display-area">
                    <div class="item-name" id="agentName">CLICK THE BOX</div>
                    <div class="item-type" id="agentRole">Để random đặc vụ</div>
                </div>
                <div class="mystery-box agent-box" id="agentBox"></div>
                <div class="click-text">Nhấn để random đặc vụ!</div>
            </div>
        </div>

        <button class="random-all-btn" id="randomAllBtn">Random Tất Cả</button>
    </div>

    <script>
        // Danh sách tất cả vũ khí Valorant (trừ Giao)
        const weapons = [
            { name: "Classic", type: "Sidearm" },
            { name: "Shorty", type: "Sidearm" },
            { name: "Frenzy", type: "Sidearm" },
            { name: "Ghost", type: "Sidearm" },
            { name: "Sheriff", type: "Sidearm" },
            { name: "Stinger", type: "SMG" },
            { name: "Spectre", type: "SMG" },
            { name: "Bucky", type: "Shotgun" },
            { name: "Judge", type: "Shotgun" },
            { name: "Bulldog", type: "Rifle" },
            { name: "Vandal", type: "Rifle" },
            { name: "Phantom", type: "Rifle" },
            { name: "Guardian", type: "Rifle" },
            { name: "Marshal", type: "Sniper" },
            { name: "Operator", type: "Sniper" },
            { name: "Ares", type: "Heavy" },
            { name: "Odin", type: "Heavy" }
        ];

        // Danh sách tất cả đặc vụ Valorant
        const agents = [
            { name: "Brimstone", role: "Controller" },
            { name: "Viper", role: "Controller" },
            { name: "Omen", role: "Controller" },
            { name: "Astra", role: "Controller" },
            { name: "Harbor", role: "Controller" },
            { name: "Clove", role: "Controller" },
            { name: "Phoenix", role: "Duelist" },
            { name: "Jett", role: "Duelist" },
            { name: "Reyna", role: "Duelist" },
            { name: "Raze", role: "Duelist" },
            { name: "Yoru", role: "Duelist" },
            { name: "Neon", role: "Duelist" },
            { name: "Iso", role: "Duelist" },
            { name: "Sage", role: "Sentinel" },
            { name: "Cypher", role: "Sentinel" },
            { name: "Killjoy", role: "Sentinel" },
            { name: "Chamber", role: "Sentinel" },
            { name: "Deadlock", role: "Sentinel" },
            { name: "Vyse", role: "Sentinel" },
            { name: "Sova", role: "Initiator" },
            { name: "Breach", role: "Initiator" },
            { name: "Skye", role: "Initiator" },
            { name: "Kay/O", role: "Initiator" },
            { name: "Fade", role: "Initiator" },
            { name: "Gekko", role: "Initiator" },
            { name: "KAY/O", role: "Initiator" }
        ];

        // Elements
        const weaponName = document.getElementById('weaponName');
        const weaponType = document.getElementById('weaponType');
        const weaponBox = document.getElementById('weaponBox');
        const agentName = document.getElementById('agentName');
        const agentRole = document.getElementById('agentRole');
        const agentBox = document.getElementById('agentBox');
        const randomAllBtn = document.getElementById('randomAllBtn');

        // Tạo hiệu ứng particles
        function createParticles() {
            const particlesContainer = document.getElementById('particles');
            for (let i = 0; i < 50; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 6 + 's';
                particle.style.animationDuration = (Math.random() * 3 + 3) + 's';
                particlesContainer.appendChild(particle);
            }
        }

        // Random vũ khí
        function randomWeapon() {
            weaponBox.classList.add('shake');
            setTimeout(() => {
                weaponBox.classList.remove('shake');
            }, 500);

            weaponName.classList.remove('show');
            weaponType.classList.remove('show');

            setTimeout(() => {
                const randomIndex = Math.floor(Math.random() * weapons.length);
                const selectedWeapon = weapons[randomIndex];
                weaponName.textContent = selectedWeapon.name;
                weaponType.textContent = selectedWeapon.type;
                weaponName.classList.add('show');
                weaponType.classList.add('show');
            }, 300);
        }

        // Random đặc vụ
        function randomAgent() {
            agentBox.classList.add('shake');
            setTimeout(() => {
                agentBox.classList.remove('shake');
            }, 500);

            agentName.classList.remove('show');
            agentRole.classList.remove('show');

            setTimeout(() => {
                const randomIndex = Math.floor(Math.random() * agents.length);
                const selectedAgent = agents[randomIndex];
                agentName.textContent = selectedAgent.name;
                agentRole.textContent = selectedAgent.role;
                agentName.classList.add('show');
                agentRole.classList.add('show');
            }, 300);
        }

        // Random tất cả
        function randomAll() {
            randomWeapon();
            setTimeout(randomAgent, 100);
        }

        // Event listeners
        weaponBox.addEventListener('click', randomWeapon);
        agentBox.addEventListener('click', randomAgent);
        randomAllBtn.addEventListener('click', randomAll);

        // Khởi tạo
        createParticles();
    </script>
</body>
</html>
