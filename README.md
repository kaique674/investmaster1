
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>InvestMaster - Autodidata</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --bg: #F8F9FA;
            --white: #FFFFFF;
            --primary: #1A252F;
            --accent: #2980B9;
            --success: #27AE60;
            --danger: #E74C3C;
        }

        body { font-family: 'Segoe UI', sans-serif; background: var(--bg); margin: 0; display: flex; color: #333; }

        /* Menu Lateral */
        #side-menu {
            width: 260px; height: 100vh; background: var(--primary); color: white;
            position: fixed; left: -260px; transition: 0.3s; z-index: 1000; padding: 25px 15px;
        }
        #side-menu.active { left: 0; }
        .menu-link { display: block; color: #BDC3C7; padding: 15px; text-decoration: none; border-bottom: 1px solid #2C3E50; cursor: pointer; }
        .menu-link:hover { color: white; background: #2C3E50; border-radius: 8px; }
        #overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); z-index: 999; }

        /* Container */
        main { flex: 1; padding: 20px; display: flex; justify-content: center; min-height: 100vh; }
        .container { background: var(--white); width: 100%; max-width: 550px; padding: 35px; border-radius: 25px; box-shadow: 0 15px 40px rgba(0,0,0,0.08); position: relative; margin-top: 40px; }
        #menu-btn { font-size: 28px; cursor: pointer; color: var(--primary); position: absolute; left: 20px; top: 20px; z-index: 500; }

        /* Estilos de Aula */
        .level-card { background: #fff; border: 2px solid #EDF2F7; border-radius: 18px; padding: 20px; margin-bottom: 15px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; }
        .level-card.locked { opacity: 0.5; cursor: not-allowed; background: #F2F4F4; }
        .box { background: #F0F7FF; border: 1px solid #D1E9FF; padding: 20px; border-radius: 15px; margin: 20px 0; }
        .label-t { color: #2D3748; font-weight: 800; font-size: 11px; text-transform: uppercase; display: block; }
        .label-i { color: var(--success); font-weight: 700; display: block; margin-top: 15px; font-size: 11px; text-transform: uppercase; }
        
        button.opt { width: 100%; padding: 18px; margin: 10px 0; border: 2px solid #EDF2F7; border-radius: 12px; background: white; font-weight: 600; cursor: pointer; text-align: left; }
        button.opt.selected { background: var(--accent); color: white; border-color: var(--accent); }
        .btn-main { width: 100%; padding: 18px; background: var(--primary); color: white; border: none; border-radius: 12px; font-weight: 700; cursor: pointer; margin-top: 15px; }
        .hidden { display: none; }
    </style>
</head>
<body>

<div id="side-menu">
    <h2 style="padding-left: 15px;">InvestMaster 🎓</h2>
    <div class="menu-link" onclick="mostrarTela('home')">🏠 Trilha de Níveis</div>
    <div class="menu-link" onclick="mostrarTela('progresso')">📈 Meu Progresso</div>
    <div class="menu-link" onclick="limparHistorico()" style="color: #E74C3C; margin-top: 50px;">🗑️ Reiniciar Tudo</div>
</div>
<div id="overlay" onclick="toggleMenu()"></div>

<main>
    <div id="menu-btn" onclick="toggleMenu()">☰</div>
    
    <div class="container">
        <div id="tela-home">
            <h2 style="text-align: center;">Escolha seu Nível</h2>
            <div id="lista-niveis"></div>
        </div>

        <div id="tela-jogo" class="hidden">
            <h3 id="game-title" style="color: var(--accent); margin: 0;"></h3>
            <div id="icon-display" style="font-size: 60px; text-align: center; margin: 10px 0;"></div>
            <div class="box">
                <span class="label-t">📘 Explicação Técnica</span>
                <div id="txt-tecnico" style="margin-bottom: 10px;"></div>
                <hr style="border: 0; border-top: 1px solid #D1E9FF;">
                <span class="label-i">💡 Traduzindo (O que importa)</span>
                <div id="txt-informal"></div>
            </div>
            <p style="font-weight: bold; background: #FFF9C4; padding: 5px; display: inline-block;">Agora responda:</p>
            <p id="game-question" style="margin-top: 5px;"></p>
            <div id="game-options"></div>
            <button id="btn-confirmar" class="btn-main hidden" onclick="confirmarResposta()">Confirmar Resposta</button>
        </div>

        <div id="tela-resultado" class="hidden" style="text-align: center;">
            <div id="resultado-content"></div>
        </div>

        <div id="tela-progresso" class="hidden">
            <h2 style="text-align: center;">Evolução</h2>
            <canvas id="progressoChart"></canvas>
            <button class="btn-main" onclick="mostrarTela('home')">Voltar</button>
        </div>
    </div>
</main>

<script>
    const trilha = [
        {
            id: 1, nome: "Nível 1: O Início", status: "unlocked",
            fases: [
                { 
                    t: "Liquidez Diária", e: "💧", 
                    tecnico: "Capacidade de converter um ativo em caixa prontamente sem perda significativa de valor.", 
                    informal: "É a velocidade de pegar seu dinheiro de volta. Se tem 'Liquidez Diária', você saca hoje mesmo.", 
                    q: "Se um investimento tem Liquidez Diária, quando você pode resgatar o dinheiro?", 
                    o: ["A qualquer momento (hoje)", "Somente após 2 anos"], c: 0 
                },
                { 
                    t: "CDB e FGC", e: "🛡️", 
                    tecnico: "O Fundo Garantidor de Créditos assegura depósitos em CDBs até o limite de R$ 250 mil por CPF/Instituição.", 
                    informal: "O FGC é o seu 'seguro'. Se o banco quebrar, ele te devolve até 250 mil reais.", 
                    q: "Qual o valor máximo que o FGC protege por banco/CPF?", 
                    o: ["Até R$ 250.000,00", "Qualquer valor, sem limite"], c: 0 
                }
            ]
        },
        {
            id: 2, nome: "Nível 2: O Sócio", status: "locked",
            fases: [
                { 
                    t: "Dividendos", e: "🍎", 
                    tecnico: "Proventos pagos em pecúnia pela companhia aos seus acionistas como parte dos lucros líquidos.", 
                    informal: "Dividendos são pedaços do lucro da empresa que ela deposita na sua conta por você ser sócio.", 
                    q: "O que são os Dividendos pagos pelas empresas?", 
                    o: ["Uma parte do lucro dividida com o sócio", "Um imposto que o investidor paga"], c: 0 
                }
            ]
        }
    ];

    let nivelAtual = 0; let faseAtual = 0; let acertosNivel = 0;
    let escolha = null; let progressoScores = JSON.parse(localStorage.getItem('progresso')) || [];

    function toggleMenu() {
        document.getElementById('side-menu').classList.toggle('active');
        document.getElementById('overlay').style.display = document.getElementById('side-menu').classList.contains('active') ? 'block' : 'none';
    }

    function mostrarTela(id) {
        ['tela-home', 'tela-jogo', 'tela-resultado', 'tela-progresso'].forEach(t => document.getElementById(t).classList.add('hidden'));
        document.getElementById('tela-' + id).classList.remove('hidden');
        if(id === 'home') renderMenu();
        if(id === 'progresso') renderGrafico();
        if(document.getElementById('side-menu').classList.contains('active')) toggleMenu();
    }

    function renderMenu() {
        const lista = document.getElementById('lista-niveis');
        lista.innerHTML = "";
        trilha.forEach((n, i) => {
            const card = document.createElement('div');
            card.className = `level-card ${n.status}`;
            card.innerHTML = `<div><strong>${n.nome}</strong></div> <span>${n.status === 'locked' ? '🔒' : '▶️'}</span>`;
            if(n.status !== 'locked') card.onclick = () => iniciarNivel(i);
            lista.appendChild(card);
        });
    }

    function iniciarNivel(i) {
        nivelAtual = i; faseAtual = 0; acertosNivel = 0;
        mostrarTela('jogo');
        renderFase();
    }

    function renderFase() {
        const f = trilha[nivelAtual].fases[faseAtual];
        document.getElementById('game-title').innerText = f.t;
        document.getElementById('icon-display').innerText = f.e;
        document.getElementById('txt-tecnico').innerText = f.tecnico;
        document.getElementById('txt-informal').innerText = f.informal;
        document.getElementById('game-question').innerText = f.q;
        const opts = document.getElementById('game-options');
        opts.innerHTML = "";
        f.o.forEach((txt, i) => {
            const b = document.createElement('button');
            b.className = 'opt';
            b.innerText = txt;
            b.onclick = () => {
                escolha = i;
                Array.from(opts.children).forEach(el => el.classList.remove('selected'));
                b.classList.add('selected');
                document.getElementById('btn-confirmar').classList.remove('hidden');
            };
            opts.appendChild(b);
        });
    }

    function confirmarResposta() {
        if(escolha === trilha[nivelAtual].fases[faseAtual].c) acertosNivel++;
        faseAtual++; escolha = null;
        document.getElementById('btn-confirmar').classList.add('hidden');
        if(faseAtual < trilha[nivelAtual].fases.length) renderFase();
        else finalizarNivel();
    }

    function finalizarNivel() {
        mostrarTela('resultado');
        const total = trilha[nivelAtual].fases.length;
        const passou = acertosNivel === total;
        let html = `<h2>${passou ? '📜 Diploma Liberado!' : '⚠️ Precisa Revisar'}</h2>`;
        html += `<p>Você acertou ${acertosNivel} de ${total}.</p>`;
        if(passou) {
            trilha[nivelAtual].status = 'unlocked'; // Mantém unlocked ou marca como completo
            if(trilha[nivelAtual+1]) trilha[nivelAtual+1].status = 'unlocked';
            progressoScores.push(100);
        } else {
            progressoScores.push(Math.round((acertosNivel/total)*100));
        }
        localStorage.setItem('progresso', JSON.stringify(progressoScores));
        html += `<button class="btn-main" onclick="mostrarTela('home')">Voltar ao Menu</button>`;
        document.getElementById('resultado-content').innerHTML = html;
    }

    function renderGrafico() {
        const ctx = document.getElementById('progressoChart').getContext('2d');
        if(window.myChart) window.myChart.destroy();
        window.myChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: progressoScores.map((_, i) => i + 1),
                datasets: [{ label: 'Score %', data: progressoScores, borderColor: '#2980B9', fill: false }]
            }
        });
    }

    function limparHistorico() { localStorage.clear(); location.reload(); }

    renderMenu();
</script>
</body>
</html>
