<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Dota 2 Spy</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            min-height: 100vh;
            background: radial-gradient(circle at 20% 30%, #0a0f1e, #020617);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
            padding: 20px;
            color: #f1f5f9;
            font-weight: 400;
            line-height: 1.5;
        }

        .app {
            max-width: 1400px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            margin-bottom: 48px;
        }

        .header h1 {
            font-size: 3.8rem;
            font-weight: 700;
            background: linear-gradient(135deg, #ffb347, #ff6b6b, #ee0979);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: -0.5px;
            text-shadow: 0 2px 20px rgba(238, 9, 121, 0.2);
            margin-bottom: 12px;
        }

        .mode-selector {
            display: flex;
            gap: 20px;
            justify-content: center;
            margin-bottom: 48px;
            flex-wrap: wrap;
        }

        .mode-btn {
            background: rgba(30, 41, 59, 0.5);
            backdrop-filter: blur(12px);
            padding: 12px 32px;
            border-radius: 50px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.25s ease;
            color: #cbd5e6;
            letter-spacing: 0.3px;
        }

        .mode-btn.active {
            background: linear-gradient(105deg, #ff6a00, #ee0979);
            color: white;
            box-shadow: 0 6px 20px rgba(238, 9, 121, 0.35);
            transform: scale(1.02);
        }

        .mode-panel {
            display: none;
        }

        .mode-panel.active-panel {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }

        button {
            background: linear-gradient(105deg, #3b82f6, #a855f7);
            border: none;
            padding: 12px 28px;
            border-radius: 50px;
            font-weight: 600;
            font-size: 0.95rem;
            color: white;
            cursor: pointer;
            transition: 0.2s;
            letter-spacing: 0.3px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        }

        button:active {
            transform: scale(0.96);
        }

        .reset-btn {
            background: linear-gradient(105deg, #475569, #1e293b);
        }

        .spy-game-panel {
            background: rgba(15, 23, 42, 0.55);
            backdrop-filter: blur(8px);
            border-radius: 40px;
            padding: 28px;
            margin-bottom: 32px;
        }

        .players-control {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 20px;
            margin-bottom: 32px;
        }

        .players-control label {
            font-weight: 500;
            font-size: 1rem;
            color: #e2e8f0;
        }

        #playerCount {
            background: #0f172a;
            padding: 10px 16px;
            border-radius: 30px;
            color: white;
            font-size: 1rem;
            width: 80px;
            text-align: center;
            border: 1px solid #334155;
            font-weight: 500;
        }

        .role-screen {
            margin-top: 24px;
            padding: 28px;
            background: #0f172a;
            border-radius: 36px;
            text-align: center;
        }

        .current-player-badge {
            font-size: 1.6rem;
            font-weight: 700;
            margin-bottom: 20px;
            background: linear-gradient(120deg, #facc15, #ffb347);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .spy-message {
            font-size: 2.2rem;
            font-weight: 800;
            background: linear-gradient(120deg, #f97316, #ef4444);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            padding: 20px;
        }

        .hero-card {
            background: linear-gradient(145deg, #1e293b, #0f172a);
            border-radius: 36px;
            padding: 28px;
            margin-top: 20px;
            max-width: 380px;
            margin-left: auto;
            margin-right: auto;
            box-shadow: 0 12px 24px -12px rgba(0,0,0,0.5);
        }

        .hero-card img {
            width: 160px;
            height: 160px;
            object-fit: contain;
            margin-bottom: 16px;
        }

        .hero-name {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(135deg, #facc15, #ffb347);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            white-space: nowrap;
        }

        .guess-game-panel {
            background: rgba(15, 23, 42, 0.55);
            backdrop-filter: blur(8px);
            border-radius: 40px;
            padding: 28px;
            margin-bottom: 32px;
        }

        .hero-selector-area {
            background: #0f172a;
            border-radius: 32px;
            padding: 24px;
        }

        .lore-card {
            background: linear-gradient(145deg, #1e1b2e, #0f0f1a);
            border-radius: 36px;
            padding: 32px;
            margin: 28px 0;
            text-align: center;
            box-shadow: 0 12px 28px -10px rgba(0,0,0,0.4);
        }

        .lore-text {
            font-size: 1.28rem;
            line-height: 1.65;
            font-weight: 450;
            font-style: normal;
            color: #fde047;
            margin: 24px 0;
            padding: 24px;
            background: rgba(0,0,0,0.35);
            border-radius: 28px;
            max-height: 420px;
            overflow-y: auto;
            white-space: pre-wrap;
            word-wrap: break-word;
            text-align: left;
            font-family: 'Georgia', 'Times New Roman', serif;
            letter-spacing: 0.2px;
        }

        .timer {
            font-size: 3.5rem;
            font-weight: 800;
            font-family: monospace;
            text-align: center;
            margin: 24px 0;
            color: #facc15;
            text-shadow: 0 0 12px rgba(250, 204, 21, 0.4);
            letter-spacing: 4px;
        }

        .hint-buttons {
            display: flex;
            gap: 18px;
            flex-wrap: wrap;
            justify-content: center;
            margin: 28px 0;
        }

        .hint-btn {
            background: #334155;
            padding: 12px 28px;
            font-size: 1rem;
            font-weight: 600;
            border-radius: 44px;
            letter-spacing: 0.3px;
        }

        .hint-btn.used {
            opacity: 0.45;
            filter: grayscale(0.2);
        }

        .guess-result {
            margin-top: 24px;
            padding: 20px;
            border-radius: 32px;
            text-align: center;
            font-size: 1.1rem;
            font-weight: 500;
        }

        .victory {
            background: linear-gradient(120deg, #14532d, #166534);
            color: #bbf7d0;
        }

        .defeat {
            background: linear-gradient(120deg, #7f1d1d, #991b1b);
            color: #fecaca;
        }

        .heroes-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 24px;
            margin-top: 40px;
        }

        .hero-item {
            background: rgba(30, 41, 59, 0.65);
            backdrop-filter: blur(4px);
            padding: 18px;
            border-radius: 28px;
            text-align: center;
            transition: all 0.2s ease;
            cursor: pointer;
        }

        .hero-item.selected {
            background: linear-gradient(145deg, #334155, #1e293b);
            transform: scale(1.02);
            box-shadow: 0 8px 20px rgba(249, 115, 22, 0.25);
        }

        .hero-item:hover {
            transform: translateY(-6px);
            background: #3b4a5f;
        }

        .hero-item img {
            width: 120px;
            height: 120px;
            object-fit: contain;
            margin-bottom: 12px;
        }

        .hero-item span {
            display: block;
            font-weight: 600;
            font-size: 1rem;
            letter-spacing: 0.2px;
            white-space: normal;
            word-break: keep-all;
        }

        .section-title {
            font-size: 1.8rem;
            font-weight: 600;
            margin: 48px 0 24px 0;
            padding-left: 20px;
            background: linear-gradient(120deg, #facc15, #f97316);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: -0.3px;
        }

        .hint-content {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(4px);
            padding: 14px 18px;
            border-radius: 28px;
            margin: 10px 0;
            font-size: 1rem;
            text-align: left;
            font-weight: 450;
            border-left: 3px solid #f97316;
        }

        .hidden {
            display: none;
        }

        @media (max-width: 700px) {
            .header h1 { font-size: 2.2rem; }
            .heroes-grid { grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); }
            .hero-item img { width: 100px; height: 100px; }
            .timer { font-size: 2.2rem; }
            .lore-text { font-size: 1rem; }
            .hero-name { font-size: 1.3rem; white-space: normal; }
        }
    </style>
</head>
<body>
<div class="app">
    <div class="header">
        <h1>DOTA 2: ШПИОН</h1>
    </div>

    <div class="mode-selector" id="modeSelector">
        <div class="mode-btn active" data-mode="spy">Режим Шпион</div>
        <div class="mode-btn" data-mode="guess">Режим Угадай героя</div>
    </div>

    <div id="spyModePanel" class="mode-panel active-panel">
        <div class="spy-game-panel">
            <div class="players-control">
                <label>Количество игроков</label>
                <input type="number" id="playerCount" min="2" max="12" value="5">
                <button id="startSpyGameBtn">Начать игру</button>
                <button id="resetSpyGameBtn" class="reset-btn">Сбросить</button>
            </div>
            <div id="spyRoleScreen" class="role-screen">
                Настройте количество игроков и нажмите "Начать игру"
            </div>
        </div>
    </div>

    <div id="guessModePanel" class="mode-panel">
        <div class="guess-game-panel">
            <div id="heroSelectorBlock" class="hero-selector-area">
                <div style="text-align: center; margin-bottom: 18px; font-weight: 500; font-size: 1.1rem;">Ведущий, выбери героя</div>
                <div id="guessHeroSelector" class="heroes-grid" style="max-height: 400px; overflow-y: auto; margin-top: 0;"></div>
                <div style="display: flex; gap: 15px; justify-content: center; flex-wrap: wrap; margin-top: 24px;">
                    <button id="startGuessGameBtn" disabled style="background: #22c55e;">Начать игру</button>
                </div>
            </div>

            <div id="guessGameArea" style="display: none;">
                <div class="lore-card">
                    <div style="font-size: 1.1rem; font-weight: 500; opacity: 0.8;">Отрывок из лора</div>
                    <div id="loreTextDisplay" class="lore-text"></div>
                </div>
                <div class="timer" id="timer">02:00</div>
                <div class="hint-buttons">
                    <button id="hint1Btn" class="hint-btn">Подсказка 1</button>
                    <button id="hint2Btn" class="hint-btn">Подсказка 2</button>
                    <button id="hint3Btn" class="hint-btn">Подсказка 3</button>
                </div>
                <div style="text-align: center; margin: 24px 0;">
                    <button id="winGuessBtn" style="background: #22c55e;">Угадали</button>
                    <button id="loseGuessBtn" style="background: #ef4444; margin-left: 12px;">Не угадали</button>
                </div>
                <div id="guessResult" class="guess-result"></div>
            </div>
        </div>
    </div>

    <div class="section-title">Библиотека героев Dota 2</div>
    <div id="heroesLibrary" class="heroes-grid"></div>
</div>

<script>
    const heroesData = [
        { 
            id: 1, 
            name: "Anti-Mage", 
            imageUrl: "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/heroes/antimage.png",
            lore: `Он ненавидит магию всей душой. Его клинки выкованы из чистого антимагического металла. Он может одним прыжком преодолеть огромные расстояния и сжечь всю ману врага одним ударом. Говорят, он уничтожил целую академию магов за одну ночь. Его тело покрыто защитными рунами, отражающими заклинания.`,
            hint1: "Фиолетовый цвет",
            hint2: "Ловкость",
            hint3: "Керри позиция"
        },
        { 
            id: 2, 
            name: "Pudge", 
            imageUrl: "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/heroes/pudge.png",
            lore: `Он бродит по полям сражений в поисках свежей плоти. Его главное оружие - длинный цепной крюк, которым он притягивает жертв к себе. Чем больше он ест, тем огромнее становится его тело. В его животе разлагаются останки врагов. Он может вонзить в себя крюк и разорвать собственные кишки, чтобы замедлить врагов.`,
            hint1: "Зелёный цвет",
            hint2: "Сила",
            hint3: "Мидер / Роумер"
        },
        { 
            id: 3, 
            name: "Invoker", 
            imageUrl: "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/heroes/invoker.png",
            lore: `Он помнит создание Солнца. Он может вызвать метеор, ледяной шторм или огненную стену одним движением пальцев. Его настоящий возраст - тысячи лет. Он комбинирует сферы огня, льда и молнии, чтобы создавать новые заклинания на лету. Нет магии, которую он не знает.`,
            hint1: "Бело-синий / золотой",
            hint2: "Интеллект",
            hint3: "Мидер позиция"
        },
        { 
            id: 4, 
            name: "Crystal Maiden", 
            imageUrl: "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/heroes/crystal_maiden.png",
            lore: `Она заключила в себе силу вечного холода. Её аура восстанавливает ману союзникам. Она может вызвать корни льда из-под земли, чтобы обездвижить врагов. Её ультимативная способность создаёт вокруг неё настоящую снежную бурю, уничтожающую всех, кто подойдёт близко.`,
            hint1: "Голубой цвет",
            hint2: "Интеллект",
            hint3: "Саппорт позиция"
        },
        { 
            id: 5, 
            name: "Juggernaut", 
            imageUrl: "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/heroes/juggernaut.png",
            lore: `Он носит маску воина и никогда её не снимает. Его меч вращается с бешеной скоростью, создавая вихрь, который рассекает врагов на части. Он может стать полностью неуязвимым на время своего танца смерти. Он лечит себя и союзников с помощью тотема исцеления.`,
            hint1: "Красный / Белый",
            hint2: "Ловкость",
            hint3: "Керри позиция"
        },
        { 
            id: 6, 
            name: "Rubick", 
            imageUrl: "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/heroes/rubick.png",
            lore: `Он не создаёт магию - он её крадёт. Его плащ завораживает врагов своим переливающимся светом. Он может поднять в воздух телекинезом любого врага и швырнуть его куда захочет. Говорят, он украл заклинания у самого архимага.`,
            hint1: "Зелёный / фиолетовый",
            hint2: "Интеллект",
            hint3: "Саппорт / Роумер"
        }
    ];

    function censorHeroName(loreText, heroName) {
        if (!loreText || !heroName) return loreText;
        const regex = new RegExp(heroName, 'gi');
        return loreText.replace(regex, '[имя персонажа]');
    }

    function renderHeroesLibrary() {
        const container = document.getElementById("heroesLibrary");
        container.innerHTML = "";
        heroesData.forEach(hero => {
            const card = document.createElement("div");
            card.className = "hero-item";
            card.innerHTML = `
                <img src="${hero.imageUrl}" alt="${hero.name}" loading="lazy">
                <span>${hero.name}</span>
            `;
            container.appendChild(card);
        });
    }

    // РЕЖИМ ШПИОНА
    let spyGameInitialized = false;
    let spyPlayerNumber = null;
    let secretHero = null;
    let totalPlayers = 5;
    let currentPlayerIndex = 1;
    let roleRevealedForCurrent = false;

    const spyRoleScreen = document.getElementById("spyRoleScreen");
    const playerCountInput = document.getElementById("playerCount");
    const startSpyBtn = document.getElementById("startSpyGameBtn");
    const resetSpyBtn = document.getElementById("resetSpyGameBtn");

    function getRandomHeroForSpy() {
        if (heroesData.length === 0) return null;
        const randomIndex = Math.floor(Math.random() * heroesData.length);
        return { ...heroesData[randomIndex] };
    }

    function initSpyGame() {
        totalPlayers = parseInt(playerCountInput.value);
        if (isNaN(totalPlayers) || totalPlayers < 2) { alert("Минимум 2 игрока"); return false; }
        if (heroesData.length === 0) { alert("Нет героев"); return false; }
        spyPlayerNumber = Math.floor(Math.random() * totalPlayers) + 1;
        secretHero = getRandomHeroForSpy();
        spyGameInitialized = true;
        currentPlayerIndex = 1;
        roleRevealedForCurrent = false;
        return true;
    }

    function showSpyPlayerScreen() {
        if (!spyGameInitialized) { spyRoleScreen.innerHTML = `<div>Нажмите "Начать игру"</div>`; return; }
        if (currentPlayerIndex > totalPlayers) {
            spyRoleScreen.innerHTML = `<div style="padding:40px;"><div style="font-size:1.6rem;">Игра готова</div><div>Все узнали роли</div><button onclick="resetSpyGame()" class="reset-btn" style="margin-top:20px;">Сыграть ещё</button></div>`;
            return;
        }
        const isSpy = (currentPlayerIndex === spyPlayerNumber);
        const playerLabel = `Игрок ${currentPlayerIndex} из ${totalPlayers}`;
        if (!roleRevealedForCurrent) {
            spyRoleScreen.innerHTML = `<div class="current-player-badge">${playerLabel}</div><div style="margin:20px 0;">Нажми, чтобы узнать роль</div><button id="revealRoleBtn" style="background:#f97316;">Узнать роль</button>`;
            document.getElementById("revealRoleBtn")?.addEventListener("click", () => revealSpyRole(), { once: true });
        } else {
            spyRoleScreen.innerHTML = `<div class="current-player-badge">${playerLabel}</div><div id="roleDisplay"></div><button id="nextPlayerBtn" style="background:#f97316;">Далее</button>`;
            const roleDisplay = document.getElementById("roleDisplay");
            if (roleDisplay) {
                if (isSpy) roleDisplay.innerHTML = `<div class="spy-message">Вы — Шпион</div><div>Вы не знаете героя</div>`;
                else roleDisplay.innerHTML = `<div style="font-size:1.2rem;">Вы — не шпион</div><div class="hero-card"><img src="${secretHero.imageUrl}"><div class="hero-name">${secretHero.name}</div></div>`;
            }
            document.getElementById("nextPlayerBtn")?.addEventListener("click", () => { currentPlayerIndex++; roleRevealedForCurrent = false; showSpyPlayerScreen(); });
        }
    }
    function revealSpyRole() { if (spyGameInitialized && !roleRevealedForCurrent) { roleRevealedForCurrent = true; showSpyPlayerScreen(); } }
    function startSpyGame() { if (initSpyGame()) showSpyPlayerScreen(); }
    function resetSpyGame() { spyGameInitialized = false; spyPlayerNumber = null; secretHero = null; currentPlayerIndex = 1; roleRevealedForCurrent = false; spyRoleScreen.innerHTML = `Настройте количество игроков и нажмите "Начать игру"`; }
    startSpyBtn.onclick = startSpyGame;
    resetSpyBtn.onclick = resetSpyGame;

    // РЕЖИМ УГАДАЙ ГЕРОЯ
    let selectedHeroForGuess = null, guessGameActive = false, timerInterval = null, timeSeconds = 120, hintsUsed = { hint1: false, hint2: false, hint3: false };
    const heroSelectorBlock = document.getElementById("heroSelectorBlock"), guessHeroSelectorDiv = document.getElementById("guessHeroSelector"), startGuessGameBtn = document.getElementById("startGuessGameBtn"), guessGameArea = document.getElementById("guessGameArea"), timerDiv = document.getElementById("timer"), loreTextDisplay = document.getElementById("loreTextDisplay"), hint1Btn = document.getElementById("hint1Btn"), hint2Btn = document.getElementById("hint2Btn"), hint3Btn = document.getElementById("hint3Btn"), winGuessBtn = document.getElementById("winGuessBtn"), loseGuessBtn = document.getElementById("loseGuessBtn"), guessResultDiv = document.getElementById("guessResult");

    function getRandomLoreSegment(lore, heroName) {
        if (!lore) return "Лор не добавлен";
        let censored = censorHeroName(lore, heroName);
        let sentences = censored.split(/[.!?]+/).filter(s => s.trim().length > 15);
        if (!sentences.length) return censored;
        let start = Math.floor(Math.random() * Math.max(1, sentences.length - 1));
        return sentences.slice(start, start + 2).join(". ") + ".";
    }
    function renderGuessHeroSelector() {
        guessHeroSelectorDiv.innerHTML = "";
        heroesData.forEach(hero => {
            const card = document.createElement("div");
            card.className = "hero-item";
            if (selectedHeroForGuess?.id === hero.id) card.classList.add("selected");
            card.innerHTML = `<img src="${hero.imageUrl}"><span>${hero.name}</span>`;
            card.onclick = () => { selectedHeroForGuess = hero; renderGuessHeroSelector(); startGuessGameBtn.disabled = false; };
            guessHeroSelectorDiv.appendChild(card);
        });
    }
    function stopTimer() { if (timerInterval) clearInterval(timerInterval); timerInterval = null; }
    function updateTimerDisplay() {
        let m = Math.floor(timeSeconds / 60), s = timeSeconds % 60;
        timerDiv.textContent = `${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
        if (timeSeconds <= 0 && guessGameActive) { 
            guessGameActive = false; 
            guessResultDiv.innerHTML = `<div class="defeat" style="padding:20px;">Время вышло. Загадан: ${selectedHeroForGuess?.name}</div>`;
        }
    }
    function startTimer() { stopTimer(); timeSeconds = 120; updateTimerDisplay(); timerInterval = setInterval(() => { if (guessGameActive && timeSeconds > 0) { timeSeconds--; updateTimerDisplay(); } else if (timeSeconds <= 0) stopTimer(); }, 1000); }
    function resetHints() { hintsUsed = { hint1: false, hint2: false, hint3: false }; [hint1Btn, hint2Btn, hint3Btn].forEach(btn => { btn.classList.remove("used"); btn.disabled = false; }); }
    function startGuessGame() {
        if (!selectedHeroForGuess) { alert("Выбери героя"); return; }
        loreTextDisplay.textContent = getRandomLoreSegment(selectedHeroForGuess.lore, selectedHeroForGuess.name);
        heroSelectorBlock.classList.add("hidden");
        guessGameActive = true;
        resetHints();
        startTimer();
        guessGameArea.style.display = "block";
        guessResultDiv.innerHTML = "";
        startGuessGameBtn.disabled = true;
    }
    function useHint(num) {
        if (!guessGameActive) { alert("Игра не активна"); return; }
        let key = `hint${num}`;
        if (hintsUsed[key]) { alert("Подсказка уже использована"); return; }
        hintsUsed[key] = true;
        let text = selectedHeroForGuess[key] || "—";
        guessResultDiv.innerHTML += `<div class="hint-content">Подсказка ${num}: ${text}</div>`;
        let btn = [null, hint1Btn, hint2Btn, hint3Btn][num];
        if (btn) { btn.classList.add("used"); btn.disabled = true; }
    }
    function winGame() { 
        if (guessGameActive) { 
            guessGameActive = false; 
            stopTimer(); 
            guessResultDiv.innerHTML = `<div class="victory" style="padding:20px;">Победа! Угадан герой: ${selectedHeroForGuess?.name}</div>`; 
        } 
    }
    function loseGame() { 
        if (guessGameActive) { 
            guessGameActive = false; 
            stopTimer(); 
            guessResultDiv.innerHTML = `<div class="defeat" style="padding:20px;">Не угадали. Загадан: ${selectedHeroForGuess?.name}</div>`; 
        } 
    }
    function resetGuessGame() { if (guessGameActive) { guessGameActive = false; stopTimer(); } guessGameArea.style.display = "none"; heroSelectorBlock.classList.remove("hidden"); selectedHeroForGuess = null; renderGuessHeroSelector(); startGuessGameBtn.disabled = true; guessResultDiv.innerHTML = ""; }

    startGuessGameBtn.onclick = startGuessGame;
    hint1Btn.onclick = () => useHint(1);
    hint2Btn.onclick = () => useHint(2);
    hint3Btn.onclick = () => useHint(3);
    winGuessBtn.onclick = winGame;
    loseGuessBtn.onclick = loseGame;

    const spyPanel = document.getElementById("spyModePanel"), guessPanel = document.getElementById("guessModePanel"), modeBtns = document.querySelectorAll(".mode-btn");
    function switchMode(mode) {
        if (mode === "spy") { spyPanel.classList.add("active-panel"); guessPanel.classList.remove("active-panel"); }
        else { spyPanel.classList.remove("active-panel"); guessPanel.classList.add("active-panel"); resetGuessGame(); renderGuessHeroSelector(); }
        modeBtns.forEach(btn => { if (btn.dataset.mode === mode) btn.classList.add("active"); else btn.classList.remove("active"); });
    }
    modeBtns.forEach(btn => btn.addEventListener("click", () => switchMode(btn.dataset.mode)));

    renderHeroesLibrary();
    renderGuessHeroSelector();
    startGuessGameBtn.disabled = true;
    window.resetSpyGame = resetSpyGame;
</script>
</body>
</html>
