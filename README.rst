<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>🐻 Animated Login with Rive — Flutter</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=JetBrains+Mono:wght@400;500&family=Nunito:wght@400;500;600&display=swap" rel="stylesheet" />
<style>
  :root {
    --bg:        #0f0e0c;
    --bg2:       #181612;
    --bg3:       #201d18;
    --card:      #1e1a14;
    --border:    #2e2820;
    --amber:     #f5a623;
    --amber2:    #e8892a;
    --amber-glow:#f5a62330;
    --teal:      #3ecfb2;
    --red:       #e05a5a;
    --green:     #5ecb8c;
    --text:      #ede8df;
    --muted:     #8a7f6e;
    --code-bg:   #141210;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Nunito', sans-serif;
    font-size: 16px;
    line-height: 1.75;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── NOISE TEXTURE OVERLAY ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: .5;
  }

  .container {
    max-width: 860px;
    margin: 0 auto;
    padding: 0 24px 80px;
    position: relative;
    z-index: 1;
  }

  /* ── HERO ── */
  .hero {
    text-align: center;
    padding: 72px 0 56px;
    position: relative;
  }

  .hero-bear {
    font-size: 88px;
    line-height: 1;
    display: block;
    animation: float 4s ease-in-out infinite;
    filter: drop-shadow(0 0 32px var(--amber-glow));
    margin-bottom: 24px;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50%       { transform: translateY(-12px); }
  }

  .hero h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(2rem, 5vw, 3.2rem);
    font-weight: 800;
    letter-spacing: -0.02em;
    line-height: 1.1;
    background: linear-gradient(135deg, var(--amber) 0%, #fff4dc 50%, var(--amber2) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 20px;
  }

  .hero-desc {
    font-size: 1.1rem;
    color: var(--muted);
    max-width: 580px;
    margin: 0 auto 32px;
    font-weight: 500;
  }

  /* ── BADGES ── */
  .badges {
    display: flex;
    gap: 10px;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 0;
  }

  .badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 6px 14px;
    border-radius: 999px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    font-weight: 500;
    border: 1px solid var(--border);
    background: var(--bg3);
    color: var(--muted);
    transition: all .2s;
  }
  .badge-amber { border-color: #f5a62340; color: var(--amber); background: #f5a62310; }
  .badge-teal  { border-color: #3ecfb230; color: var(--teal);  background: #3ecfb210; }
  .badge-red   { border-color: #e05a5a30; color: var(--red);   background: #e05a5a10; }
  .badge .dot  { width: 7px; height: 7px; border-radius: 50%; display: inline-block; }
  .badge-amber .dot { background: var(--amber); box-shadow: 0 0 6px var(--amber); }
  .badge-teal  .dot { background: var(--teal);  box-shadow: 0 0 6px var(--teal); }
  .badge-red   .dot { background: var(--red);   box-shadow: 0 0 6px var(--red); }

  /* ── DIVIDER ── */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border) 30%, var(--border) 70%, transparent);
    margin: 52px 0;
  }

  /* ── SECTION ── */
  section { margin-bottom: 52px; }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 1.45rem;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-title .icon {
    font-size: 1.3rem;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 38px;
    height: 38px;
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: 10px;
    flex-shrink: 0;
  }

  /* ── FEATURE CARDS ── */
  .feature-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 14px;
  }

  .feature-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 22px 20px;
    position: relative;
    overflow: hidden;
    transition: border-color .25s, transform .25s;
  }
  .feature-card:hover {
    border-color: var(--amber2);
    transform: translateY(-3px);
  }
  .feature-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--amber), var(--amber2));
    opacity: 0;
    transition: opacity .25s;
  }
  .feature-card:hover::before { opacity: 1; }

  .feature-emoji { font-size: 1.8rem; margin-bottom: 10px; display: block; }
  .feature-card h3 {
    font-family: 'Syne', sans-serif;
    font-size: 1rem;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 6px;
  }
  .feature-card p { color: var(--muted); font-size: 0.9rem; line-height: 1.6; }

  /* ── CONCEPT CARDS ── */
  .concept-grid { display: grid; gap: 14px; }

  .concept-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 24px 24px;
    border-left: 3px solid var(--amber);
  }
  .concept-card h3 {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    color: var(--amber);
    margin-bottom: 8px;
    font-size: 1.05rem;
  }
  .concept-card p { color: var(--muted); font-size: 0.95rem; }
  .concept-card strong { color: var(--text); }

  /* ── TECH STACK ── */
  .tech-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 14px;
  }

  .tech-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 22px 18px;
    text-align: center;
    transition: transform .2s, border-color .2s;
  }
  .tech-card:hover { transform: translateY(-4px); border-color: var(--amber2); }
  .tech-card .tech-icon { font-size: 2rem; margin-bottom: 10px; display: block; }
  .tech-card .tech-name {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    color: var(--text);
    font-size: 1rem;
  }
  .tech-card .tech-role { color: var(--muted); font-size: 0.82rem; margin-top: 4px; }

  /* ── INPUTS GRID ── */
  .inputs-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 20px;
  }
  .input-chip {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px 16px;
  }
  .input-chip .chip-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.82rem;
    color: var(--amber);
    font-weight: 500;
    margin-bottom: 4px;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .chip-type {
    font-size: 0.68rem;
    background: var(--amber-glow);
    color: var(--amber2);
    border-radius: 4px;
    padding: 1px 6px;
    font-family: 'JetBrains Mono', monospace;
  }
  .chip-type.trigger {
    background: #3ecfb215;
    color: var(--teal);
  }
  .input-chip p { color: var(--muted); font-size: 0.85rem; }

  /* ── CODE BLOCKS ── */
  .code-wrap {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
    margin: 16px 0;
  }
  .code-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 16px;
    border-bottom: 1px solid var(--border);
    background: #0d0b09;
  }
  .code-lang {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    color: var(--muted);
    letter-spacing: .05em;
    text-transform: uppercase;
  }
  .code-dots { display: flex; gap: 6px; }
  .code-dots span {
    width: 10px; height: 10px; border-radius: 50%;
  }
  .code-dots .d1 { background: #e05a5a; }
  .code-dots .d2 { background: #f5c842; }
  .code-dots .d3 { background: #5ecb8c; }

  pre {
    padding: 20px 20px;
    overflow-x: auto;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.83rem;
    line-height: 1.7;
    color: #cdc5b4;
  }
  .kw  { color: #cc99cd; }
  .str { color: #7ec8a0; }
  .cmt { color: #5a5348; font-style: italic; }
  .fn  { color: var(--amber); }
  .cls { color: #79c0ff; }
  .num { color: #f0883e; }

  /* ── FOLDER TREE ── */
  .tree {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
  }
  .tree pre {
    color: var(--muted);
    padding: 20px 24px;
  }
  .tree .folder { color: var(--amber); }
  .tree .file   { color: #79c0ff; }
  .tree .comment { color: #3a3530; }

  /* ── DEMO BOX ── */
  .demo-box {
    background: var(--card);
    border: 2px dashed var(--border);
    border-radius: 16px;
    padding: 48px 24px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .demo-box::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at 50% 0%, var(--amber-glow), transparent 60%);
  }
  .demo-box p {
    color: var(--muted);
    position: relative;
    font-size: 0.95rem;
  }
  .demo-placeholder-icon {
    font-size: 3.5rem;
    display: block;
    margin-bottom: 16px;
    filter: grayscale(.4);
    position: relative;
  }
  .demo-box strong { color: var(--amber); }

  /* ── TABLE ── */
  table {
    width: 100%;
    border-collapse: collapse;
    background: var(--card);
    border-radius: 12px;
    overflow: hidden;
    border: 1px solid var(--border);
  }
  th {
    background: var(--bg3);
    color: var(--muted);
    font-family: 'Syne', sans-serif;
    font-size: 0.8rem;
    font-weight: 600;
    letter-spacing: .06em;
    text-transform: uppercase;
    padding: 12px 20px;
    text-align: left;
    border-bottom: 1px solid var(--border);
  }
  td {
    padding: 14px 20px;
    border-bottom: 1px solid var(--border);
    color: var(--muted);
    font-size: 0.92rem;
  }
  tr:last-child td { border-bottom: none; }
  td strong { color: var(--text); }
  td code {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.82rem;
    background: var(--bg3);
    padding: 2px 8px;
    border-radius: 5px;
    color: var(--amber);
    border: 1px solid var(--border);
  }

  /* ── CREDITS ── */
  .credits-card {
    background: linear-gradient(135deg, #1a1510, #1e1a14);
    border: 1px solid #f5a62330;
    border-radius: 16px;
    padding: 28px 28px;
    display: flex;
    align-items: flex-start;
    gap: 18px;
  }
  .credits-icon { font-size: 2.4rem; flex-shrink: 0; }
  .credits-card h3 {
    font-family: 'Syne', sans-serif;
    font-size: 1rem;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 6px;
  }
  .credits-card p { color: var(--muted); font-size: 0.9rem; }
  .credits-card a {
    color: var(--amber);
    text-decoration: none;
    font-weight: 600;
    border-bottom: 1px solid var(--amber-glow);
    transition: border-color .2s;
  }
  .credits-card a:hover { border-color: var(--amber); }

  /* ── FOOTER ── */
  footer {
    text-align: center;
    padding: 32px 0 0;
    color: var(--muted);
    font-size: 0.88rem;
    border-top: 1px solid var(--border);
  }
  footer span { color: var(--amber); }

  /* ── NOTE ── */
  .note {
    background: #3ecfb210;
    border: 1px solid #3ecfb230;
    border-left: 3px solid var(--teal);
    border-radius: 0 10px 10px 0;
    padding: 14px 18px;
    color: var(--muted);
    font-size: 0.9rem;
    margin: 16px 0;
  }
  .note strong { color: var(--teal); }

  /* ── REVEAL ANIMATION ── */
  .reveal {
    opacity: 0;
    transform: translateY(24px);
    animation: revealUp .6s ease forwards;
  }
  @keyframes revealUp {
    to { opacity: 1; transform: translateY(0); }
  }
  .d1 { animation-delay: .05s; }
  .d2 { animation-delay: .12s; }
  .d3 { animation-delay: .2s; }
  .d4 { animation-delay: .28s; }
  .d5 { animation-delay: .36s; }
  .d6 { animation-delay: .44s; }
  .d7 { animation-delay: .52s; }
  .d8 { animation-delay: .6s; }
  .d9 { animation-delay: .68s; }

  @media (max-width: 600px) {
    .tech-grid    { grid-template-columns: 1fr 1fr; }
    .inputs-grid  { grid-template-columns: 1fr; }
    .credits-card { flex-direction: column; }
  }
</style>
</head>
<body>

<div class="container">

  <!-- HERO -->
  <div class="hero reveal d1">
    <span class="hero-bear">🐻</span>
    <h1>Animated Login with Rive</h1>
    <p class="hero-desc">
      A delightful Flutter login screen where an animated bear tracks your eyes,
      covers his face for privacy, and reacts to your authentication — all in real time.
    </p>
    <div class="badges">
      <span class="badge badge-amber"><span class="dot"></span>Flutter 3.x</span>
      <span class="badge badge-amber"><span class="dot"></span>Dart</span>
      <span class="badge badge-teal"><span class="dot"></span>Rive Animation</span>
      <span class="badge"><span class="dot"></span>Educational</span>
      <span class="badge badge-red"><span class="dot"></span>State Machine</span>
    </div>
  </div>

  <div class="divider"></div>

  <!-- FEATURES -->
  <section class="reveal d2">
    <h2 class="section-title"><span class="icon">✨</span> Features</h2>
    <div class="feature-grid">
      <div class="feature-card">
        <span class="feature-emoji">👀</span>
        <h3>Gaze Tracking</h3>
        <p>The bear follows your cursor as you type in the email field, simulating real eye movement tied to text input length.</p>
      </div>
      <div class="feature-card">
        <span class="feature-emoji">🙈</span>
        <h3>Privacy Mode</h3>
        <p>The bear covers his eyes when the password field is focused. Toggle visibility and he peeks — a charming "peek mode".</p>
      </div>
      <div class="feature-card">
        <span class="feature-emoji">✅</span>
        <h3>Visual Validation</h3>
        <p>Trigger-based animations react expressively to login success or failure, giving instant animated feedback.</p>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- CONCEPTS -->
  <section class="reveal d3">
    <h2 class="section-title"><span class="icon">🧠</span> Key Concepts</h2>
    <div class="concept-grid">
      <div class="concept-card">
        <h3>What is Rive?</h3>
        <p><a href="https://rive.app" style="color:var(--amber);text-decoration:none;font-weight:600;">Rive</a> is a <strong>real-time interactive animation tool</strong> for designers and developers. Unlike GIFs or Lottie, Rive animations respond to live data, state, and user interaction — and can be controlled programmatically at runtime inside your Flutter app.</p>
      </div>
      <div class="concept-card">
        <h3>What is a State Machine?</h3>
        <p>A <strong>State Machine</strong> in Rive is a visual logic layer built on top of animations. It defines <strong>states</strong> (e.g., <code style="color:var(--amber);font-family:JetBrains Mono,monospace;font-size:.82rem;">Idle</code>, <code style="color:var(--amber);font-family:JetBrains Mono,monospace;font-size:.82rem;">HandsUp</code>, <code style="color:var(--amber);font-family:JetBrains Mono,monospace;font-size:.82rem;">Success</code>) and <strong>transitions</strong> between them driven by boolean inputs and triggers your app controls at runtime — no animation code required.</p>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- TECH STACK -->
  <section class="reveal d4">
    <h2 class="section-title"><span class="icon">🛠️</span> Tech Stack</h2>
    <div class="tech-grid">
      <div class="tech-card">
        <span class="tech-icon">🐦</span>
        <div class="tech-name">Flutter</div>
        <div class="tech-role">UI framework & widget tree</div>
      </div>
      <div class="tech-card">
        <span class="tech-icon">🎯</span>
        <div class="tech-name">Dart</div>
        <div class="tech-role">Programming language</div>
      </div>
      <div class="tech-card">
        <span class="tech-icon">🎨</span>
        <div class="tech-name">Rive</div>
        <div class="tech-role">Animation & state machine</div>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- STATE MACHINE -->
  <section class="reveal d5">
    <h2 class="section-title"><span class="icon">🎮</span> State Machine Inputs</h2>
    <p style="color:var(--muted);margin-bottom:18px;">This project controls a Rive animation called <strong style="color:var(--text);">"Login Machine"</strong> via <code style="color:var(--amber);font-family:JetBrains Mono,monospace;font-size:.85rem;">StateMachineController</code> with four inputs:</p>

    <div class="inputs-grid">
      <div class="input-chip">
        <div class="chip-label">isChecking <span class="chip-type">bool</span></div>
        <p>Bear tracks the email text field as you type</p>
      </div>
      <div class="input-chip">
        <div class="chip-label">isHandsUp <span class="chip-type">bool</span></div>
        <p>Bear covers his eyes when password is focused</p>
      </div>
      <div class="input-chip">
        <div class="chip-label">trigSuccess <span class="chip-type trigger">trigger</span></div>
        <p>Fires on a successful login attempt</p>
      </div>
      <div class="input-chip">
        <div class="chip-label">trigFail <span class="chip-type trigger">trigger</span></div>
        <p>Fires on a failed login attempt</p>
      </div>
    </div>

    <div class="note">
      <strong>💡 Peek Mode:</strong> The <code style="font-family:JetBrains Mono,monospace;font-size:.82rem;">obscureText</code> toggle is synced with <code style="font-family:JetBrains Mono,monospace;font-size:.82rem;">isHandsUp</code> — when the user reveals their password, the bear briefly uncovers his eyes.
    </div>
  </section>

  <div class="divider"></div>

  <!-- PROJECT STRUCTURE -->
  <section class="reveal d5">
    <h2 class="section-title"><span class="icon">📁</span> Project Structure</h2>
    <div class="tree">
      <div class="code-header">
        <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
        <span class="code-lang">directory</span>
      </div>
      <pre><span class="folder">flutter_rive_login/</span>
│
├── <span class="folder">lib/</span>
│   ├── <span class="file">main.dart</span>              <span class="comment"># App entry point</span>
│   └── <span class="file">login_screen.dart</span>      <span class="comment"># Login UI + Rive controller logic</span>
│
├── <span class="folder">assets/</span>
│   └── <span class="folder">animations/</span>
│       └── <span class="file">login_teddy.riv</span>    <span class="comment"># Rive animation file (bear)</span>
│
├── <span class="file">pubspec.yaml</span>               <span class="comment"># Dependencies & asset declarations</span>
└── <span class="file">README.md</span></pre>
    </div>
  </section>

  <div class="divider"></div>

  <!-- GETTING STARTED -->
  <section class="reveal d6">
    <h2 class="section-title"><span class="icon">⚙️</span> Getting Started</h2>

    <div class="code-wrap">
      <div class="code-header">
        <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
        <span class="code-lang">bash</span>
      </div>
      <pre><span class="cmt"># 1. Clone the repository</span>
git clone https://github.com/your-username/flutter-rive-login.git
cd flutter-rive-login

<span class="cmt"># 2. Install dependencies</span>
flutter pub get

<span class="cmt"># 3. Run the app</span>
flutter run</pre>
    </div>

    <p style="color:var(--muted);margin: 20px 0 10px;">Add Rive to your <code style="color:var(--amber);font-family:JetBrains Mono,monospace;font-size:.82rem;">pubspec.yaml</code>:</p>

    <div class="code-wrap">
      <div class="code-header">
        <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
        <span class="code-lang">yaml</span>
      </div>
      <pre><span class="kw">dependencies:</span>
  flutter:
    sdk: flutter
  <span class="fn">rive</span>: ^0.12.0   <span class="cmt"># Check pub.dev for latest</span>

<span class="kw">flutter:</span>
  <span class="kw">assets:</span>
    - <span class="str">assets/animations/login_teddy.riv</span></pre>
    </div>
  </section>

  <div class="divider"></div>

  <!-- CORE IMPL -->
  <section class="reveal d6">
    <h2 class="section-title"><span class="icon">🧩</span> Core Implementation</h2>

    <p style="color:var(--muted);margin-bottom:14px;font-weight:600;color:var(--text);">Initializing the State Machine</p>
    <div class="code-wrap">
      <div class="code-header">
        <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
        <span class="code-lang">dart</span>
      </div>
      <pre><span class="cls">StateMachineController</span>? _controller;
<span class="cls">SMIBool</span>?    isChecking;
<span class="cls">SMIBool</span>?    isHandsUp;
<span class="cls">SMITrigger</span>? trigSuccess;
<span class="cls">SMITrigger</span>? trigFail;

<span class="kw">void</span> <span class="fn">_onRiveInit</span>(<span class="cls">Artboard</span> artboard) {
  <span class="kw">final</span> controller = <span class="cls">StateMachineController</span>.<span class="fn">fromArtboard</span>(
    artboard,
    <span class="str">'Login Machine'</span>,
  );
  <span class="kw">if</span> (controller != <span class="kw">null</span>) {
    artboard.<span class="fn">addController</span>(controller);
    isChecking  = controller.<span class="fn">findInput</span>&lt;<span class="kw">bool</span>&gt;(<span class="str">'isChecking'</span>)  <span class="kw">as</span> <span class="cls">SMIBool</span>;
    isHandsUp   = controller.<span class="fn">findInput</span>&lt;<span class="kw">bool</span>&gt;(<span class="str">'isHandsUp'</span>)   <span class="kw">as</span> <span class="cls">SMIBool</span>;
    trigSuccess = controller.<span class="fn">findInput</span>&lt;<span class="kw">bool</span>&gt;(<span class="str">'trigSuccess'</span>) <span class="kw">as</span> <span class="cls">SMITrigger</span>;
    trigFail    = controller.<span class="fn">findInput</span>&lt;<span class="kw">bool</span>&gt;(<span class="str">'trigFail'</span>)    <span class="kw">as</span> <span class="cls">SMITrigger</span>;
  }
}</pre>
    </div>

    <p style="color:var(--text);margin: 20px 0 14px;font-weight:600;">FocusNode Listeners</p>
    <div class="code-wrap">
      <div class="code-header">
        <div class="code-dots"><span class="d1"></span><span class="d2"></span><span class="d3"></span></div>
        <span class="code-lang">dart</span>
      </div>
      <pre>_emailFocusNode.<span class="fn">addListener</span>(() {
  isChecking?.<span class="fn">change</span>(_emailFocusNode.hasFocus);
});

_passwordFocusNode.<span class="fn">addListener</span>(() {
  isHandsUp?.<span class="fn">change</span>(_passwordFocusNode.hasFocus);
});</pre>
    </div>
  </section>

  <div class="divider"></div>

  <!-- DEMO -->
  <section class="reveal d7">
    <h2 class="section-title"><span class="icon">🎬</span> Demo</h2>
    <div class="demo-box">
      <span class="demo-placeholder-icon">🎥</span>
      <p><strong>[ Insert demo GIF here ]</strong></p>
      <p style="margin-top:8px;font-size:0.85rem;">Run the app on an emulator → record screen → convert to GIF via <a href="https://ezgif.com" style="color:var(--teal);">ezgif.com</a> or <code style="font-family:JetBrains Mono,monospace;font-size:.8rem;">ffmpeg</code></p>
    </div>
  </section>

  <div class="divider"></div>

  <!-- ACADEMIC -->
  <section class="reveal d8">
    <h2 class="section-title"><span class="icon">🎓</span> Academic Info</h2>
    <table>
      <thead>
        <tr><th>Field</th><th>Details</th></tr>
      </thead>
      <tbody>
        <tr><td>📚 <strong>Subject Name</strong></td><td><code>[ Subject Name Here ]</code></td></tr>
        <tr><td>👨‍🏫 <strong>Professor Name</strong></td><td><code>[ Professor Name Here ]</code></td></tr>
        <tr><td>🏫 <strong>Institution</strong></td><td><code>[ Institution Name Here ]</code></td></tr>
        <tr><td>📅 <strong>Semester / Year</strong></td><td><code>[ e.g., Spring 2025 ]</code></td></tr>
      </tbody>
    </table>
  </section>

  <div class="divider"></div>

  <!-- CREDITS -->
  <section class="reveal d9">
    <h2 class="section-title"><span class="icon">🙏</span> Credits</h2>
    <div class="credits-card">
      <span class="credits-icon">🎨</span>
      <div>
        <h3>Original Animation — Teddy Bear Character</h3>
        <p>
          The animated bear character used in this project was originally created for the Rive Community.
          Full credit goes to the original designer:<br><br>
          <a href="https://rive.app/community/files/1563-3098-animated-login-screen/" target="_blank">
            ✨ Teddy Login Screen by JcToon — Rive Community
          </a><br><br>
          Please visit the original file and give the creator a ❤️ if you use or are inspired by this animation.
        </p>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- LICENSE -->
  <section class="reveal d9">
    <h2 class="section-title"><span class="icon">📄</span> License</h2>
    <p style="color:var(--muted);">This project is intended for <strong style="color:var(--text);">educational purposes only</strong>. The Rive animation asset is subject to the original creator's licensing terms on the Rive Community platform. Please review those terms before using it in any commercial project.</p>
  </section>

  <footer>
    <p>Made with <span>❤️</span> and Flutter <span>🐦</span></p>
  </footer>

</div>

</body>
</html>
