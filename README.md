<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Edu Finance — Trilha de Investimentos</title>
  <style>
    :root{
      --bg:#0b0b0d;
      --card:#0f1113;
      --muted:#9aa4ad;
      --accent:#34d399; /* green */
      --danger:#ff6b6b;
      --glass: rgba(255,255,255,0.04);
      --glass-2: rgba(255,255,255,0.02);
      --text:#e6eef3;
      --max-width:900px;
      --touch:44px;
    }

    html,body{height:100%;}
    body{
      margin:0;
      font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background:linear-gradient(180deg,#050506 0%, #0b0b0d 100%);
      color:var(--text);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      display:flex;
      justify-content:center;
      padding:20px;
    }

    .app {
      width:100%;
      max-width:var(--max-width);
      display:flex;
      gap:16px;
      align-items:flex-start;
    }

    /* Sidebar (hamburger) */
    .sidebar {
      width:64px;
      min-width:64px;
      background:var(--card);
      border-radius:12px;
      padding:12px 8px;
      box-shadow:0 6px 18px rgba(0,0,0,0.6);
      position:sticky;
      top:20px;
      height:fit-content;
      display:flex;
      flex-direction:column;
      gap:12px;
      align-items:center;
    }
    .hamburger {
      width:40px;
      height:40px;
      background:rgba(255,255,255,0.03);
      border-radius:10px;
      display:flex;
      align-items:center;
      justify-content:center;
      cursor:pointer;
      margin-top:8px;
      transform:translateX(8%); /* descolado da margem */
    }
    .hamburger:active { transform:translateX(9%); }

    .avatar {
      width:40px;
      height:40px;
      border-radius:8px;
      background:linear-gradient(180deg,#1f2937,#111318);
      display:flex;
      align-items:center;
      justify-content:center;
      font-weight:600;
      color:var(--muted);
    }

    .sidebar button {
      background:transparent;
      border:0;
      color:var(--muted);
      cursor:pointer;
      font-size:13px;
    }

    /* Main content */
    .main {
      flex:1;
    }

    header.app-header {
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      margin-bottom:12px;
    }

    .title {
      display:flex;
      flex-direction:column;
    }
    .title h1 {
      margin:0;
      font-size:18px;
      letter-spacing:0.2px;
    }
    .title p { margin:0; color:var(--muted); font-size:13px; }

    .progress-wrap {
      width:320px;
      max-width:55%;
      background:var(--glass-2);
      border-radius:999px;
      padding:6px;
      display:flex;
      align-items:center;
      gap:8px;
    }
    .xp-bar {
      position:relative;
      background:linear-gradient(90deg,#0b1220,#0b0b0d);
      height:14px;
      flex:1;
      border-radius:999px;
      overflow:hidden;
    }
    .xp-fill {
      height:100%;
      width:0%;
      background:linear-gradient(90deg,#34d399,#06b6d4);
      transition:width 700ms cubic-bezier(.2,.9,.2,1);
    }
    .xp-label { font-size:13px; color:var(--muted); min-width:120px; text-align:right; }

    /* Question card */
    .card {
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:12px;
      padding:18px;
      box-shadow:0 8px 30px rgba(2,6,23,0.6);
    }

    .meta {
      display:flex;
      gap:12px;
      align-items:center;
      margin-bottom:8px;
      color:var(--muted);
      font-size:13px;
    }

    .question {
      font-size:16px;
      margin:12px 0;
      line-height:1.4;
    }

    .answers {
      display:flex;
      flex-direction:column;
      gap:10px;
    }

    .btn {
      background:var(--glass);
      color:var(--text);
      border:1px solid rgba(255,255,255,0.03);
      padding:12px 14px;
      border-radius:10px;
      cursor:pointer;
      text-align:left;
      font-size:15px;
      min-height:var(--touch);
      display:flex;
      align-items:center;
      gap:10px;
    }
    .btn:hover { transform:translateY(-1px); }
    .btn:active { transform:translateY(0); }

    .btn[disabled] { opacity:0.5; cursor:default; transform:none; }

    .feedback {
      margin-top:12px;
      padding:12px;
      border-radius:10px;
      background:linear-gradient(180deg, rgba(52,211,153,0.06), rgba(52,211,153,0.03));
      color:var(--accent);
      display:none;
    }
    .feedback.err {
      background:linear-gradient(180deg, rgba(255,107,107,0.06), rgba(255,107,107,0.03));
      color:var(--danger);
    }
    .explanation {
      margin-top:12px;
      padding:12px;
      border-radius:10px;
      background:var(--glass-2);
      color:var(--muted);
      font-size:14px;
      display:none;
    }

    .links {
      margin-top:12px;
      display:flex;
      gap:8px;
      flex-wrap:wrap;
    }
    .links a {
      color:var(--muted);
      text-decoration:none;
      background:transparent;
      border:1px solid rgba(255,255,255,0.03);
      padding:8px 10px;
      border-radius:8px;
      font-size:13px;
    }

    .controls {
      margin-top:14px;
      display:flex;
      gap:8px;
      justify-content:space-between;
      align-items:center;
    }
    .small {
      font-size:13px;
      color:var(--muted);
    }

    .level-select {
      display:flex;
      gap:8px;
    }
    .level-btn {
      padding:8px 12px;
      background:transparent;
      border-radius:8px;
      border:1px solid rgba(255,255,255,0.03);
      color:var(--muted);
      cursor:pointer;
    }
    .level-btn.active {
      background:linear-gradient(90deg,#071218,#0a1720);
      border:1px solid rgba(255,255,255,0.05);
      color:var(--text);
    }

    footer {
      margin-top:18px;
      font-size:13px;
      color:var(--muted);
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:8px;
    }

    /* Responsive mobile-first */
    @media(min-width:880px){
      .app { padding:24px; }
      .sidebar { width:80px; min-width:80px; padding:16px 10px; }
      .progress-wrap { max-width:360px; }
    }

    /* tiny helpers */
    .muted { color:var(--muted); }
    .kbd { font-family:monospace; font-size:12px; background:rgba(255,255,255,0.03); padding:4px 6px; border-radius:6px; }
  </style>
</head>
<body>
  <div class="app" id="app">
    <aside class="sidebar">
      <div class="avatar" id="avatar">KF</div>
      <div class="hamburger" title="Menu" id="menuBtn">☰</div>
      <button id="soundToggle" title="Ativar/Desativar sons">🔊</button>
      <button id="resetBtn" title="Resetar progresso">♻️</button>
      <div style="flex:1"></div>
      <div class="small muted">Edu Finance</div>
    </aside>

    <main class="main">
      <header class="app-header">
        <div class="title">
          <h1>Trilha: Investimentos Básicos → Avançado</h1>
          <p id="userLine">Usuário: kaique674</p>
        </div>

        <div class="progress-wrap" aria-hidden="false">
          <div style="width:12px; text-align:center; color:var(--muted); font-size:12px;">XP</div>
          <div class="xp-bar" aria-hidden="true">
            <div class="xp-fill" id="xpFill" style="width:0%"></div>
          </div>
          <div class="xp-label" id="xpLabel">0 XP</div>
        </div>
      </header>

      <section class="card" id="card">
        <div class="meta">
          <div class="muted" id="levelLabel">Nível 1 — Renda Fixa</div>
          <div style="flex:1"></div>
          <div class="muted">Rank: <span id="rankLabel">Poupador</span></div>
        </div>

        <div style="display:flex; justify-content:space-between; align-items:center; gap:8px; flex-wrap:wrap;">
          <div class="level-select" id="levelSelect">
            <button class="level-btn active" data-level="0">Nível 1</button>
            <button class="level-btn" data-level="1">Nível 2</button>
            <button class="level-btn" data-level="2">Nível 3</button>
          </div>
          <div class="small muted">XP para próximo Rank: <span id="xpToNext">1000</span></div>
        </div>

        <hr style="border:none; height:8px; opacity:0.02; margin:12px 0;">

        <div id="questionArea">
          <!-- dynamically filled -->
        </div>

        <footer>
          <div class="small muted">Mobile-first • Dark mode • Links oficiais no final de cada questão</div>
          <div><button id="startLevelBtn" class="level-btn">Iniciar/Resetar Nível</button></div>
        </footer>
      </section>

      <div style="height:12px"></div>

      <section class="card" id="logCard" style="display:none">
        <div class="muted" id="logTitle">Resumo do Nível</div>
        <div id="logContent" style="margin-top:8px; color:var(--muted); font-size:14px;"></div>
      </section>

    </main>
  </div>

  <script>
    /******************************************************************
     * Edu Finance — Single file HTML app
     * - Mobile-first, Dark Mode
     * - 3 níveis com 4 perguntas cada (3 MCQ + 1 dissertativa)
     * - Alternativas embaralhadas no início do nível
     * - Questões dissertativas com verificação de palavras-chave
     * - XP, ranks, progress bar, LocalStorage per user
     * - Sounds via WebAudio
     *
     * User from conversation: kaique674
     ******************************************************************/

    // ----- Configuration & Content -----
    const USER = 'kaique674';
    const STORAGE_KEY = `edu.finance:progress:${USER}`;
    const RANKS = [
      { name: 'Poupador', min: 0, max: 999 },
      { name: 'Investidor', min: 1000, max: 4999 },
      { name: 'Shark', min: 5000, max: Infinity }
    ];

    // Questions data. Each MCQ includes `answerIndex` (internal correct index in the original alternatives array)
    // Explanations do not indicate the correct option.
    const QUIZ = [
      // Nível 1 — Renda Fixa
      [
        {
          id: '1-1',
          type: 'mcq',
          question: 'O que é um CDB (Certificado de Depósito Bancário)?',
          alternatives: [
            'Depósito emitido por banco que remunera o aplicador com juros.',
            'Uma ação de instituição financeira.',
            'Um título público emitido pelo Tesouro Nacional.',
            'Um seguro de vida com componente de investimento.'
          ],
          answerIndex: 0,
          explanation: 'CDBs são títulos emitidos por bancos para captar recursos. O investidor empresta dinheiro ao banco e recebe remuneração contratada (prefixada, pós‑fixada atrelada a um indexador — ex.: CDI — ou híbrida). O risco é o risco de crédito do emissor.',
          summary: 'Pense como um empréstimo ao banco que paga juros. Compare liquidez e garantia (FGC) antes de aplicar.',
          links: [
            { title: 'B3 — Renda Fixa', url: 'https://www.b3.com.br' },
            { title: 'FGC — Informações', url: 'https://www.fgc.org.br' }
          ],
          xp: 80
        },
        {
          id: '1-2',
          type: 'mcq',
          question: 'Em termos de liquidez, qual afirmação é mais adequada?',
          alternatives: [
            'Alguns CDBs oferecem liquidez diária; outros exigem prazo de carência.',
            'Todos os investimentos em renda fixa têm liquidez diária.',
            'Tesouro Direto nunca pode ser resgatado antes do vencimento.',
            'A liquidez depende apenas do valor aplicado, não do produto.'
          ],
          answerIndex: 0,
          explanation: 'Produtos de renda fixa variam: alguns permitem resgate diário, outros possuem carência ou marcação a mercado. Tesouro Direto tem negociação em mercado secundário, com preço variável.',
          summary: 'Verifique “liquidez” no contrato do produto e se há penalidades ou marcação a mercado em caso de resgate antecipado.',
          links: [
            { title: 'Tesouro Direto — FAQ', url: 'https://www.tesourodireto.com.br' },
            { title: 'B3 — Liquidez em Renda Fixa', url: 'https://www.b3.com.br' }
          ],
          xp: 60
        },
        {
          id: '1-3',
          type: 'mcq',
          question: 'O FGC (Fundo Garantidor de Créditos) fornece qual tipo de proteção?',
          alternatives: [
            'Garantia sobre depósitos e créditos em instituições financeiras até um limite por CPF/CNPJ por instituição.',
            'Garantia sobre títulos públicos federais.',
            'Seguro obrigatoriamente contratado em todas as aplicações.',
            'Proteção ilimitada para quaisquer investimentos em renda fixa.'
          ],
          answerIndex: 0,
          explanation: 'O FGC tem regras de cobertura por depositante e por instituição; é uma camada de proteção em caso de intervenção ou falência da instituição participante. Consulte o FGC para saber o limite e as condições atualizadas.',
          summary: 'Verifique se o produto e a instituição são cobertos pelo FGC e saiba que há limite por depositante/instituição.',
          links: [
            { title: 'FGC — Regras e Cobertura', url: 'https://www.fgc.org.br' }
          ],
          xp: 80
        },
        {
          id: '1-4',
          type: 'open',
          question: 'Explique, em até 6 linhas, as diferenças práticas entre CDB e poupança considerando liquidez e proteção por FGC.',
          requiredKeywords: ['banco', 'juros', 'fgc', 'liquidez'],
          minKeywordsRequired: 3,
          xp: 200
        }
      ],

      // Nível 2 — Renda Variável
      [
        {
          id: '2-1',
          type: 'mcq',
          question: 'O que são dividendos?',
          alternatives: [
            'Parcela dos lucros distribuída aos acionistas quando a empresa decide repartir.',
            'Um imposto cobrado sobre a venda de ações.',
            'Um tipo de título público indexado à inflação.',
            'Uma garantia do governo sobre rendimentos de FIIs.'
          ],
          answerIndex: 0,
          explanation: 'Dividendos são distribuições realizadas por empresas a acionistas quando há lucro e deliberação. A política de distribuição varia por empresa; a tributação depende da jurisdição e do tipo de provento.',
          summary: 'Dividendo = distribuição de lucro; diferencie de juros sobre capital próprio e de ganho de capital (valorização da ação).',
          links: [ { title: 'B3 — Proventos', url: 'https://www.b3.com.br' } ],
          xp: 90
        },
        {
          id: '2-2',
          type: 'mcq',
          question: 'A principal diferença entre ações ordinárias (ON) e preferenciais (PN) é:',
          alternatives: [
            'Direitos de voto versus prioridade em dividendos (cada tipo tem características distintas).',
            'Uma é garantida pelo FGC e a outra não.',
            'Preferenciais representam títulos de dívida; ordinárias são títulos de propriedade.',
            'Preferenciais são negociadas apenas no mercado de balcão.'
          ],
          answerIndex: 0,
          explanation: 'Tipos de ações atribuem direitos diferentes (voto, prioridade em proventos). As regras específicas dependem do estatuto da companhia e legislação.',
          summary: 'Ao escolher ações, verifique se você precisa do direito de voto ou de preferência em recebimento de proventos.',
          links: [ { title: 'B3 — Tipos de Ações', url: 'https://www.b3.com.br' } ],
          xp: 70
        },
        {
          id: '2-3',
          type: 'mcq',
          question: 'Sobre FIIs (Fundos Imobiliários), assinale a afirmação mais precisa:',
          alternatives: [
            'FIIs são negociados em bolsa e podem distribuir rendimentos periodicamente; não são cobertos pelo FGC.',
            'FIIs são protegidos integralmente pelo FGC.',
            'FIIs não podem distribuir rendimentos a cotistas.',
            'FIIs são títulos do Tesouro Nacional.'
          ],
          answerIndex: 0,
          explanation: 'FIIs representam cotas de fundos que investem em ativos imobiliários; são negociados em bolsa e suas cotas têm liquidez de mercado variável. A garantia do FGC não cobre cotas de fundos imobiliários.',
          summary: 'Avalie liquidez, taxa de administração e qualidade dos ativos antes de comprar cotas de FIIs.',
          links: [ { title: 'B3 — Guia de FIIs', url: 'https://www.b3.com.br' } ],
          xp: 80
        },
        {
          id: '2-4',
          type: 'open',
          question: 'Diferencie rendimento por dividendos de FIIs e valorização de preço (ganho de capital). Use termos técnicos.',
          requiredKeywords: ['dividendos', 'valorização', 'yield', 'tributação'],
          minKeywordsRequired: 3,
          xp: 220
        }
      ],

      // Nível 3 — Avançado / Tesouro
      [
        {
          id: '3-1',
          type: 'mcq',
          question: 'O que representa um título IPCA+ (Tesouro IPCA+ ou NTN‑B)?',
          alternatives: [
            'Pagamento da inflação (IPCA) acrescida de uma taxa fixa contratada.',
            'Título com rendimento fixo sem correção por inflação.',
            'Um tipo de ação indexada ao IPCA.',
            'Um empréstimo entre bancos no mercado interbancário.'
          ],
          answerIndex: 0,
          explanation: 'Títulos atrelados ao IPCA protegem o poder de compra, pagando inflação mais prêmio fixo; ao serem negociados antes do vencimento, sofrem variação de preço por causa de taxas de juros e marcação a mercado.',
          summary: 'IPCA+ é indicado para proteção contra inflação no longo prazo; atenção à volatilidade se vender antes do vencimento.',
          links: [ { title: 'Tesouro IPCA+ — Tesouro Direto', url: 'https://www.tesourodireto.com.br' } ],
          xp: 120
        },
        {
          id: '3-2',
          type: 'mcq',
          question: 'O que é marcação a mercado (mark‑to‑market)?',
          alternatives: [
            'Ajuste diário do preço de um ativo ao seu valor corrente no mercado, refletindo variações nas taxas de juros e expectativas.',
            'Uma garantia paga pelo FGC aos detentores de títulos.',
            'Uma taxa cobrada pelo emissor do título ao resgatar antecipadamente.',
            'Uma técnica usada apenas para ações e não para títulos.'
          ],
          answerIndex: 0,
          explanation: 'Marcação a mercado traduz a variação do preço de um ativo conforme condições de mercado (taxas, liquidez, risco). Para títulos de renda fixa negociados, isso gera ganhos ou perdas não realizados até a efetiva venda.',
          summary: 'Ao comprar títulos com pagamento na data de vencimento, a marcação a mercado não altera o principal se mantidos até o vencimento; ao vender antes, o preço depende das taxas vigentes.',
          links: [ { title: 'Tesouro Direto — Marcação a Mercado', url: 'https://www.tesourodireto.com.br' } ],
          xp: 110
        },
        {
          id: '3-3',
          type: 'mcq',
          question: 'Qual fator deixa um título prefixado mais sensível a marcação a mercado?',
          alternatives: [
            'Maior duração (ou prazo mais longo) aumenta a sensibilidade a variações de taxa de juros.',
            'Títulos com prazo curto são mais sensíveis que os de prazo longo.',
            'Marcação a mercado não é afetada por alterações nas taxas de juros.',
            'A volatilidade de um título depende apenas do emissor e não do prazo.'
          ],
          answerIndex: 0,
          explanation: 'A sensibilidade do preço de um título a variações nas taxas (duration/convexity) cresce com o prazo até o vencimento; outros fatores influem, mas prazo é central.',
          summary: 'Para reduzir risco de preço se houver queda/elevação de juros, escolha títulos de prazo mais curto ou prefira manter até vencimento.',
          links: [ { title: 'Tesouro Direto — Duração', url: 'https://www.tesourodireto.com.br' } ],
          xp: 130
        },
        {
          id: '3-4',
          type: 'open',
          question: 'Explique como a marcação a mercado pode impactar os ganhos não realizados de um Tesouro IPCA+ quando vendido antes do vencimento.',
          requiredKeywords: ['marcação a mercado', 'taxa de juros', 'vencimento', 'ganhos não realizados'],
          minKeywordsRequired: 3,
          xp: 300
        }
      ]
    ];

    // ----- Utility functions -----
    function shuffleArray(arr) {
      const a = arr.slice();
      for (let i = a.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
      }
      return a;
    }

    function beep(frequency=440, duration=150, type='sine') {
      if (!appState.soundsEnabled) return;
      try {
        const ctx = new (window.AudioContext || window.webkitAudioContext)();
        const o = ctx.createOscillator();
        const g = ctx.createGain();
        o.type = type;
        o.frequency.value = frequency;
        o.connect(g);
        g.connect(ctx.destination);
        g.gain.value = 0.0001;
        o.start();
        g.gain.exponentialRampToValueAtTime(0.25, ctx.currentTime + 0.01);
        g.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime + (duration/1000));
        setTimeout(() => { o.stop(); ctx.close(); }, duration + 20);
      } catch(e){
        // ignore audio failures silently
        console.warn('Audio failed', e);
      }
    }

    function wordBoundaryRegex(keyword) {
      // create regex to match whole word, case-insensitive, allow diacritics roughly via \w is limited but acceptable for basic check
      // We'll escape keyword
      const esc = keyword.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
      return new RegExp('\\b' + esc + '\\b', 'iu');
    }

    // ----- Persistence -----
    function loadProgress() {
      const raw = localStorage.getItem(STORAGE_KEY);
      if (!raw) {
        // create initial
        const init = {
          version: 1,
          user: USER,
          xp: 0,
          rank: 'Poupador',
          currentLevel: 0,
          completed: {},
          lastUpdated: new Date().toISOString()
        };
        saveProgress(init);
        return init;
      }
      try {
        return JSON.parse(raw);
      } catch(e) {
        localStorage.removeItem(STORAGE_KEY);
        return loadProgress();
      }
    }

    function saveProgress(prog) {
      prog.lastUpdated = new Date().toISOString();
      localStorage.setItem(STORAGE_KEY, JSON.stringify(prog));
    }

    function resetProgress() {
      if (!confirm('Apagar todo o histórico e recomeçar do zero?')) return;
      localStorage.removeItem(STORAGE_KEY);
      location.reload();
    }

    // ----- App State -----
    const appState = {
      soundsEnabled: true,
      progress: loadProgress(),
      levelIndex: 0,
      // will hold shuffled alternatives per question for the active level
      activeLevelShuffles: {}, // { questionId: { alternatives: [...], correctShuffledIndex: n } }
      currentQuestionIdx: 0
    };

    // ----- Ranking Helpers -----
    function updateRankFromXp(xp) {
      for (const r of RANKS) {
        if (xp >= r.min && xp <= r.max) return r.name;
      }
      return RANKS[0].name;
    }

    function xpToNextRank(xp) {
      const current = updateRankFromXp(xp);
      const r = RANKS.find(r=>r.name===current);
      if (r.name === 'Shark') return 0;
      const next = RANKS.find(rr => rr.min > r.min);
      return Math.max(0, next.min - xp);
    }

    // ----- UI Rendering -----
    const cardEl = document.getElementById('card');
    const questionArea = document.getElementById('questionArea');
    const xpFill = document.getElementById('xpFill');
    const xpLabel = document.getElementById('xpLabel');
    const xpToNextEl = document.getElementById('xpToNext');
    const rankLabel = document.getElementById('rankLabel');
    const levelLabel = document.getElementById('levelLabel');
    const userLine = document.getElementById('userLine');

    userLine.textContent = `Usuário: ${USER}`;
    document.getElementById('avatar').textContent = USER.slice(0,2).toUpperCase();

    function renderProgress() {
      const xp = appState.progress.xp || 0;
      xpLabel.textContent = `${xp} XP`;
      const percent = Math.min(100, Math.round((xp % 1000) / 1000 * 100)); // visual rule: per 1000 XP block
      xpFill.style.width = percent + '%';
      rankLabel.textContent = updateRankFromXp(xp);
      xpToNextEl.textContent = xpToNextRank(xp);
    }

    function startLevel(index) {
      appState.levelIndex = index;
      appState.currentQuestionIdx = 0;
      appState.activeLevelShuffles = {};
      // shuffle alternatives for each MCQ in this level and store mapping
      const level = QUIZ[index];
      level.forEach(q => {
        if (q.type === 'mcq') {
          const items = q.alternatives.map((t, i)=>({text:t, origIndex:i}));
          const shuffled = shuffleArray(items);
          const correctOrigIndex = q.answerIndex;
          const correctShuffledIndex = shuffled.findIndex(s=>s.origIndex===correctOrigIndex);
          appState.activeLevelShuffles[q.id] = {
            alternatives: shuffled.map(s=>s.text),
            correctShuffledIndex
          };
        }
      });
      renderQuestion();
      document.getElementById('logCard').style.display = 'none';
      updateLevelButtons();
    }

    function updateLevelButtons(){
      document.querySelectorAll('.level-btn').forEach(btn=>{
        const li = Number(btn.getAttribute('data-level'));
        if (li === appState.levelIndex) btn.classList.add('active'); else btn.classList.remove('active');
      });
      const levelNames = ['Nível 1 — Renda Fixa','Nível 2 — Renda Variável','Nível 3 — Avançado / Tesouro'];
      levelLabel.textContent = levelNames[appState.levelIndex] || `Nível ${appState.levelIndex+1}`;
    }

    function renderQuestion() {
      const level = QUIZ[appState.levelIndex];
      const q = level[appState.currentQuestionIdx];
      if (!q) {
        showLevelSummary();
        return;
      }

      // build HTML
      const wrapper = document.createElement('div');
      wrapper.innerHTML = `
        <div class="meta">
          <div class="muted">Pergunta ${appState.currentQuestionIdx + 1} de ${level.length}</div>
        </div>
        <div class="question">${escapeHtml(q.question)}</div>
        <div class="answers" id="answers"></div>
        <div class="feedback" id="feedback"></div>
        <div class="explanation" id="explanation"></div>
        <div class="links" id="linksWrap"></div>
        <div class="controls">
          <div>
            <button id="prevQ" class="level-btn">Anterior</button>
            <button id="nextQ" class="level-btn">Próxima</button>
          </div>
          <div class="small muted">XP desta pergunta: <span id="qXp">${q.xp || 0}</span></div>
        </div>
      `;

      questionArea.innerHTML = '';
      questionArea.appendChild(wrapper);

      // render alternatives or input for open
      const answersEl = document.getElementById('answers');
      const explanationEl = document.getElementById('explanation');
      const feedbackEl = document.getElementById('feedback');
      const linksWrap = document.getElementById('linksWrap');

      feedbackEl.style.display = 'none';
      explanationEl.style.display = 'none';
      linksWrap.innerHTML = '';

      if (q.type === 'mcq') {
        const shuffleInfo = appState.activeLevelShuffles[q.id];
        const alts = shuffleInfo ? shuffleInfo.alternatives : q.alternatives;
        alts.forEach((txt, idx) => {
          const btn = document.createElement('button');
          btn.className = 'btn';
          btn.innerHTML = `<span>${String.fromCharCode(65 + idx)}.</span> <div style="flex:1">${escapeHtml(txt)}</div>`;
          btn.onclick = () => handleMcqAnswer(q, idx);
          // disable if already answered
          const completedForLevel = (appState.progress.completed[`level${appState.levelIndex+1}`] || {});
          if (completedForLevel[q.id] && completedForLevel[q.id] === true) {
            btn.disabled = true;
          }
          answersEl.appendChild(btn);
        });
      } else if (q.type === 'open') {
        const ta = document.createElement('textarea');
        ta.style.width = '100%';
        ta.style.minHeight = '120px';
        ta.style.padding = '10px';
        ta.style.borderRadius = '8px';
        ta.style.background = 'transparent';
        ta.style.color = 'var(--text)';
        ta.style.border = '1px solid rgba(255,255,255,0.04)';
        ta.placeholder = 'Escreva sua resposta (até 6 linhas). Use termos técnicos.';
        answersEl.appendChild(ta);

        const send = document.createElement('button');
        send.className = 'btn';
        send.textContent = 'Enviar resposta';
        send.style.marginTop = '8px';
        send.onclick = () => handleOpenAnswer(q, ta.value);
        answersEl.appendChild(send);

        // If already approved, show status
        const completedForLevel = (appState.progress.completed[`level${appState.levelIndex+1}`] || {});
        if (completedForLevel[q.id] && completedForLevel[q.id].approved) {
          const done = document.createElement('div');
          done.className = 'muted';
          done.textContent = 'Questão dissertativa aprovada previamente.';
          answersEl.appendChild(done);
        }
      }

      // Links
      if (q.links) {
        q.links.forEach(l=>{
          const a = document.createElement('a');
          a.href = l.url;
          a.target = '_blank';
          a.rel = 'noopener';
          a.textContent = l.title;
          linksWrap.appendChild(a);
        });
      } else {
        // generic links area
        linksWrap.innerHTML = `
          <a href="https://www.b3.com.br" target="_blank">B3</a>
          <a href="https://www.tesourodireto.com.br" target="_blank">Tesouro Direto</a>
          <a href="https://www.fgc.org.br" target="_blank">FGC</a>
        `;
      }

      // Prev/Next click
      document.getElementById('prevQ').onclick = () => {
        if (appState.currentQuestionIdx > 0) {
          appState.currentQuestionIdx--;
          renderQuestion();
        }
      };
      document.getElementById('nextQ').onclick = () => {
        if (appState.currentQuestionIdx < QUIZ[appState.levelIndex].length - 1) {
          appState.currentQuestionIdx++;
          renderQuestion();
        } else {
          showLevelSummary();
        }
      };
    }

    function showFeedback(message, ok=true) {
      const feedbackEl = document.getElementById('feedback');
      feedbackEl.textContent = message;
      feedbackEl.classList.remove('err');
      if (!ok) feedbackEl.classList.add('err');
      feedbackEl.style.display = 'block';
    }

    function showExplanation(q) {
      const ex = document.getElementById('explanation');
      if (!ex) return;
      if (q.type === 'mcq') {
        ex.innerHTML = `<strong>Explicação técnica</strong><div style="margin-top:8px">${escapeHtml(q.explanation)}</div>
                        <div style="margin-top:8px"><strong>Resumo prático</strong><div style="margin-top:6px;color:var(--muted)">${escapeHtml(q.summary)}</div></div>`;
      } else {
        ex.innerHTML = `<strong>Detalhes</strong><div style="margin-top:8px;color:var(--muted)">Sua resposta foi analisada quanto ao uso de termos técnicos essenciais. Caso aprovado, XP foi adicionado ao seu progresso.</div>`;
      }
      ex.style.display = 'block';
    }

    function handleMcqAnswer(q, chosenShuffledIdx) {
      // check internal mapping
      const shuffleInfo = appState.activeLevelShuffles[q.id] || { correctShuffledIndex: q.answerIndex };
      const isCorrect = (chosenShuffledIdx === shuffleInfo.correctShuffledIndex);
      const xpPotential = q.xp || 0;
      let xpChange = 0;

      // Update progress store
      const levelKey = `level${appState.levelIndex+1}`;
      if (!appState.progress.completed[levelKey]) appState.progress.completed[levelKey] = {};
      // avoid double-scoring
      if (appState.progress.completed[levelKey][q.id] === true) {
        // already answered earlier
        showFeedback('Questão já respondida anteriormente.', true);
        showExplanation(q);
        saveProgress(appState.progress);
        return;
      }

      if (isCorrect) {
        xpChange = xpPotential;
        appState.progress.xp = (appState.progress.xp || 0) + xpChange;
        showFeedback(`Acerto! +${xpChange} XP`, true);
        beep(880, 120);
      } else {
        // Penalty optional: lose 10% of potential XP (rounded)
        const penalty = Math.round(xpPotential * 0.10);
        xpChange = -penalty;
        appState.progress.xp = Math.max(0, (appState.progress.xp || 0) + xpChange);
        showFeedback(`Resposta incorreta. ${penalty} XP deduzido.`, false);
        beep(220, 300);
      }

      appState.progress.completed[levelKey][q.id] = isCorrect;
      // save
      appState.progress.rank = updateRankFromXp(appState.progress.xp);
      saveProgress(appState.progress);
      renderProgress();
      showExplanation(q);

      // disable buttons for this question
      document.querySelectorAll('.answers .btn').forEach(b => b.disabled = true);
    }

    function handleOpenAnswer(q, text) {
      const required = q.requiredKeywords || [];
      const minRequired = q.minKeywordsRequired || Math.max(1, Math.floor(required.length * 0.75));
      // count matches of whole words (case-insensitive)
      const found = [];
      required.forEach(k=>{
        const re = wordBoundaryRegex(k);
        if (re.test(text || '')) found.push(k);
      });
      const approved = found.length >= minRequired;
      const levelKey = `level${appState.levelIndex+1}`;
      if (!appState.progress.completed[levelKey]) appState.progress.completed[levelKey] = {};

      if (approved) {
        const xpGain = q.xp || 0;
        // extra bonus if all keywords present
        const bonus = (found.length === required.length) ? Math.round(xpGain * 0.30) : 0;
        const total = xpGain + bonus;
        appState.progress.xp = (appState.progress.xp || 0) + total;
        appState.progress.completed[levelKey][q.id] = { approved: true, keywordsFound: found.length, found };
        showFeedback(`Dissertativa aprovada! +${total} XP (${xpGain} base ${bonus ? '+'+bonus+' bônus' : ''})`, true);
        beep(880, 120);
      } else {
        appState.progress.completed[levelKey][q.id] = { approved: false, keywordsFound: found.length, found };
        showFeedback(`Dissertativa não aprovada. Termos encontrados: ${found.length}/${required.length}`, false);
        // no XP if not approved
        beep(220, 300);
      }

      appState.progress.rank = updateRankFromXp(appState.progress.xp);
      saveProgress(appState.progress);
      renderProgress();
      showExplanation(q);
    }

    function showLevelSummary() {
      const lv = appState.levelIndex;
      const levelKey = `level${lv+1}`;
      const completed = appState.progress.completed[levelKey] || {};
      const levelData = QUIZ[lv];
      const total = levelData.length;
      let correctCount = 0;
      let dissertApproved = 0;
      levelData.forEach(q=>{
        const res = completed[q.id];
        if (!res) return;
        if (q.type === 'mcq' && res === true) correctCount++;
        if (q.type === 'open' && res.approved) dissertApproved++;
      });

      const content = document.getElementById('logContent');
      content.innerHTML = `
        <div>Progresso do ${levelKey}:</div>
        <ul>
          <li>Acertos (MCQ): ${correctCount} / ${levelData.filter(q=>q.type==='mcq').length}</li>
          <li>Dissertativa aprovada: ${dissertApproved} / ${levelData.filter(q=>q.type==='open').length}</li>
          <li>XP total: ${appState.progress.xp}</li>
          <li>Rank atual: ${appState.progress.rank}</li>
        </ul>
      `;
      document.getElementById('logCard').style.display = 'block';
    }

    // ----- Helpers -----
    function escapeHtml(s) {
      if (!s) return '';
      return s.replace(/[&<>"]/g, function(c) {
        return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c];
      });
    }

    // ----- Init & Event wiring -----
    document.getElementById('soundToggle').onclick = () => {
      appState.soundsEnabled = !appState.soundsEnabled;
      document.getElementById('soundToggle').textContent = appState.soundsEnabled ? '🔊' : '🔇';
    };
    document.getElementById('resetBtn').onclick = resetProgress;
    document.getElementById('startLevelBtn').onclick = () => startLevel(appState.levelIndex);

    document.querySelectorAll('.level-btn[data-level]').forEach(btn=>{
      btn.onclick = () => {
        const li = Number(btn.getAttribute('data-level'));
        startLevel(li);
      };
    });

    // load UI with saved progress and start first level
    function boot() {
      renderProgress();
      updateLevelButtons();
      // Initialize active level shuffles for current level
      startLevel(appState.progress.currentLevel || 0);
      // apply progress.currentLevel value to state so user returns there
      appState.levelIndex = appState.progress.currentLevel || 0;
      updateLevelButtons();
      renderQuestion();
    }

    // Save current level index when switching
    window.addEventListener('beforeunload', () => {
      appState.progress.currentLevel = appState.levelIndex;
      saveProgress(appState.progress);
    });

    // Start
    boot();

    // Expose a small API for debugging in console if needed:
    window.eduApp = {
      state: appState,
      QUIZ,
      saveProgress,
      loadProgress,
      resetProgress
    };

  </script>
</body>
</html>
```
