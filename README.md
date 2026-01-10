<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>InvestMaster - Elite Edition</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --bg: #0F172A;
            --card: #1E293B;
            --text: #F8FAFC;
            --accent: #38BDF8;
            --success: #22C55E;
            --danger: #EF4444;
            --xp: #FACC15;
        }

        body { font-family: 'Inter', system-ui, sans-serif; background: var(--bg); margin: 0; display: flex; color: var(--text); }

        /* Menu Lateral */
        #side-menu {
            width: 280px; height: 100vh; background: var(--card);
            position: fixed; left: -280px; transition: 0.4s; z-index: 1000; padding: 25px 15px; border-right: 1px solid #334155;
        }
        #side-menu.active { left: 0; }
        .profile-area { background: #0F172A; padding: 20px; border-radius: 16px; text-align: center; margin-bottom: 30px; }
        .xp-bar { background: #334155; height: 10px; border-radius: 5px; margin-top: 10px; overflow: hidden; }
        #xp-fill { background: var(--xp); height: 100%; width: 0%; transition: 0.5s; box-shadow: 0 0 10px var(--xp); }

        /* Botão Menu */
        #menu-btn { position: fixed; left: 25px; top: 25px; z-index: 500; background: var(--accent); color: var(--bg); padding: 12px 20px; border-radius: 10px; cursor: pointer; font-weight: bold; border: none; }

        /* Container Principal */
        main { flex: 1; display: flex; justify-content: center; padding: 20px; min-height: 100vh; }
        .app-container { background: var(--card); width: 100%; max-width: 600px; padding: 40px; border-radius: 24px; margin-top: 80px; box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5); height: fit-content; }

        /* Elementos do Jogo */
        .info-box { background: rgba(56, 189, 248, 0.1); border-left: 4px solid var(--accent); padding: 20px; border-radius: 12px; margin: 20px 0; }
        .tag { font-size: 10px; font-weight: bold; text-transform: uppercase; background: var(--accent); color: var(--bg); padding: 2px 6px; border-radius: 4px; margin-bottom: 10px; display: inline-block; }
        
        button.opt { width: 100%; padding: 16px; margin: 8px 0; background: #334155; border: 1px solid #475569; color: white; border-radius: 12px; text-align: left; cursor: pointer; transition: 0.2s; font-size: 15px; }
        button.opt:hover { border-color: var(--accent); background: #3E4C5E; }
        button.opt.selected { border-color: var(--accent); background: rgba(56, 189, 248, 0.2); }

        textarea { width: 100%; height: 100px; background: #0F172A; border: 1px solid #475569; color: white; border-radius: 12px; padding: 15px; margin-top: 10px; font-family: inherit; resize: none; }

        .hidden { display: none; }
        #overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 999; backdrop-filter: blur(4px); }
        .btn-confirm { width: 100%; padding: 18px; background: var(--success); color: white; border: none; border-radius: 12px; font-weight: bold; margin-top: 20px; cursor: pointer; }
    </style>
</head>
<body>

<div id="side-menu">
    <div class="profile-area">
        <div style="font-size: 40px;">💎</div>
        <div id="rank-name" style="color: var(--accent); font-weight: bold; margin-top: 10px;">Novato</div>
        <div style="font-size: 12px; margin-top: 5px;">XP: <span id="xp-val">0</span></div>
        <div class="xp-bar"><div id="xp-fill"></div></div>
    </div>
    <div onclick="showPage('home')" style="cursor:pointer; padding:15px;">🏠 Jornada</div>
    <div onclick="resetGame()" style="cursor:pointer; padding:15px; color:var(--danger);">⚠️ Resetar</div>
</div>
<div id="overlay" onclick="toggleMenu()"></div>

<main>
    <button id="menu-btn" onclick="toggleMenu()">☰ MENU</button>

    <div class="app-container">
        <div id="page-home">
            <h2 style="text-align: center;">Trilha do Especialista</h2>
            <div id="level-list"></div>
        </div>

        <div id="page-game" class="hidden">
            <div style="display:flex; justify-content: space-between;">
                <span id="game-title" style="color: var(--accent); font-weight: bold;"></span>
                <span id="game-step"></span>
            </div>
            
            <div class="info-box">
                <span class="tag">Conceito</span>
                <div id="txt-tech" style="font-size: 14px; margin-bottom: 10px;"></div>
                <span class="tag" style="background: var(--success);">Na Prática</span>
                <div id="txt-real" style="font-size: 15px;"></div>
            </div>

            <p id="game-q" style="font-weight: bold;"></p>
            
            <div id="options-area"></div>
            <div id="open-area" class="hidden">
                <textarea id="open-answer" placeholder="Explique com suas palavras..."></textarea>
                <small style="color: #94A3B8;">Dica: Use termos técnicos aprendidos acima.</small>
            </div>

            <button id="btn-next" class="btn-confirm hidden" onclick="handleNext()">Confirmar Resposta</button>
            <div id="study-tip" class="hidden" style="margin-top: 15px; font-size: 13px;">
                <a id="study-link" href="#" target="_blank" style="color: var(--accent);">Estudar mais sobre isso →</a>
            </div>
        </div>

        <div id="page-res" class="hidden" style="text-align: center;">
            <div id="res-body"></div>
        </div>
    </div>
</main>

<script>
    // --- DATABASE ---
    const course = [
        {
            name: "Módulo 1: Renda Fixa",
            questions: [
                {
                    type: "multiple",
                    title: "O que é CDB?",
                    tech: "Certificado de Depósito Bancário. Título de dívida emitido por bancos.",
                    real: "Você empresta dinheiro para o banco e ele te paga juros. É seguro pelo FGC.",
                    q: "Qual a função do investidor ao comprar um CDB?",
                    o: ["Emprestar dinheiro para o banco", "Tornar-se dono de uma agência", "Pagar as dívidas do governo", "Comprar ações da bolsa"],
                    c: "Emprestar dinheiro para o banco",
                    link: "https://www.tesourodireto.com.br/educacao-financeira/blog/o-que-e-cdb.htm"
                },
                {
                    type: "open",
                    title: "Avaliação Final: CDB",
                    tech: "Demonstre que entendeu o fluxo de capital no CDB.",
                    real: "Explique como funciona o CDB usando termos como 'emprestar', 'banco' e 'juros'.",
                    q: "Como você explicaria o CDB para um amigo?",
                    keys: ["emprestar", "banco", "juros", "renda fixa", "fgc"],
                    link: "https://www.b3.com.br"
                }
            ]
        }
    ];

    // --- LOGIC ---
    let xp = parseInt(localStorage.getItem('master_xp')) || 0;
    let curLvl = 0; let curStep = 0; let selectedIdx = null;

    // SONS (Web Audio API)
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    function playSound(type) {
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain); gain.connect(audioCtx.destination);
        if(type === 'win') { osc.frequency.setValueAtTime(523.25, audioCtx.currentTime); osc.frequency.exponentialRampToValueAtTime(1046.50, audioCtx.currentTime + 0.3); }
        else if(type === 'err') { osc.frequency.setValueAtTime(150, audioCtx.currentTime); osc.frequency.linearRampToValueAtTime(50, audioCtx.currentTime + 0.3); }
        else { osc.frequency.setValueAtTime(400, audioCtx.currentTime); }
        osc.start(); gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 0.3); osc.stop(audioCtx.currentTime + 0.3);
    }

    function toggleMenu() {
        document.getElementById('side-menu').classList.toggle('active');
        document.getElementById('overlay').style.display = document.getElementById('side-menu').classList.contains('active') ? 'block' : 'none';
    }

    function updateProfile() {
        document.getElementById('xp-val').innerText = xp;
        document.getElementById('xp-fill').style.width = (xp % 100) + "%";
        let r = "Poupador"; if(xp > 200) r = "Investidor"; if(xp > 500) r = "Shark";
        document.getElementById('rank-name').innerText = r;
        localStorage.setItem('master_xp', xp);
    }

    function showPage(p) {
        ['page-home', 'page-game', 'page-res'].forEach(id => document.getElementById(id).classList.add('hidden'));
        document.getElementById('page-' + p).classList.remove('hidden');
        if(p === 'home') renderLevels();
    }

    function renderLevels() {
        const list = document.getElementById('level-list'); list.innerHTML = "";
        course.forEach((lvl, i) => {
            const btn = document.createElement('div');
            btn.className = "opt"; btn.innerHTML = `<b>${lvl.name}</b>`;
            btn.onclick = () => startLevel(i);
            list.appendChild(btn);
        });
    }

    function startLevel(i) {
        curLvl = i; curStep = 0; showPage('game'); renderStep();
    }

    function renderStep() {
        const s = course[curLvl].questions[curStep];
        document.getElementById('game-title').innerText = s.title;
        document.getElementById('game-step').innerText = `${curStep + 1}/${course[curLvl].questions.length}`;
        document.getElementById('txt-tech').innerText = s.tech;
        document.getElementById('txt-real').innerText = s.real;
        document.getElementById('game-q').innerText = s.q;
        document.getElementById('study-link').href = s.link;
        document.getElementById('study-tip').classList.remove('hidden');
        
        const optArea = document.getElementById('options-area');
        const openArea = document.getElementById('open-area');
        optArea.innerHTML = ""; optArea.classList.add('hidden');
        openArea.classList.add('hidden');
        document.getElementById('btn-next').classList.add('hidden');

        if(s.type === 'multiple') {
            optArea.classList.remove('hidden');
            // Aleatoriedade das alternativas
            const shuffled = [...s.o].sort(() => Math.random() - 0.5);
            shuffled.forEach(txt => {
                const b = document.createElement('button'); b.className = "opt"; b.innerText = txt;
                b.onclick = () => {
                    selectedIdx = txt;
                    Array.from(optArea.children).forEach(c => c.classList.remove('selected'));
                    b.classList.add('selected');
                    document.getElementById('btn-next').classList.remove('hidden');
                };
                optArea.appendChild(b);
            });
        } else {
            openArea.classList.remove('hidden');
            document.getElementById('btn-next').classList.remove('hidden');
            document.getElementById('open-answer').value = "";
        }
    }

    function handleNext() {
        const s = course[curLvl].questions[curStep];
        let correct = false;

        if(s.type === 'multiple') {
            correct = (selectedIdx === s.c);
        } else {
            const val = document.getElementById('open-answer').value.toLowerCase();
            const hits = s.keys.filter(k => val.includes(k));
            correct = (hits.length >= 2); // Exige pelo menos 2 palavras-chave
        }

        if(correct) {
            playSound('win'); xp += 50; updateProfile();
            curStep++;
            if(curStep < course[curLvl].questions.length) renderStep();
            else finishLevel();
        } else {
            playSound('err'); alert("Resposta incompleta ou incorreta. Releia o conceito!");
        }
    }

    function finishLevel() {
        showPage('res');
        document.getElementById('res-body').innerHTML = `<h2>Módulo Concluído! 🎓</h2><p>Você ganhou XP e está mais perto de se tornar um Shark.</p><button class="btn-confirm" onclick="showPage('home')">Voltar</button>`;
    }

    function resetGame() { if(confirm("Resetar tudo?")) { localStorage.clear(); location.reload(); } }

    updateProfile(); renderLevels();
</script>
</body>
</html>
