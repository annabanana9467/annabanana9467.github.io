

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Where in the World? 🌍</title>
  <style>
    /* ══════════════════════════════════════════════
       DESIGN TOKENS
    ══════════════════════════════════════════════ */
    :root {
      --brand-red:     #d64000;
      --brand-dark:    #123123;
      --red-hover:     #b83500;
      --dark-hover:    #0c2119;
      --light-red:     #ff8c55;
      --pale-red:      #fff0e8;
      --pale-green:    #e6f4ec;
      --mid-green:     #1e5c3a;
      --soft-border:   #b8ddc8;
      --muted-text:    #5a7a65;
      --body-bg-a:     #0b1e14;
      --body-bg-b:     #123123;
      --body-bg-c:     #1a3d28;
      --white:         #ffffff;
      --text-dark:     #0d2218;
      --curtain-a:     #123123;
      --curtain-b:     #1e5c3a;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(155deg,
        var(--body-bg-a) 0%,
        var(--body-bg-b) 50%,
        var(--body-bg-c) 100%);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 28px 16px 50px;
      color: var(--white);
    }

    /* ── HEADER ─────────────────────────────────── */
    .site-header {
      text-align: center;
      margin-bottom: 26px;
    }
    .site-header h1 {
      font-size: clamp(1.9rem, 5.5vw, 3rem);
      color: var(--light-red);
      text-shadow: 0 3px 14px rgba(0,0,0,.6);
      letter-spacing: 2px;
      line-height: 1.2;
    }
    .site-header .sub {
      margin-top: 7px;
      font-size: 1rem;
      color: #a8d4bc;
      letter-spacing: .4px;
    }
    .pill-tag {
      display: inline-block;
      margin-top: 11px;
      background: var(--brand-red);
      color: var(--white);
      font-size: .8rem;
      font-weight: 800;
      letter-spacing: 1.6px;
      text-transform: uppercase;
      padding: 5px 20px;
      border-radius: 30px;
      box-shadow: 0 3px 12px rgba(214,64,0,.4);
    }

    /* ── SCORE / PROGRESS ───────────────────────── */
    .meta-row {
      display: flex;
      gap: 32px;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      margin-bottom: 14px;
      font-size: .95rem;
      color: #b0d8c0;
    }
    .meta-row strong { color: var(--light-red); font-size: 1.18rem; }

    .progress-dots {
      display: flex;
      gap: 10px;
      justify-content: center;
      margin-bottom: 18px;
    }
    .dot {
      width: 13px; height: 13px;
      border-radius: 50%;
      background: #2a4d38;
      transition: background .35s, transform .25s;
    }
    .dot.done    { background: var(--brand-red); }
    .dot.current {
      background: var(--light-red);
      transform: scale(1.35);
      box-shadow: 0 0 0 3px rgba(255,140,85,.3);
    }

    /* ── ROUND NAV ──────────────────────────────── */
    .round-nav {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: center;
      margin-bottom: 24px;
    }
    .round-btn {
      background: transparent;
      border: 2px solid var(--light-red);
      color: var(--light-red);
      padding: 7px 20px;
      border-radius: 30px;
      cursor: pointer;
      font-size: .88rem;
      font-weight: 700;
      letter-spacing: .5px;
      transition: all .22s;
    }
    .round-btn:hover  { background: rgba(255,140,85,.15); }
    .round-btn.active {
      background: var(--brand-red);
      color: var(--white);
      border-color: var(--brand-red);
      box-shadow: 0 4px 14px rgba(214,64,0,.45);
    }

    /* ── GAME CARD ──────────────────────────────── */
    .game-card {
      background: var(--white);
      color: var(--text-dark);
      border-radius: 22px;
      box-shadow: 0 20px 70px rgba(0,0,0,.55);
      max-width: 700px;
      width: 100%;
      overflow: hidden;
    }

    .card-header {
      background: var(--brand-red);
      color: var(--white);
      text-align: center;
      padding: 14px 22px;
      font-size: 1.05rem;
      font-weight: 800;
      letter-spacing: 1px;
      text-transform: uppercase;
    }

    /* ── IMAGE REVEAL ───────────────────────────── */
    .image-wrapper {
      position: relative;
      width: 100%;
      height: 370px;
      overflow: hidden;
      background: #0b1e14;
      cursor: pointer;
      user-select: none;
      -webkit-user-select: none;
    }

    /* The actual landmark photo */
    .image-wrapper img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
      /* Always visible underneath — curtains slide away */
    }

    /* Two curtain panels that slide away on click */
    .curtain {
      position: absolute;
      left: 0;
      width: 100%;
      height: 50%;
      z-index: 3;
      transition: transform .9s cubic-bezier(.77, 0, .18, 1);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }
    /* Subtle texture on curtains */
    .curtain::before {
      content: '';
      position: absolute;
      inset: 0;
      background: repeating-linear-gradient(
        135deg,
        transparent,
        transparent 18px,
        rgba(255,255,255,.03) 18px,
        rgba(255,255,255,.03) 36px
      );
    }
    .curtain::after {
      content: '🌍';
      font-size: 3rem;
      opacity: .18;
      position: relative;
      z-index: 1;
    }

    .curtain-top {
      top: 0;
      background: linear-gradient(180deg, var(--curtain-a), var(--curtain-b));
      transform: translateY(0);
    }
    .curtain-bottom {
      bottom: 0;
      background: linear-gradient(0deg, var(--curtain-a), var(--curtain-b));
      transform: translateY(0);
    }

    /* STEP 1 — slide top curtain up (reveals top half) */
    .reveal-half .curtain-top {
      transform: translateY(-100%);
    }

    /* STEP 2 — slide bottom curtain down (reveals full image) */
    .reveal-full .curtain-top    { transform: translateY(-100%); }
    .reveal-full .curtain-bottom { transform: translateY(100%); }

    /* Click-to-reveal badge */
    .click-hint {
      position: absolute;
      bottom: 14px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(214, 64, 0, .92);
      color: var(--white);
      padding: 7px 22px;
      border-radius: 30px;
      font-size: .82rem;
      font-weight: 700;
      z-index: 5;
      pointer-events: none;
      white-space: nowrap;
      box-shadow: 0 3px 14px rgba(0,0,0,.35);
      transition: opacity .45s;
    }
    .reveal-full .click-hint { opacity: 0; }

    /* Two small progress pips on the image */
    .img-pips {
      position: absolute;
      top: 12px;
      right: 14px;
      display: flex;
      gap: 6px;
      z-index: 5;
    }
    .img-pip {
      width: 10px; height: 10px;
      border-radius: 50%;
      background: rgba(255,255,255,.28);
      transition: background .3s;
    }
    .img-pip.lit { background: var(--light-red); }

    /* Loading spinner shown before image loads */
    .img-loader {
      position: absolute;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 2;
      pointer-events: none;
    }
    .spinner {
      width: 44px; height: 44px;
      border: 4px solid rgba(255,255,255,.15);
      border-top-color: var(--light-red);
      border-radius: 50%;
      animation: spin .8s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }
    .img-loader.hidden { display: none; }

    /* ── CHOICES ────────────────────────────────── */
    .choices-area {
      padding: 20px 22px 4px;
    }
    .choices-label {
      text-align: center;
      font-size: 1rem;
      color: var(--brand-dark);
      margin-bottom: 14px;
      font-weight: 700;
    }
    .choices-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }
    .choice-btn {
      background: var(--pale-green);
      border: 2px solid var(--soft-border);
      color: var(--text-dark);
      padding: 15px 10px;
      border-radius: 14px;
      font-size: .97rem;
      font-weight: 700;
      cursor: pointer;
      transition: all .22s;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      min-height: 58px;
      text-align: center;
      line-height: 1.3;
    }
    .choice-btn:hover:not(:disabled) {
      background: var(--mid-green);
      color: var(--white);
      border-color: var(--mid-green);
      transform: translateY(-2px);
      box-shadow: 0 5px 18px rgba(18,49,35,.28);
    }
    .choice-btn.correct {
      background: #1e8c4a;
      border-color: #166337;
      color: var(--white);
      animation: pop .4s ease;
    }
    .choice-btn.wrong {
      background: #d63b3b;
      border-color: #a82b2b;
      color: var(--white);
    }
    .choice-btn:disabled { cursor: default; }

    @keyframes pop {
      0%   { transform: scale(1); }
      45%  { transform: scale(1.07); }
      100% { transform: scale(1); }
    }

    /* ── RESULT + CAPTION ───────────────────────── */
    .result-msg {
      text-align: center;
      margin: 14px 0 0;
      font-size: 1.05rem;
      font-weight: 700;
      min-height: 26px;
      padding: 0 10px;
    }
    .result-msg.correct { color: #1a7a40; }
    .result-msg.wrong   { color: var(--brand-red); }

    .landmark-caption {
      text-align: center;
      padding: 8px 20px 0;
      font-size: .82rem;
      color: var(--muted-text);
      font-style: italic;
      min-height: 22px;
    }

    /* ── CARD FOOTER ────────────────────────────── */
    .card-footer {
      padding: 16px 22px 22px;
      display: flex;
      justify-content: center;
      gap: 12px;
      flex-wrap: wrap;
    }
    .action-btn {
      border: none;
      padding: 11px 28px;
      border-radius: 30px;
      font-size: .95rem;
      font-weight: 800;
      cursor: pointer;
      transition: all .22s;
      letter-spacing: .4px;
    }
    .btn-primary {
      background: var(--brand-red);
      color: var(--white);
      box-shadow: 0 5px 18px rgba(214,64,0,.38);
    }
    .btn-primary:hover { background: var(--red-hover); transform: translateY(-2px); }

    .btn-secondary {
      background: var(--pale-green);
      color: var(--brand-dark);
      border: 2px solid var(--soft-border);
    }
    .btn-secondary:hover { background: var(--soft-border); transform: translateY(-2px); }

    /* ── FINAL SCREEN ───────────────────────────── */
    .final-screen {
      padding: 46px 28px 40px;
      text-align: center;
    }
    .final-screen .trophy { font-size: 5rem; display: block; margin-bottom: 16px; }
    .final-screen h2 { color: var(--brand-red); font-size: 1.9rem; margin-bottom: 10px; }
    .final-screen .verdict { color: var(--brand-dark); font-size: 1.05rem; margin-bottom: 8px; }
    .final-screen .recap { color: var(--muted-text); font-size: .88rem; margin-bottom: 28px; line-height: 1.7; }

    /* ── SITE FOOTER ────────────────────────────── */
    .site-footer {
      margin-top: 30px;
      text-align: center;
      font-size: .76rem;
      color: #6a9e80;
      line-height: 1.8;
      max-width: 640px;
    }
    .site-footer a { color: #8ec8a4; }

    /* ── RESPONSIVE ─────────────────────────────── */
    @media (max-width: 480px) {
      .choices-grid  { grid-template-columns: 1fr; }
      .image-wrapper { height: 260px; }
      .curtain::after { font-size: 2rem; }
    }
  </style>
</head>
<body>

<!-- ═══════════════════════ HEADER ═══════════════════════ -->
<header class="site-header">
  <h1>🌍 Where in the World?</h1>
  <p class="sub">Icebreaker Game — Guess the landmark, name the country!</p>
  <span class="pill-tag">✈️ 4 Rounds · 4 Destinations</span>
</header>

<!-- ═══════════════════════ SCORE ROW ════════════════════ -->
<div class="meta-row">
  <span>Round <strong id="roundDisplay">1</strong> of 4</span>
  <span>Score <strong id="scoreDisplay">0</strong> / 4</span>
</div>

<!-- ═══════════════════════ PROGRESS DOTS ════════════════ -->
<div class="progress-dots" id="progressDots"></div>

<!-- ═══════════════════════ ROUND NAV ════════════════════ -->
<nav class="round-nav" id="roundNav" aria-label="Select round"></nav>

<!-- ═══════════════════════ GAME CARD ════════════════════ -->
<main class="game-card" id="gameCard">

  <div class="card-header" id="cardHeader">🔍 Round 1 — Where is this place?</div>

  <!-- IMAGE REVEAL ZONE -->
  <div class="image-wrapper" id="imageWrapper"
       role="button" tabindex="0"
       aria-label="Click to reveal the landmark image"
       onclick="revealStep()">

    <!-- Landmark photo — always loaded, curtains hide it -->
    <img id="landmarkImg" src="" alt="Mystery landmark" />

    <!-- Loading spinner -->
    <div class="img-loader" id="imgLoader">
      <div class="spinner"></div>
    </div>

    <!-- Curtains -->
    <div class="curtain curtain-top"    id="curtainTop"></div>
    <div class="curtain curtain-bottom" id="curtainBottom"></div>

    <!-- Hint badge -->
    <div class="click-hint" id="clickHint">👆 Click to reveal the top half!</div>

    <!-- Step pips -->
    <div class="img-pips">
      <div class="img-pip" id="pip0"></div>
      <div class="img-pip" id="pip1"></div>
    </div>

  </div><!-- /image-wrapper -->

  <!-- CHOICES -->
  <section class="choices-area">
    <p class="choices-label">🗺️ Which country does this landmark belong to?</p>
    <div class="choices-grid" id="choicesGrid"></div>
    <p class="result-msg" id="resultMsg" role="alert"></p>
  </section>

  <p class="landmark-caption" id="landmarkCaption"></p>

  <!-- FOOTER BUTTONS -->
  <div class="card-footer">
    <button class="action-btn btn-secondary" onclick="resetRound()">🔄 Reset Round</button>
    <button class="action-btn btn-primary" id="nextBtn"
            onclick="nextRound()" style="display:none;">Next Round ➡️</button>
  </div>

</main><!-- /game-card -->

<!-- ═══════════════════════ SITE FOOTER ══════════════════ -->
<footer class="site-footer">
  Images from <a href="https://commons.wikimedia.org" target="_blank" rel="noopener">Wikimedia Commons</a>
  — Creative Commons / Public Domain licences.<br/>
  🎮 <em>"Where in the World?"</em> Icebreaker &nbsp;|&nbsp; GitHub Pages ready
</footer>


<!-- ═══════════════════════════════════════════════════════
     GAME LOGIC
════════════════════════════════════════════════════════ -->
<script>
/* ─────────────────────────────────────────────────────────
   ROUND DATA
   Direct Wikimedia Commons upload URLs (no API, no redirect)
──────────────────────────────────────────────────────────── */
const rounds = [
  {
    country : "Philippines",
    landmark: "Siquijor Island, Philippines",
    hint    : "A mystical island in the Central Visayas known for pristine beaches and local folklore",
    emoji   : "🇵🇭",
    /* Paliton Beach, Siquijor — CC BY-SA 3.0 — Checawey */
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a6/Paliton_Beach%2C_Siquijor%2C_Philippines.JPG/1280px-Paliton_Beach%2C_Siquijor%2C_Philippines.JPG",
    choices : ["Vietnam", "Philippines", "Indonesia", "Malaysia"]
  },
  {
    country : "India",
    landmark: "Majuli Island, Assam, India",
    hint    : "The world's largest river island, sitting in the mighty Brahmaputra River",
    emoji   : "🇮🇳",
    /* Ferry connecting Majuli Island — Wikimedia Commons */
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Ferry_service_connecting_Majuli_island.jpg/1280px-Ferry_service_connecting_Majuli_island.jpg",
    choices : ["Bangladesh", "Myanmar", "India", "Nepal"]
  },
  {
    country : "Peru",
    landmark: "Choquequirao, Cusco Region, Peru",
    hint    : "A partly excavated Inca 'lost city' deep in the Andes — rarer than Machu Picchu!",
    emoji   : "🇵🇪",
    /* Choquequirao Plaza — CC BY-SA — Wikimedia Commons */
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Choquequirao_Plaza.JPG/1280px-Choquequirao_Plaza.JPG",
    choices : ["Bolivia", "Colombia", "Peru", "Ecuador"]
  },
  {
    country : "USA",
    landmark: "North Cascades National Park, Washington, USA",
    hint    : "Called the 'American Alps' — a rugged wilderness of glaciers and turquoise lakes",
    emoji   : "🇺🇸",
    /* Diablo Lake, North Cascades — Wikimedia Commons */
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Diablo_lake_north_cascades.jpg/1280px-Diablo_lake_north_cascades.jpg",
    choices : ["Canada", "Norway", "USA", "New Zealand"]
  }
];

/* ─────────────────────────────────────────────────────────
   STATE
──────────────────────────────────────────────────────────── */
let currentRound = 0;
let score        = 0;
let revealState  = 0;   // 0 = hidden | 1 = top half shown | 2 = full
let answered     = false;

/* ─────────────────────────────────────────────────────────
   BUILD NAV + PROGRESS DOTS
──────────────────────────────────────────────────────────── */
function buildNav() {
  const nav  = document.getElementById('roundNav');
  const dots = document.getElementById('progressDots');
  nav.innerHTML  = '';
  dots.innerHTML = '';

  rounds.forEach((r, i) => {
    /* Nav button — shows only emoji + round number, NO country name */
    const btn = document.createElement('button');
    btn.className = 'round-btn' + (i === currentRound ? ' active' : '');
    btn.textContent = r.emoji + '  Round ' + (i + 1);
    btn.setAttribute('aria-label', 'Go to round ' + (i + 1));
    btn.onclick = () => { currentRound = i; loadRound(); };
    nav.appendChild(btn);

    /* Progress dot */
    const dot = document.createElement('div');
    dot.className = 'dot'
      + (i < currentRound   ? ' done'    : '')
      + (i === currentRound ? ' current' : '');
    dots.appendChild(dot);
  });
}

/* ─────────────────────────────────────────────────────────
   LOAD ROUND
──────────────────────────────────────────────────────────── */
function loadRound() {
  revealState = 0;
  answered    = false;

  const r = rounds[currentRound];

  /* Header + meta */
  document.getElementById('cardHeader').textContent =
    '🔍 Round ' + (currentRound + 1) + ' — Where is this place?';
  document.getElementById('roundDisplay').textContent = currentRound + 1;
  document.getElementById('scoreDisplay').textContent = score;

  /* Reset curtains */
  const wrapper = document.getElementById('imageWrapper');
  wrapper.classList.remove('reveal-half', 'reveal-full');

  /* Show spinner, load image */
  const img    = document.getElementById('landmarkImg');
  const loader = document.getElementById('imgLoader');
  loader.classList.remove('hidden');
  img.src = '';                       // force reload
  img.alt = 'Mystery landmark';

  img.onload  = () => loader.classList.add('hidden');
  img.onerror = () => {
    loader.classList.add('hidden');
    img.alt = '⚠️ Image could not be loaded. Check your connection.';
  };
  img.src = r.image;

  /* Reset hint */
  const hint = document.getElementById('clickHint');
  hint.textContent = '👆 Click to reveal the top half!';
  hint.style.opacity = '1';

  /* Reset pips */
  document.getElementById('pip0').classList.remove('lit');
  document.getElementById('pip1').classList.remove('lit');

  /* Clear result + caption */
  const msg = document.getElementById('resultMsg');
  msg.textContent = '';
  msg.className   = 'result-msg';
  document.getElementById('landmarkCaption').textContent = '';

  /* Hide next button */
  const nextBtn = document.getElementById('nextBtn');
  nextBtn.style.display = 'none';
  nextBtn.textContent   = 'Next Round ➡️';
  nextBtn.onclick       = nextRound;

  /* Build shuffled choices — only country names, no emojis */
  const grid = document.getElementById('choicesGrid');
  grid.innerHTML = '';
  const shuffled = [...r.choices].sort(() => Math.random() - .5);
  shuffled.forEach(choice => {
    const btn = document.createElement('button');
    btn.className   = 'choice-btn';
    btn.textContent = choice;
    btn.onclick     = () => checkAnswer(choice, btn);
    grid.appendChild(btn);
  });

  buildNav();
}

/* ─────────────────────────────────────────────────────────
   REVEAL STEPS (click on image area)
──────────────────────────────────────────────────────────── */
function revealStep() {
  if (answered) return;

  if (revealState === 0) {
    /* Step 1: slide top curtain up → top half visible */
    document.getElementById('imageWrapper').classList.add('reveal-half');
    document.getElementById('clickHint').textContent = '👆 Click again for the full picture!';
    document.getElementById('pip0').classList.add('lit');
    revealState = 1;

  } else if (revealState === 1) {
    /* Step 2: slide bottom curtain down → full image visible */
    document.getElementById('imageWrapper').classList.add('reveal-full');
    document.getElementById('pip1').classList.add('lit');
    revealState = 2;
  }
  /* After step 2, further clicks do nothing until next round */
}

/* ─────────────────────────────────────────────────────────
   CHECK ANSWER
──────────────────────────────────────────────────────────── */
function checkAnswer(choice, clickedBtn) {
  if (answered) return;
  answered = true;

  /* Force full reveal so audience sees the landmark */
  const wrapper = document.getElementById('imageWrapper');
  wrapper.classList.add('reveal-half', 'reveal-full');
  document.getElementById('pip0').classList.add('lit');
  document.getElementById('pip1').classList.add('lit');
  revealState = 2;

  const r       = rounds[currentRound];
  const allBtns = document.querySelectorAll('.choice-btn');
  const msg     = document.getElementById('resultMsg');

  /* Disable all, highlight correct answer */
  allBtns.forEach(b => {
    b.disabled = true;
    if (b.textContent === r.country) b.classList.add('correct');
  });

  if (choice === r.country) {
    clickedBtn.classList.add('correct');
    msg.textContent = '✅ Correct! That\'s ' + r.landmark + '! 🎉';
    msg.className   = 'result-msg correct';
    score++;
    document.getElementById('scoreDisplay').textContent = score;
  } else {
    clickedBtn.classList.add('wrong');
    msg.textContent = '❌ Not quite! It\'s ' + r.landmark + '.';
    msg.className   = 'result-msg wrong';
  }

  document.getElementById('landmarkCaption').textContent =
    '📍 ' + r.landmark + ' — ' + r.hint;

  /* Show next / finish button */
  const nextBtn = document.getElementById('nextBtn');
  nextBtn.style.display = 'inline-block';
  if (currentRound === rounds.length - 1) {
    nextBtn.textContent = '🏆 See Final Score';
    nextBtn.onclick     = showFinalScore;
  }
}

/* ─────────────────────────────────────────────────────────
   NAVIGATION
──────────────────────────────────────────────────────────── */
function nextRound() {
  currentRound++;
  loadRound();
}

function resetRound() {
  loadRound();
}

/* ─────────────────────────────────────────────────────────
   FINAL SCORE SCREEN
──────────────────────────────────────────────────────────── */
function showFinalScore() {
  const icons = ['😅', '🥉', '🥉', '🥈', '🥇'];
  const msgs  = [
    "Keep exploring — every journey starts with curiosity! 🌱",
    "One more trip around the globe and you'll nail it! ✈️",
    "Not bad at all! You've got a traveller's instinct! 🗺️",
    "Great job! You really know your world landmarks! 🌟",
    "Perfect score! You are a certified World Explorer! 🌍"
  ];

  document.getElementById('gameCard').innerHTML = `
    <div class="card-header" style="background:var(--brand-red);">🏆 Final Results</div>
    <div class="final-screen">
      <span class="trophy">${icons[score]}</span>
      <h2>You scored ${score} out of ${rounds.length}!</h2>
      <p class="verdict">${msgs[score]}</p>
      <p class="recap">
        🇵🇭 Siquijor Island, Philippines<br/>
        🇮🇳 Majuli Island, Assam, India<br/>
        🇵🇪 Choquequirao, Cusco Region, Peru<br/>
        🇺🇸 North Cascades National Park, USA
      </p>
      <button class="action-btn btn-primary" onclick="restartGame()">🔄 Play Again</button>
    </div>
  `;
}

/* ─────────────────────────────────────────────────────────
   RESTART
──────────────────────────────────────────────────────────── */
function restartGame() {
  currentRound = 0;
  score        = 0;

  document.getElementById('gameCard').innerHTML = `
    <div class="card-header" id="cardHeader">🔍 Round 1 — Where is this place?</div>

    <div class="image-wrapper" id="imageWrapper"
         role="button" tabindex="0"
         aria-label="Click to reveal the landmark image"
         onclick="revealStep()">
      <img id="landmarkImg" src="" alt="Mystery landmark" />
      <div class="img-loader" id="imgLoader"><div class="spinner"></div></div>
      <div class="curtain curtain-top"    id="curtainTop"></div>
      <div class="curtain curtain-bottom" id="curtainBottom"></div>
      <div class="click-hint" id="clickHint">👆 Click to reveal the top half!</div>
      <div class="img-pips">
        <div class="img-pip" id="pip0"></div>
        <div class="img-pip" id="pip1"></div>
      </div>
    </div>

    <section class="choices-area">
      <p class="choices-label">🗺️ Which country does this landmark belong to?</p>
      <div class="choices-grid" id="choicesGrid"></div>
      <p class="result-msg" id="resultMsg" role="alert"></p>
    </section>

    <p class="landmark-caption" id="landmarkCaption"></p>

    <div class="card-footer">
      <button class="action-btn btn-secondary" onclick="resetRound()">🔄 Reset Round</button>
      <button class="action-btn btn-primary" id="nextBtn"
              onclick="nextRound()" style="display:none;">Next Round ➡️</button>
    </div>
  `;

  loadRound();
}

/* ─────────────────────────────────────────────────────────
   KEYBOARD — Enter / Space on image wrapper
──────────────────────────────────────────────────────────── */
document.addEventListener('keydown', e => {
  if ((e.key === 'Enter' || e.key === ' ') &&
       document.activeElement &&
       document.activeElement.id === 'imageWrapper') {
    e.preventDefault();
    revealStep();
  }
});

/* ─────────────────────────────────────────────────────────
   INIT
──────────────────────────────────────────────────────────── */
loadRound();
</script>
</body>
</html>
✅ What Was Fixed & Improved
Issue You Raised	What Was Done
Country names showing on top nav	Nav now shows emoji + Round number only — no country names
Images must load on the page	Images load immediately on page open via <img src=""> with a spinner while loading
Use specific Wikipedia/Commons landmarks	All 4 confirmed Wikimedia Commons direct upload URLs used
Reveal: half first, then full on 2nd click	Two curtain panels slide away in sequence — top half first, then bottom
Colors #d64000 and #123123	Applied across headers, buttons, curtains, accents, and backgrounds

🖼️ Confirmed Wikimedia Commons Images Used
Round	Landmark	Wikimedia File
1 🇵🇭	Siquijor Island	Paliton_Beach,_Siquijor,_Philippines.JPG
2 🇮🇳	Majuli Island	Ferry_service_connecting_Majuli_island.jpg
3 🇵🇪	Choquequirao	Choquequirao_Plaza.JPG
4 🇺🇸	North Cascades	Diablo_lake_north_cascades.jpg

🌐 Free Hosting Platforms
Platform	How	Best For
GitHub Pages	Push index.html → Settings → Pages → Deploy from main	Developers & facilitators
Netlify Drop	Drag & drop at app.netlify.com/drop	Instant no-login deploy
Tiiny.host	Upload at tiiny.host — live link in seconds	One-click meeting sharing
Cloudflare Pages	Connect GitHub repo, auto-deploys	Fast global CDN
Itch.io	Upload as HTML game project (free)	Gamified workshops

View Source
Estimated query cost:
$0.2011 USD
–

–

Anna Fae FloresAF
Anna Fae Flores
–
3m ago
make the background as globe image
Claude 4.6 Sonnet
Claude 4.6 Sonnet
–
just now
I have all the confirmed Wikimedia Commons NASA Blue Marble image URLs I need. Here is your fully upgraded index.html with the Earth globe as the background — built using the real NASA Blue Marble photo (Public Domain, NASA) from Wikimedia Commons, layered with a CSS globe effect, floating particles, and a dark space atmosphere:


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Where in the World? 🌍</title>
  <style>
    /* ══════════════════════════════════════════════
       DESIGN TOKENS
    ══════════════════════════════════════════════ */
    :root {
      --brand-red:    #d64000;
      --brand-dark:   #123123;
      --red-hover:    #b83500;
      --light-red:    #ff8c55;
      --pale-red:     #fff0e8;
      --pale-green:   #e6f4ec;
      --mid-green:    #1e5c3a;
      --soft-border:  #b8ddc8;
      --muted-text:   #5a7a65;
      --white:        #ffffff;
      --text-dark:    #0d2218;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    /* ══════════════════════════════════════════════
       FULL-PAGE GLOBE BACKGROUND
    ══════════════════════════════════════════════ */
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 28px 16px 50px;
      color: var(--white);
      position: relative;
      overflow-x: hidden;
    }

    /* Layer 1 — deep space black base */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background: #000510;
      z-index: -3;
    }

    /* Layer 2 — NASA Blue Marble globe image, centred, large */
    .globe-bg {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: min(110vw, 110vh);
      height: min(110vw, 110vh);
      border-radius: 50%;
      background-image: url('https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/The_Earth_seen_from_Apollo_17.jpg/1024px-The_Earth_seen_from_Apollo_17.jpg');
      background-size: cover;
      background-position: center;
      z-index: -2;
      opacity: 0.28;
      /* Slow rotation animation */
      animation: globeSpin 90s linear infinite;
      box-shadow:
        0 0 80px 40px rgba(0, 60, 120, 0.55),
        0 0 160px 80px rgba(0, 30, 80, 0.35),
        inset 0 0 60px 30px rgba(0, 0, 20, 0.6);
    }

    @keyframes globeSpin {
      from { background-position: 0% center; }
      to   { background-position: 100% center; }
    }

    /* Layer 3 — radial dark vignette so edges stay dark */
    .globe-vignette {
      position: fixed;
      inset: 0;
      background: radial-gradient(
        ellipse 70% 70% at 50% 50%,
        transparent 30%,
        rgba(0, 5, 16, 0.55) 60%,
        rgba(0, 5, 16, 0.92) 100%
      );
      z-index: -1;
      pointer-events: none;
    }

    /* Layer 4 — star field (pure CSS dots) */
    .stars {
      position: fixed;
      inset: 0;
      z-index: -1;
      pointer-events: none;
      overflow: hidden;
    }
    .stars span {
      position: absolute;
      display: block;
      border-radius: 50%;
      background: white;
      animation: twinkle var(--dur, 4s) ease-in-out infinite;
      opacity: 0;
    }
    @keyframes twinkle {
      0%, 100% { opacity: 0; transform: scale(1); }
      50%       { opacity: var(--op, .7); transform: scale(1.4); }
    }

    /* ── HEADER ─────────────────────────────────── */
    .site-header {
      text-align: center;
      margin-bottom: 22px;
      position: relative;
      z-index: 1;
    }
    .site-header h1 {
      font-size: clamp(2rem, 6vw, 3.2rem);
      color: var(--light-red);
      text-shadow:
        0 0 30px rgba(255,140,85,.6),
        0 3px 14px rgba(0,0,0,.8);
      letter-spacing: 3px;
      line-height: 1.15;
    }
    .site-header .sub {
      margin-top: 7px;
      font-size: 1rem;
      color: #c0ddd0;
      letter-spacing: .5px;
      text-shadow: 0 1px 6px rgba(0,0,0,.7);
    }
    .pill-tag {
      display: inline-block;
      margin-top: 11px;
      background: var(--brand-red);
      color: var(--white);
      font-size: .8rem;
      font-weight: 800;
      letter-spacing: 1.6px;
      text-transform: uppercase;
      padding: 5px 20px;
      border-radius: 30px;
      box-shadow: 0 3px 16px rgba(214,64,0,.55);
    }

    /* ── SCORE / PROGRESS ───────────────────────── */
    .meta-row {
      display: flex;
      gap: 32px;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      margin-bottom: 14px;
      font-size: .95rem;
      color: #b0d8c0;
      position: relative;
      z-index: 1;
      text-shadow: 0 1px 6px rgba(0,0,0,.7);
    }
    .meta-row strong { color: var(--light-red); font-size: 1.18rem; }

    .progress-dots {
      display: flex;
      gap: 10px;
      justify-content: center;
      margin-bottom: 18px;
      position: relative;
      z-index: 1;
    }
    .dot {
      width: 13px; height: 13px;
      border-radius: 50%;
      background: rgba(255,255,255,.18);
      border: 1px solid rgba(255,255,255,.2);
      transition: background .35s, transform .25s;
    }
    .dot.done    { background: var(--brand-red); border-color: var(--brand-red); }
    .dot.current {
      background: var(--light-red);
      transform: scale(1.35);
      box-shadow: 0 0 0 3px rgba(255,140,85,.35);
      border-color: var(--light-red);
    }

    /* ── ROUND NAV ──────────────────────────────── */
    .round-nav {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: center;
      margin-bottom: 24px;
      position: relative;
      z-index: 1;
    }
    .round-btn {
      background: rgba(18,49,35,.55);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      border: 2px solid var(--light-red);
      color: var(--light-red);
      padding: 7px 20px;
      border-radius: 30px;
      cursor: pointer;
      font-size: .88rem;
      font-weight: 700;
      letter-spacing: .5px;
      transition: all .22s;
    }
    .round-btn:hover  { background: rgba(214,64,0,.3); }
    .round-btn.active {
      background: var(--brand-red);
      color: var(--white);
      border-color: var(--brand-red);
      box-shadow: 0 4px 18px rgba(214,64,0,.55);
    }

    /* ── GAME CARD ──────────────────────────────── */
    .game-card {
      background: rgba(255,255,255,.96);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      color: var(--text-dark);
      border-radius: 22px;
      box-shadow:
        0 20px 70px rgba(0,0,0,.65),
        0 0 0 1px rgba(255,255,255,.15);
      max-width: 700px;
      width: 100%;
      overflow: hidden;
      position: relative;
      z-index: 1;
    }

    .card-header {
      background: var(--brand-red);
      color: var(--white);
      text-align: center;
      padding: 14px 22px;
      font-size: 1.05rem;
      font-weight: 800;
      letter-spacing: 1px;
      text-transform: uppercase;
    }

    /* ── IMAGE REVEAL ───────────────────────────── */
    .image-wrapper {
      position: relative;
      width: 100%;
      height: 370px;
      overflow: hidden;
      background: #0a1a10;
      cursor: pointer;
      user-select: none;
      -webkit-user-select: none;
    }
    .image-wrapper img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
    }

    /* Curtain panels */
    .curtain {
      position: absolute;
      left: 0;
      width: 100%;
      height: 50%;
      z-index: 3;
      transition: transform .9s cubic-bezier(.77,0,.18,1);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }
    .curtain::before {
      content: '';
      position: absolute;
      inset: 0;
      background-image: url('https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/The_Earth_seen_from_Apollo_17.jpg/512px-The_Earth_seen_from_Apollo_17.jpg');
      background-size: 200% 200%;
      background-position: center;
      opacity: 0.22;
    }
    .curtain-top {
      top: 0;
      background: linear-gradient(180deg, #000d06 0%, #123123 100%);
      transform: translateY(0);
    }
    .curtain-top::before  { background-position: center top; }
    .curtain-bottom {
      bottom: 0;
      background: linear-gradient(0deg, #000d06 0%, #123123 100%);
      transform: translateY(0);
    }
    .curtain-bottom::before { background-position: center bottom; }
    .curtain::after {
      content: '🌍';
      font-size: 3.2rem;
      opacity: .22;
      position: relative;
      z-index: 1;
    }

    /* Reveal states */
    .reveal-half .curtain-top    { transform: translateY(-100%); }
    .reveal-full .curtain-top    { transform: translateY(-100%); }
    .reveal-full .curtain-bottom { transform: translateY(100%); }

    /* Click hint */
    .click-hint {
      position: absolute;
      bottom: 14px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(214,64,0,.92);
      color: var(--white);
      padding: 7px 22px;
      border-radius: 30px;
      font-size: .82rem;
      font-weight: 700;
      z-index: 5;
      pointer-events: none;
      white-space: nowrap;
      box-shadow: 0 3px 14px rgba(0,0,0,.4);
      transition: opacity .45s;
    }
    .reveal-full .click-hint { opacity: 0; }

    /* Step pips */
    .img-pips {
      position: absolute;
      top: 12px; right: 14px;
      display: flex; gap: 6px;
      z-index: 5;
    }
    .img-pip {
      width: 10px; height: 10px;
      border-radius: 50%;
      background: rgba(255,255,255,.28);
      transition: background .3s;
    }
    .img-pip.lit { background: var(--light-red); }

    /* Loading spinner */
    .img-loader {
      position: absolute;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 2;
      pointer-events: none;
      background: rgba(0,13,6,.6);
    }
    .spinner {
      width: 44px; height: 44px;
      border: 4px solid rgba(255,255,255,.15);
      border-top-color: var(--light-red);
      border-radius: 50%;
      animation: spin .8s linear infinite;
    }
    @keyframes spin { to { transform: rotate(360deg); } }
    .img-loader.hidden { display: none; }

    /* ── CHOICES ────────────────────────────────── */
    .choices-area {
      padding: 20px 22px 4px;
    }
    .choices-label {
      text-align: center;
      font-size: 1rem;
      color: var(--brand-dark);
      margin-bottom: 14px;
      font-weight: 700;
    }
    .choices-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }
    .choice-btn {
      background: var(--pale-green);
      border: 2px solid var(--soft-border);
      color: var(--text-dark);
      padding: 15px 10px;
      border-radius: 14px;
      font-size: .97rem;
      font-weight: 700;
      cursor: pointer;
      transition: all .22s;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      min-height: 58px;
      text-align: center;
      line-height: 1.3;
    }
    .choice-btn:hover:not(:disabled) {
      background: var(--mid-green);
      color: var(--white);
      border-color: var(--mid-green);
      transform: translateY(-2px);
      box-shadow: 0 5px 18px rgba(18,49,35,.28);
    }
    .choice-btn.correct {
      background: #1e8c4a;
      border-color: #166337;
      color: var(--white);
      animation: pop .4s ease;
    }
    .choice-btn.wrong {
      background: #d63b3b;
      border-color: #a82b2b;
      color: var(--white);
    }
    .choice-btn:disabled { cursor: default; }

    @keyframes pop {
      0%   { transform: scale(1); }
      45%  { transform: scale(1.07); }
      100% { transform: scale(1); }
    }

    /* ── RESULT + CAPTION ───────────────────────── */
    .result-msg {
      text-align: center;
      margin: 14px 0 0;
      font-size: 1.05rem;
      font-weight: 700;
      min-height: 26px;
      padding: 0 10px;
    }
    .result-msg.correct { color: #1a7a40; }
    .result-msg.wrong   { color: var(--brand-red); }

    .landmark-caption {
      text-align: center;
      padding: 8px 20px 0;
      font-size: .82rem;
      color: var(--muted-text);
      font-style: italic;
      min-height: 22px;
    }

    /* ── CARD FOOTER ────────────────────────────── */
    .card-footer {
      padding: 16px 22px 22px;
      display: flex;
      justify-content: center;
      gap: 12px;
      flex-wrap: wrap;
    }
    .action-btn {
      border: none;
      padding: 11px 28px;
      border-radius: 30px;
      font-size: .95rem;
      font-weight: 800;
      cursor: pointer;
      transition: all .22s;
      letter-spacing: .4px;
    }
    .btn-primary {
      background: var(--brand-red);
      color: var(--white);
      box-shadow: 0 5px 18px rgba(214,64,0,.38);
    }
    .btn-primary:hover { background: var(--red-hover); transform: translateY(-2px); }
    .btn-secondary {
      background: var(--pale-green);
      color: var(--brand-dark);
      border: 2px solid var(--soft-border);
    }
    .btn-secondary:hover { background: var(--soft-border); transform: translateY(-2px); }

    /* ── FINAL SCREEN ───────────────────────────── */
    .final-screen {
      padding: 46px 28px 40px;
      text-align: center;
    }
    .final-screen .trophy { font-size: 5rem; display: block; margin-bottom: 16px; }
    .final-screen h2 { color: var(--brand-red); font-size: 1.9rem; margin-bottom: 10px; }
    .final-screen .verdict { color: var(--brand-dark); font-size: 1.05rem; margin-bottom: 8px; }
    .final-screen .recap {
      color: var(--muted-text);
      font-size: .88rem;
      margin-bottom: 28px;
      line-height: 1.8;
    }

    /* ── SITE FOOTER ────────────────────────────── */
    .site-footer {
      margin-top: 30px;
      text-align: center;
      font-size: .76rem;
      color: rgba(160,210,185,.75);
      line-height: 1.8;
      max-width: 640px;
      position: relative;
      z-index: 1;
      text-shadow: 0 1px 4px rgba(0,0,0,.6);
    }
    .site-footer a { color: #8ec8a4; }

    /* ── RESPONSIVE ─────────────────────────────── */
    @media (max-width: 480px) {
      .choices-grid  { grid-template-columns: 1fr; }
      .image-wrapper { height: 260px; }
    }
  </style>
</head>
<body>

  <!-- ═══ GLOBE BACKGROUND LAYERS ═══ -->
  <div class="globe-bg" aria-hidden="true"></div>
  <div class="globe-vignette" aria-hidden="true"></div>

  <!-- ═══ STAR FIELD (injected by JS) ═══ -->
  <div class="stars" id="starField" aria-hidden="true"></div>

  <!-- ═══════════════════════ HEADER ═══════════════════════ -->
  <header class="site-header">
    <h1>🌍 Where in the World?</h1>
    <p class="sub">Icebreaker Game — Guess the landmark, name the country!</p>
    <span class="pill-tag">✈️ 4 Rounds · 4 Destinations</span>
  </header>

  <!-- ═══════════════════════ SCORE ROW ════════════════════ -->
  <div class="meta-row">
    <span>Round <strong id="roundDisplay">1</strong> of 4</span>
    <span>Score <strong id="scoreDisplay">0</strong> / 4</span>
  </div>

  <!-- ═══════════════════════ PROGRESS DOTS ════════════════ -->
  <div class="progress-dots" id="progressDots"></div>

  <!-- ═══════════════════════ ROUND NAV ════════════════════ -->
  <nav class="round-nav" id="roundNav" aria-label="Select round"></nav>

  <!-- ═══════════════════════ GAME CARD ════════════════════ -->
  <main class="game-card" id="gameCard">

    <div class="card-header" id="cardHeader">🔍 Round 1 — Where is this place?</div>

    <!-- IMAGE REVEAL ZONE -->
    <div class="image-wrapper" id="imageWrapper"
         role="button" tabindex="0"
         aria-label="Click to reveal the landmark image"
         onclick="revealStep()">

      <img id="landmarkImg" src="" alt="Mystery landmark" />

      <div class="img-loader" id="imgLoader">
        <div class="spinner"></div>
      </div>

      <div class="curtain curtain-top"    id="curtainTop"></div>
      <div class="curtain curtain-bottom" id="curtainBottom"></div>

      <div class="click-hint" id="clickHint">👆 Click to reveal the top half!</div>

      <div class="img-pips">
        <div class="img-pip" id="pip0"></div>
        <div class="img-pip" id="pip1"></div>
      </div>

    </div>

    <!-- CHOICES -->
    <section class="choices-area">
      <p class="choices-label">🗺️ Which country does this landmark belong to?</p>
      <div class="choices-grid" id="choicesGrid"></div>
      <p class="result-msg" id="resultMsg" role="alert"></p>
    </section>

    <p class="landmark-caption" id="landmarkCaption"></p>

    <div class="card-footer">
      <button class="action-btn btn-secondary" onclick="resetRound()">🔄 Reset Round</button>
      <button class="action-btn btn-primary" id="nextBtn"
              onclick="nextRound()" style="display:none;">Next Round ➡️</button>
    </div>

  </main>

  <!-- ═══════════════════════ SITE FOOTER ══════════════════ -->
  <footer class="site-footer">
    Background: NASA Blue Marble (Apollo 17, 1972) via
    <a href="https://commons.wikimedia.org/wiki/File:The_Earth_seen_from_Apollo_17.jpg"
       target="_blank" rel="noopener">Wikimedia Commons</a> — Public Domain.<br/>
    Landmark images from Wikimedia Commons (CC / Public Domain).<br/>
    🎮 <em>"Where in the World?"</em> Icebreaker &nbsp;|&nbsp; GitHub Pages ready
  </footer>


<!-- ═══════════════════════════════════════════════════════
     GAME LOGIC
════════════════════════════════════════════════════════ -->
<script>
/* ─────────────────────────────────────────────────────────
   STAR FIELD — generate 120 random CSS stars
──────────────────────────────────────────────────────────── */
(function buildStars() {
  const field = document.getElementById('starField');
  const count = 120;
  for (let i = 0; i < count; i++) {
    const s    = document.createElement('span');
    const size = Math.random() * 2.2 + 0.5;
    const dur  = (Math.random() * 5 + 2).toFixed(1);
    const del  = (Math.random() * 6).toFixed(1);
    const op   = (Math.random() * 0.5 + 0.3).toFixed(2);
    s.style.cssText = `
      left:${(Math.random()*100).toFixed(2)}%;
      top:${(Math.random()*100).toFixed(2)}%;
      width:${size.toFixed(1)}px;
      height:${size.toFixed(1)}px;
      --dur:${dur}s;
      --op:${op};
      animation-delay:${del}s;
    `;
    field.appendChild(s);
  }
})();

/* ─────────────────────────────────────────────────────────
   ROUND DATA — confirmed Wikimedia Commons direct URLs
──────────────────────────────────────────────────────────── */
const rounds = [
  {
    country : "Philippines",
    landmark: "Siquijor Island, Philippines",
    hint    : "A mystical island in the Central Visayas known for pristine beaches and local folklore",
    emoji   : "🇵🇭",
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a6/Paliton_Beach%2C_Siquijor%2C_Philippines.JPG/1280px-Paliton_Beach%2C_Siquijor%2C_Philippines.JPG",
    choices : ["Vietnam", "Philippines", "Indonesia", "Malaysia"]
  },
  {
    country : "India",
    landmark: "Majuli Island, Assam, India",
    hint    : "The world's largest river island, sitting in the mighty Brahmaputra River",
    emoji   : "🇮🇳",
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Ferry_service_connecting_Majuli_island.jpg/1280px-Ferry_service_connecting_Majuli_island.jpg",
    choices : ["Bangladesh", "Myanmar", "India", "Nepal"]
  },
  {
    country : "Peru",
    landmark: "Choquequirao, Cusco Region, Peru",
    hint    : "A partly excavated Inca 'lost city' deep in the Andes — rarer than Machu Picchu!",
    emoji   : "🇵🇪",
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Choquequirao_Plaza.JPG/1280px-Choquequirao_Plaza.JPG",
    choices : ["Bolivia", "Colombia", "Peru", "Ecuador"]
  },
  {
    country : "USA",
    landmark: "North Cascades National Park, Washington, USA",
    hint    : "Called the 'American Alps' — rugged wilderness of glaciers and turquoise lakes",
    emoji   : "🇺🇸",
    image   : "https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Diablo_lake_north_cascades.jpg/1280px-Diablo_lake_north_cascades.jpg",
    choices : ["Canada", "Norway", "USA", "New Zealand"]
  }
];

/* ─────────────────────────────────────────────────────────
   STATE
──────────────────────────────────────────────────────────── */
let currentRound = 0;
let score        = 0;
let revealState  = 0;
let answered     = false;

/* ─────────────────────────────────────────────────────────
   BUILD NAV + DOTS
──────────────────────────────────────────────────────────── */
function buildNav() {
  const nav  = document.getElementById('roundNav');
  const dots = document.getElementById('progressDots');
  nav.innerHTML  = '';
  dots.innerHTML = '';

  rounds.forEach((r, i) => {
    const btn = document.createElement('button');
    btn.className   = 'round-btn' + (i === currentRound ? ' active' : '');
    btn.textContent = r.emoji + '  Round ' + (i + 1);
    btn.setAttribute('aria-label', 'Go to round ' + (i + 1));
    btn.onclick = () => { currentRound = i; loadRound(); };
    nav.appendChild(btn);

    const dot = document.createElement('div');
    dot.className = 'dot'
      + (i < currentRound   ? ' done'    : '')
      + (i === currentRound ? ' current' : '');
    dots.appendChild(dot);
  });
}

/* ─────────────────────────────────────────────────────────
   LOAD ROUND
──────────────────────────────────────────────────────────── */
function loadRound() {
  revealState = 0;
  answered    = false;

  const r = rounds[currentRound];

  document.getElementById('cardHeader').textContent =
    '🔍 Round ' + (currentRound + 1) + ' — Where is this place?';
  document.getElementById('roundDisplay').textContent = currentRound + 1;
  document.getElementById('scoreDisplay').textContent = score;

  /* Reset curtains */
  const wrapper = document.getElementById('imageWrapper');
  wrapper.classList.remove('reveal-half', 'reveal-full');

  /* Load image with spinner */
  const img    = document.getElementById('landmarkImg');
  const loader = document.getElementById('imgLoader');
  loader.classList.remove('hidden');
  img.src = '';
  img.alt = 'Mystery landmark';
  img.onload  = () => loader.classList.add('hidden');
  img.onerror = () => {
    loader.classList.add('hidden');
    img.alt = '⚠️ Image unavailable — check connection.';
  };
  img.src = r.image;

  /* Reset hint + pips */
  const hint = document.getElementById('clickHint');
  hint.textContent = '👆 Click to reveal the top half!';
  hint.style.opacity = '1';
  document.getElementById('pip0').classList.remove('lit');
  document.getElementById('pip1').classList.remove('lit');

  /* Clear result + caption */
  const msg = document.getElementById('resultMsg');
  msg.textContent = '';
  msg.className   = 'result-msg';
  document.getElementById('landmarkCaption').textContent = '';

  /* Reset next button */
  const nextBtn = document.getElementById('nextBtn');
  nextBtn.style.display = 'none';
  nextBtn.textContent   = 'Next Round ➡️';
  nextBtn.onclick       = nextRound;

  /* Build shuffled choices */
  const grid = document.getElementById('choicesGrid');
  grid.innerHTML = '';
  const shuffled = [...r.choices].sort(() => Math.random() - .5);
  shuffled.forEach(choice => {
    const btn = document.createElement('button');
    btn.className   = 'choice-btn';
    btn.textContent = choice;
    btn.onclick     = () => checkAnswer(choice, btn);
    grid.appendChild(btn);
  });

  buildNav();
}

/* ─────────────────────────────────────────────────────────
   REVEAL STEPS
──────────────────────────────────────────────────────────── */
function revealStep() {
  if (answered) return;

  if (revealState === 0) {
    document.getElementById('imageWrapper').classList.add('reveal-half');
    document.getElementById('clickHint').textContent = '👆 Click again for the full picture!';
    document.getElementById('pip0').classList.add('lit');
    revealState = 1;
  } else if (revealState === 1) {
    document.getElementById('imageWrapper').classList.add('reveal-full');
    document.getElementById('pip1').classList.add('lit');
    revealState = 2;
  }
}

/* ─────────────────────────────────────────────────────────
   CHECK ANSWER
──────────────────────────────────────────────────────────── */
function checkAnswer(choice, clickedBtn) {
  if (answered) return;
  answered = true;

  /* Force full reveal */
  const wrapper = document.getElementById('imageWrapper');
  wrapper.classList.add('reveal-half', 'reveal-full');
  document.getElementById('pip0').classList.add('lit');
  document.getElementById('pip1').classList.add('lit');
  revealState = 2;

  const r       = rounds[currentRound];
  const allBtns = document.querySelectorAll('.choice-btn');
  const msg     = document.getElementById('resultMsg');

  allBtns.forEach(b => {
    b.disabled = true;
    if (b.textContent === r.country) b.classList.add('correct');
  });

  if (choice === r.country) {
    clickedBtn.classList.add('correct');
    msg.textContent = '✅ Correct! That\'s ' + r.landmark + '! 🎉';
    msg.className   = 'result-msg correct';
    score++;
    document.getElementById('scoreDisplay').textContent = score;
  } else {
    clickedBtn.classList.add('wrong');
    msg.textContent = '❌ Not quite! It\'s ' + r.landmark + '.';
    msg.className   = 'result-msg wrong';
  }

  document.getElementById('landmarkCaption').textContent =
    '📍 ' + r.landmark + ' — ' + r.hint;

  const nextBtn = document.getElementById('nextBtn');
  nextBtn.style.display = 'inline-block';
  if (currentRound === rounds.length - 1) {
    nextBtn.textContent = '🏆 See Final Score';
    nextBtn.onclick     = showFinalScore;
  }
}

/* ─────────────────────────────────────────────────────────
   NAVIGATION
──────────────────────────────────────────────────────────── */
function nextRound()  { currentRound++; loadRound(); }
function resetRound() { loadRound(); }

/* ─────────────────────────────────────────────────────────
   FINAL SCORE SCREEN
──────────────────────────────────────────────────────────── */
function showFinalScore() {
  const icons = ['😅','🥉','🥉','🥈','🥇'];
  const msgs  = [
    "Keep exploring — every journey starts with curiosity! 🌱",
    "One more trip around the globe and you'll nail it! ✈️",
    "Not bad! You've got a traveller's instinct! 🗺️",
    "Great job! You really know your world landmarks! 🌟",
    "Perfect score! You are a certified World Explorer! 🌍"
  ];

  document.getElementById('gameCard').innerHTML = `
    <div class="card-header" style="background:var(--brand-red);">🏆 Final Results</div>
    <div class="final-screen">
      <span class="trophy">${icons[score]}</span>
      <h2>You scored ${score} out of ${rounds.length}!</h2>
      <p class="verdict">${msgs[score]}</p>
      <p class="recap">
        🇵🇭 Siquijor Island, Philippines<br/>
        🇮🇳 Majuli Island, Assam, India<br/>
        🇵🇪 Choquequirao, Cusco Region, Peru<br/>
        🇺🇸 North Cascades National Park, USA
      </p>
      <button class="action-btn btn-primary" onclick="restartGame()">🔄 Play Again</button>
    </div>
  `;
}

/* ─────────────────────────────────────────────────────────
   RESTART
──────────────────────────────────────────────────────────── */
function restartGame() {
  currentRound = 0;
  score        = 0;

  document.getElementById('gameCard').innerHTML = `
    <div class="card-header" id="cardHeader">🔍 Round 1 — Where is this place?</div>
    <div class="image-wrapper" id="imageWrapper"
         role="button" tabindex="0"
         aria-label="Click to reveal the landmark image"
         onclick="revealStep()">
      <img id="landmarkImg" src="" alt="Mystery landmark" />
      <div class="img-loader" id="imgLoader"><div class="spinner"></div></div>
      <div class="curtain curtain-top"    id="curtainTop"></div>
      <div class="curtain curtain-bottom" id="curtainBottom"></div>
      <div class="click-hint" id="clickHint">👆 Click to reveal the top half!</div>
      <div class="img-pips">
        <div class="img-pip" id="pip0"></div>
        <div class="img-pip" id="pip1"></div>
      </div>
    </div>
    <section class="choices-area">
      <p class="choices-label">🗺️ Which country does this landmark belong to?</p>
      <div class="choices-grid" id="choicesGrid"></div>
      <p class="result-msg" id="resultMsg" role="alert"></p>
    </section>
    <p class="landmark-caption" id="landmarkCaption"></p>
    <div class="card-footer">
      <button class="action-btn btn-secondary" onclick="resetRound()">🔄 Reset Round</button>
      <button class="action-btn btn-primary" id="nextBtn"
              onclick="nextRound()" style="display:none;">Next Round ➡️</button>
    </div>
  `;

  loadRound();
}

/* ─────────────────────────────────────────────────────────
   KEYBOARD — Enter / Space on image wrapper
──────────────────────────────────────────────────────────── */
document.addEventListener('keydown', e => {
  if ((e.key === 'Enter' || e.key === ' ') &&
       document.activeElement &&
       document.activeElement.id === 'imageWrapper') {
    e.preventDefault();
    revealStep();
  }
});

/* INIT */
loadRound();
</script>
</body>
</html>
