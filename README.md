<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>InvestMaster - Versão Final</title>
    <style>
        :root {
            --bg: #0F172A; --card: #1E293B; --acc: #38BDF8; 
            --txt: #F8FAFC; --win: #22C55E; --err: #EF4444;
        }
        body { font-family: 'Inter', system-ui, sans-serif; background: var(--bg); color: var(--txt); margin: 0; display: flex; }

        /* Menu Lateral */
        #side-menu {
            width: 270px; height: 100vh; background: var(--card); position: fixed; 
            left: -270px; transition: 0.4s cubic-bezier(0.4, 0, 0.2, 1); z-index: 1000; 
            padding: 30px 20px; border-right: 1px solid #334155;
        }
        #side-menu.active { left: 0; }
        .rank-box { background: #0F172A; padding: 20px; border-radius: 15px; text-align: center; margin-bottom: 25px; border: 1px solid #334155; }
        .xp-bar { background: #334155; height: 8px; border-radius: 4px; margin-top: 10px; overflow: hidden; }
        #xp-fill { background: var(--acc); height: 100%; width: 0%; transition: 0.5s; }
        
        #menu-btn { 
            position: fixed; left: 25px; top: 25px; z-index: 500; 
            background: var(--acc); color: #000; border: none; 
            padding: 12px 20px; border-radius: 10px; cursor: pointer; font-weight: bold; 
        }

        /* Container Principal */
        main { flex: 1; display: flex; justify-content: center; padding: 20px; min-height: 100vh; }
        .container { 
            background: var(--card); width: 100%; max-width: 600px; padding: 40px; 
            border-radius: 24px; margin-top: 90px; height: fit-content;
            box-shadow: 0 20px 50px rgba(0,0,0,0.3);
        }

        /* Elementos de Jogo */
        .lesson-box { background: rgba(56, 189, 248, 0.05); padding: 20px; border-radius: 12px; margin: 20px 0; border-left: 4px solid var(--acc); }
        .label { font-size: 10px; font-weight: bold; text-transform: uppercase; color: var(--acc); display: block; margin-bottom: 5px; }
        
        button.opt { 
            width: 100%; padding: 16px; margin: 8px 0; background: #334155; 
            border: 1px solid #475569; color: #fff; border-radius: 12px; 
            text-align: left; cursor: pointer; transition: 0.2s;
        }
        button.opt:hover { border-color: var(--acc); background: #3E4C5E; }
        button.opt.selected { border-color: var(--acc); background: rgba(56, 189, 248, 0.2); }

        textarea { 
            width: 100%; height: 100px; background: #0F172A; border: 1px solid #475569; 
            color: #fff; border-radius: 12px; padding: 15px; margin-top: 10px; resize: none; font-family: inherit;
        }

        .btn-action { 
            width: 100%; padding: 18px; background: var(--win); border: none; 
            border-radius: 12px; color: #fff; font-weight: bold; cursor: pointer; margin-top: 20px;
        }

        .hidden { display: none; }
        #overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.6); z-index: 999; backdrop-filter: blur(3px); }
        .lvl-item { background: #334155; padding: 20px; border-radius: 12px; margin-bottom: 12px; cursor: pointer; display: flex; justify-content: space-between; }
    </style>
</head>
<body>

<div id="side-menu">
    <div class="rank-box">
        <div style="font-size: 30px;">💎</div>
        <div id="rank-txt" style="font-weight: bold; margin-top: 10px;">Poupador</div>
        <div style="font-size: 12px; opacity: 0.7;">XP: <span id="xp-num">0</span></div>
        <div class="xp-bar"><div id="xp-fill"></div></div>
    </div>
    <div onclick="showPage('home')" style="cursor:pointer; padding: 15px 0; border-bottom: 1px solid #334155;">🏠 Início</div>
    <div onclick="resetData()" style="cursor:pointer; padding: 15px 0; color: var(--err);">⚠️ Resetar Progresso</div>
</div>
<div id="overlay" onclick="toggleMenu()"></div>

<main>
    <button id="menu-btn" onclick="toggleMenu()">☰ MENU</button>

    <div class="container">
        <div id="page-home">
            <h2 style="text-align: center;">Módulos de Estudo</h2>
            <div id="lvl-list"></div>
        </div>

        <div id="page-game" class="hidden">
            <div id="game-header" style="display:flex; justify-content: space-between; font-size: 12px; color: var(--acc);">
                <b id="q-module"></b>
                <span id="q-count"></span>
            </div>
            
            <div class="lesson-box">
                <span class="label">Conceito Técnico</span>
                <div id="q-desc" style="font-size: 14px; line-height: 1.5;"></div>
            </div>

            <p id="q-text" style="font-weight: bold; font-size: 17px; margin: 25px 0;"></p>
            
            <div id="opt-area"></div>
            <div id="open-area" class="hidden">
                <textarea id="open-ans" placeholder="Descreva sua resposta com base na explicação acima..."></textarea>
            </div>

            <button id="btn-check" class="btn-action hidden" onclick="validate()">Confirmar Resposta</button>
            
            <div style="margin-top: 30px; text-align: center;">
                <a id="q-link" href="#" target="_blank" style="color: var(--acc); font-size: 13px; text-decoration: none; border-bottom: 1px solid;">📚 Acessar Documentação Oficial</a>
            </div>
        </div>

        <div id="page-res" class="hidden" style="text-align:center;">
            <div style="font-size: 50px;">🎉</div>
            <h2>Módulo Concluído!</h2>
            <p>Você absorveu novos conhecimentos técnicos.</p>
            <button class="btn-action" onclick="showPage('home')">Próximo Desafio</button>
        </div>
    </div>
</main>

<script>
    const database = [
        {
            name: "Módulo 1: Renda Fixa",
            steps: [
                { type: "m", desc: "CDB é um título nominativo emitido por bancos para captação de recursos. O investidor atua como financiador da instituição.", q: "Qual a relação entre investidor e banco no CDB?", o: ["Credor e Devedor", "Sócio e Empresa", "Doador e Beneficiário", "Cliente e Vendedor"], c: "Credor e Devedor", l: "https://www.tesourodireto.com.br" },
                { type: "m", desc: "Liquidez indica a velocidade
