<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>InvestMaster</title>
    <style>
        :root { --bg: #0F172A; --card: #1E293B; --acc: #38BDF8; --txt: #F8FAFC; --win: #22C55E; }
        body { font-family: sans-serif; background: var(--bg); color: var(--txt); margin: 0; display: flex; }
        
        #side-menu { width: 260px; height: 100vh; background: var(--card); position: fixed; left: -260px; transition: 0.3s; z-index: 1000; padding: 20px; border-right: 1px solid #334155; }
        #side-menu.active { left: 0; }
        .xp-bar { background: #334155; height: 10px; border-radius: 5px; margin: 10px 0; overflow: hidden; }
        #xp-fill { background: var(--acc); height: 100%; width: 0%; transition: 0.3s; }
        
        #menu-btn { position: fixed; left: 20px; top: 20px; z-index: 500; background: var(--acc); color: #000; border: none; padding: 10px 15px; border-radius: 8px; cursor: pointer; font-weight: bold; }
        
        main { flex: 1; display: flex; justify-content: center; padding: 20px; }
        .container { background: var(--card); width: 100%; max-width: 600px; padding: 30px; border-radius: 20px; margin-top: 80px; height: fit-content; }
        
        .box { background: rgba(255,255,255,0.05); padding: 15px; border-radius: 10px; margin: 15px 0; border-left: 4px solid var(--acc); }
        button.opt { width: 100%; padding: 15px; margin: 5px 0; background: #334155; border: 1px solid #475569; color: #fff; border-radius: 10px; text-align: left; cursor: pointer; }
        button.opt.selected { border-color: var(--acc); background: rgba(56, 189, 248, 0.2); }
        
        textarea { width: 100%; height: 80px; background: #0F172A; border: 1px solid #475569; color: #fff; border-radius: 10px; padding: 10px; resize: none; }
        .btn-next { width: 100%; padding: 15px; background: var(--win); border: none; border-radius: 10px; color: #fff; font-weight: bold; cursor: pointer; margin-top: 20px; }
        
        .hidden { display: none; }
        #overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 999; }
        .lvl-card { background: #334155; padding: 15px; border-radius: 10px; margin-bottom: 10px; cursor: pointer; }
    </style>
</head>
<body>

<div id="side-menu">
    <h3>Perfil</h3>
    <div id="rank">Poupador</div>
    <div style="font-size: 12px;">XP: <span id="xp-num">0</span></div>
    <div class="xp-bar"><div id="xp-fill"></div></div>
    <hr>
    <div onclick="showPage('home')" style="cursor:pointer; padding: 10px 0;">🏠 Início</div>
    <div onclick="reset()" style="cursor:pointer; padding: 10px 0; color: #EF4444;">Resetar</div>
</div>
<div id="overlay" onclick="toggleMenu()"></div>

<main>
    <button id="menu-btn" onclick="toggleMenu()">☰ MENU</button>
    <div class="container">
        <div id="page-home">
            <h2>Níveis</h2>
            <div id="lvl-list"></div>
        </div>
        <div id="page-game" class="hidden">
            <h3 id="q-title"></h3>
            <div class="box">
                <small>Explicação:</small>
                <div id="q-desc"></div>
            </div>
            <p id="q-text" style="font-weight: bold;"></p>
            <div id="opt-area"></div>
            <div id="open-area" class="hidden">
                <textarea id="open-ans" placeholder="Sua resposta..."></textarea>
            </div>
            <button id="btn-next" class="btn-next hidden" onclick="check()">Confirmar</button>
            <div style="margin-top: 15px; text-align: center;">
                <a id="q-link" href="#" target="_blank" style="color: var(--acc); font-size: 12px;">Estudar mais sobre o assunto →</a>
            </div>
        </div>
        <div id="page-res" class="hidden" style="text-align:center;">
            <h2>Módulo Concluído!</h2>
            <button class="btn-next" onclick="showPage('home')">Voltar</button>
        </div>
    </div>
</main>

<script>
    const db = [
        { name: "Módulo 1: Renda Fixa", qs: [
            { type: "m", t: "CDB", d: "Certificado de Depósito Bancário é um título onde o investidor empresta capital ao banco para financiar operações.", q: "Qual a função do investidor no CDB?", o: ["Credor", "Sócio", "Doador", "Corretor"], c: "Credor", l: "https://www.tesourodireto.com.br" },
            { type: "m", t: "Liquidez", d: "Capacidade de um ativo se transformar em dinheiro.", q: "O que caracteriza alta liquidez?", o: ["Resgate rápido", "Prazo de 5 anos", "Rendimento fixo", "Isenção de IR"], c: "Resgate rápido", l: "https://www.b3.com.br" },
            { type: "m", t: "FGC", d: "Entidade que protege depósitos bancários em caso de quebra da instituição.", q: "Qual o limite da garantia do FGC?", o: ["R$ 250 mil", "R$ 500 mil", "R$ 100 mil", "R$ 1 milhão"], c: "R$ 250 mil", l: "https://www.fgc.org.br" },
            { type: "o", t: "Dissertativa", d: "Reflexão sobre segurança em renda fixa.", q: "Explique a relação entre CDB e FGC:", k: ["banco", "garantia", "emprestar", "segurança"], l: "https://www.b3.com.br" }
        ]},
        { name: "Módulo 2: Renda Variável", qs: [
            { type: "m", t: "Ações", d: "Frações do capital social de uma empresa.", q: "O investidor de ações é um:", o: ["Sócio", "Credor", "Diretor", "Fiador"], c: "Sócio", l: "https://www.b3.com.br" },
            { type: "m", t: "Dividendos", d: "Distribuição de lucros aos acionistas.", q: "Qual a origem dos dividendos?", o: ["Lucro da empresa", "Empréstimos", "Taxa Selic", "Capital social"], c: "Lucro da empresa", l: "https://www.b3.com.br" },
            { type: "m", t: "FIIs", d: "Fundos que investem em imóveis ou títulos imobiliários.", q: "Como os FIIs de tijolo geram renda?", o: ["Aluguéis", "Venda de ações", "Taxas de banco", "IPCA"], c: "Aluguéis", l: "https://www.b3.com.br" },
            { type: "o", t: "Dissertativa", d: "Sobre o risco da renda variável.", q: "Por que as ações podem oscilar de preço?", k: ["mercado", "empresa", "oferta", "demanda", "lucro"], l: "https://www.b3.com.br" }
        ]},
        { name: "Módulo 3: Tesouro", qs: [
            { type: "m", t: "Tesouro Direto", d: "Programa de venda de títulos públicos federais.", q: "Quem garante os títulos do Tesouro?", o: ["Governo Federal", "Bancos", "Bolsa", "FGC"], c: "Governo Federal", l: "https://www.tesourodireto.com.br" },
            { type: "m", t: "IPCA+", d: "Título que rende inflação mais uma taxa fixa.", q: "Qual o objetivo do Tesouro IPCA+?", o: ["Ganho real", "Risco máximo", "Curto prazo", "Especulação"], c: "Ganho real", l: "https://www.tesourodireto.com.br" },
            { type: "m", t: "Marcação", d: "Ajuste do preço do título conforme as taxas do dia.", q: "Se os juros caem, o preço do título antigo:", o: ["Sobe", "Cai", "Soma", "Zera"], c: "Sobe", l: "https://www.tesourodireto.com.br" },
            { type: "o", t: "Dissertativa", d: "Estratégia de investimento.", q: "Por que investir no Tesouro é considerado seguro?", k: ["governo", "garantia", "soberano", "pagamento"], l: "https://www.tesourodireto.com.br" }
        ]}
    ];

    let xp = parseInt(localStorage.getItem('ixp')) || 0;
    let cl = 0, cs = 0, sel = null, ctx = new (window.AudioContext || window.webkitAudioContext)();

    function snd(f) { const o = ctx.createOscillator(), g = ctx.createGain(); o.connect(g); g.connect(ctx.destination); o.frequency.value = f; o.start(); g.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime + 0.2); o.stop(ctx.currentTime + 0.2); }
    function toggleMenu() { document.getElementById('side-menu').classList.toggle('active'); document.getElementById('overlay').style.display = document.getElementById('side-menu').classList.contains('active') ? 'block' : 'none'; }
    function update() { document.getElementById('xp-num').innerText = xp; document.getElementById('xp-fill').style.width = (xp/15) + "%"; document.getElementById('rank').innerText = xp > 800 ? "Shark" : (xp > 300 ? "Investidor" : "Poupador"); localStorage.setItem('ixp', xp); }
    function showPage(p) { ['page-home', 'page-game', 'page-res'].forEach(i => document.getElementById(i).classList.add('hidden')); document.getElementById('page-'+p).classList.remove('hidden'); if(p==='home') renderHome(); if(document.getElementById('side-menu').classList.contains('active')) toggleMenu(); }

    function renderHome() { const l = document.getElementById('lvl-list'); l.innerHTML = ""; db.forEach((m, i) => { const d = document.createElement('div'); d.className="lvl-card"; d.innerHTML=`${m.name} <span>▶</span>`; d.onclick=() => { cl=i; cs=0; showPage('game'); renderStep(); }; l.appendChild(d); }); }

    function renderStep() {
        const s = db[cl].qs[cs]; document.getElementById('q-title').innerText = s.t; document.getElementById('q-desc').innerText = s.d; document.getElementById('q-text').innerText = s.q; document.getElementById('q-link').href = s.l;
        const oa = document.getElementById('opt-area'), opa = document.getElementById('open-area'), btn = document.getElementById('btn-next');
        oa.innerHTML = ""; oa.classList.add('hidden'); opa.classList.add('hidden'); btn.classList.add('hidden');
        if(s.type === 'm') { oa.classList.remove('hidden'); [...s.o].sort(() => Math.random() - 0.5).forEach(txt => { const b = document.createElement('button'); b.className="opt"; b.innerText=txt; b.onclick=() => { sel=txt; Array.from(oa.children).forEach(x => x.classList.remove('selected')); b.classList.add('selected'); btn.classList.remove('hidden'); }; oa.appendChild(b); }); }
        else { opa.classList.remove('hidden'); btn.classList.remove('hidden'); document.getElementById('open-ans').value=""; }
    }

    function check() {
        const s = db[cl].qs[cs]; let ok = s.type==='m' ? (sel===s.c) : (s.k.filter(k => document.getElementById('open-ans').value.toLowerCase().includes(k)).length >= 2);
        if(ok) { snd(600); xp+=50; update(); cs++; if(cs < db[cl].qs.length) renderStep(); else showPage('res'); }
        else { snd(200); alert("Incorreto. Releia a explicação."); }
    }

    function reset() { if(confirm("Resetar?")) { localStorage.clear(); location.reload(); } }
    update(); renderHome();
</script>
</body>
</html>
