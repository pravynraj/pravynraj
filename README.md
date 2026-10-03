<title>Praveen Raj</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400..800&family=Instrument+Sans:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap">
<style>
  /* Layout: left-aligned editorial single column. Hero carries a live latency trace (raw jitter vs. forecast),
     then sections with a sticky label column on the left and content on the right. */
  :root {
    --bg: #0a1118;
    --surface: #0f1a24;
    --surface-2: #15232f;
    --line: #213344;
    --fg: #e3ebf1;
    --muted: #8ea3b3;
    --accent: #5ad1c0;
    --accent-ink: #04201c;
    --signal: #f2b45a;
    --font-display: "Bricolage Grotesque", "Segoe UI", system-ui, sans-serif;
    --font-body: "Instrument Sans", "Segoe UI", system-ui, sans-serif;
    --font-mono: "JetBrains Mono", ui-monospace, "SFMono-Regular", Menlo, Consolas, monospace;
    color-scheme: dark;
  }
  @media (prefers-color-scheme: light) {
    :root:not([data-theme="dark"]) {
      --bg: #f2f5f7; --surface: #ffffff; --surface-2: #e8eef2; --line: #d1dbe2;
      --fg: #0e1a23; --muted: #51677a; --accent: #0b7a6c; --accent-ink: #ffffff; --signal: #a35f08;
      color-scheme: light;
    }
  }
  :root[data-theme="light"] {
    --bg: #f2f5f7; --surface: #ffffff; --surface-2: #e8eef2; --line: #d1dbe2;
    --fg: #0e1a23; --muted: #51677a; --accent: #0b7a6c; --accent-ink: #ffffff; --signal: #a35f08;
    color-scheme: light;
  }

  *, *::before, *::after { box-sizing: border-box; }
  html { scroll-behavior: smooth; scroll-padding-top: 4.5rem; }
  body {
    background: var(--bg);
    color: var(--fg);
    font-family: var(--font-body);
    font-size: 1rem;
    line-height: 1.6;
    -webkit-font-smoothing: antialiased;
  }
  a { color: inherit; text-decoration: none; }
  :focus-visible { outline: 2px solid var(--accent); outline-offset: 3px; border-radius: 4px; }
  h1, h2, h3, p { margin: 0; }
  h1, h2, h3 { text-wrap: balance; }

  .wrap { max-width: 1080px; margin-inline: auto; padding-inline: 1.25rem; }

  /* ---------- nav ---------- */
  .nav {
    position: sticky; top: env(safe-area-inset-top, 0px); z-index: 10;
    background: color-mix(in srgb, var(--bg) 88%, transparent);
    backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px);
    border-bottom: 1px solid var(--line);
  }
  .nav .wrap { display: flex; align-items: center; justify-content: space-between; gap: 1rem; height: 3.5rem; }
  .brand { font-family: var(--font-display); font-weight: 700; font-size: 1.05rem; letter-spacing: -0.01em; }
  .brand span { color: var(--accent); }
  .links { display: flex; gap: 1.5rem; font-family: var(--font-mono); font-size: 0.78rem; color: var(--muted); }
  .links a:hover { color: var(--fg); }
  .nav-end { display: flex; align-items: center; gap: 0.75rem; }
  .icon-btn {
    width: 2.25rem; height: 2.25rem; display: grid; place-items: center; cursor: pointer;
    background: transparent; color: var(--fg); border: 1px solid var(--line); border-radius: 8px;
  }
  .icon-btn:hover { border-color: var(--muted); }
  @media (max-width: 680px) { .links { display: none; } }

  /* ---------- hero ---------- */
  .hero { padding-block: 4rem 3rem; }
  .eyebrow { font-family: var(--font-mono); font-size: 0.78rem; color: var(--muted); letter-spacing: 0.04em; display: flex; flex-wrap: wrap; gap: 0.35rem 1rem; }
  .eyebrow b { color: var(--accent); font-weight: 500; }
  .hero h1 {
    font-family: var(--font-display); font-weight: 700; font-size: clamp(3.1rem, 11vw, 7.5rem);
    line-height: 0.95; letter-spacing: -0.035em; margin-top: 1.25rem;
  }
  .role { margin-top: 1.5rem; font-size: clamp(1.1rem, 2.4vw, 1.4rem); color: var(--fg); font-weight: 500; }
  .tagline { margin-top: 0.6rem; max-width: 40rem; color: var(--muted); font-size: 1.05rem; }
  .tagline em { font-style: normal; color: var(--accent); }
  .cta { display: flex; flex-wrap: wrap; gap: 0.75rem; margin-top: 2rem; }
  .btn {
    display: inline-flex; align-items: center; gap: 0.5rem; padding: 0.7rem 1.1rem; border-radius: 8px;
    font-weight: 600; font-size: 0.95rem; cursor: pointer; border: 1px solid var(--line); background: transparent; color: var(--fg);
    font-family: var(--font-body);
  }
  .btn:hover { border-color: var(--muted); }
  .btn.primary { background: var(--accent); color: var(--accent-ink); border-color: var(--accent); }
  .btn.primary:hover { filter: brightness(1.08); }

  .trace-card { margin-top: 3rem; background: var(--surface); border: 1px solid var(--line); border-radius: 14px; overflow: hidden; }
  .trace-head {
    display: flex; flex-wrap: wrap; justify-content: space-between; gap: 0.5rem 1.5rem; padding: 0.8rem 1rem;
    border-bottom: 1px solid var(--line); font-family: var(--font-mono); font-size: 0.74rem; color: var(--muted);
  }
  .legend { display: flex; gap: 1.1rem; flex-wrap: wrap; }
  .legend i { display: inline-block; width: 1.1rem; height: 0; border-top: 2px solid; vertical-align: middle; margin-right: 0.4rem; }
  .legend .raw i { border-color: var(--muted); border-top-width: 1px; }
  .legend .fc i { border-color: var(--accent); }
  .trace-card canvas { display: block; width: 100%; height: clamp(190px, 30vw, 280px); }

  /* ---------- sections ---------- */
  .sec { display: grid; grid-template-columns: 190px minmax(0, 1fr); gap: 2rem; padding-block: 3.5rem; border-top: 1px solid var(--line); }
  .sec > h2 {
    font-family: var(--font-mono); font-weight: 500; font-size: 0.76rem; text-transform: uppercase; letter-spacing: 0.14em; color: var(--muted);
    position: sticky; top: calc(env(safe-area-inset-top, 0px) + 5rem); align-self: start;
  }
  .sec > div { min-width: 0; }
  @media (max-width: 760px) {
    .sec { grid-template-columns: minmax(0, 1fr); gap: 1.25rem; padding-block: 2.5rem; }
    .sec > h2 { position: static; }
  }

  /* about */
  .motto { font-family: var(--font-display); font-weight: 700; font-size: clamp(1.8rem, 5vw, 3rem); line-height: 1.05; letter-spacing: -0.025em; }
  .motto span { color: var(--accent); }
  .about-grid { display: grid; grid-template-columns: minmax(0, 1fr); gap: 1.5rem; margin-top: 1.75rem; }
  .code {
    margin: 0; background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 1.1rem 1.25rem;
    font-family: var(--font-mono); font-size: 0.8rem; line-height: 1.75; overflow-x: auto; color: var(--fg);
  }
  .code .k { color: var(--accent); }
  .code .s { color: var(--signal); }
  .code .p { color: var(--muted); }
  .facts { display: grid; grid-template-columns: repeat(auto-fit, minmax(210px, 1fr)); gap: 1rem 1.5rem; }
  .fact dt { font-family: var(--font-mono); font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.12em; color: var(--muted); }
  .fact dd { margin: 0.2rem 0 0; font-weight: 500; }

  /* stack */
  .stack { display: grid; gap: 1.5rem; }
  .group { display: grid; grid-template-columns: 150px minmax(0, 1fr); gap: 0.75rem 1.5rem; align-items: baseline; }
  .group h3 { font-family: var(--font-mono); font-weight: 400; font-size: 0.78rem; color: var(--muted); }
  .chips { display: flex; flex-wrap: wrap; gap: 0.5rem; padding: 0; margin: 0; list-style: none; }
  .chips li {
    font-family: var(--font-mono); font-size: 0.8rem; padding: 0.32rem 0.7rem; border: 1px solid var(--line); border-radius: 6px; background: var(--surface);
  }
  @media (max-width: 560px) { .group { grid-template-columns: minmax(0, 1fr); } }

  /* projects */
  .projects { display: grid; gap: 0; }
  .project { padding-block: 1.6rem; border-top: 1px solid var(--line); display: grid; gap: 0.7rem; }
  .project:first-child { border-top: 0; padding-top: 0; }
  .project h3 { font-family: var(--font-display); font-weight: 600; font-size: clamp(1.25rem, 3vw, 1.6rem); letter-spacing: -0.015em; line-height: 1.2; }
  .project h3 a { display: inline; background: linear-gradient(var(--accent), var(--accent)) 0 100% / 0 2px no-repeat; transition: background-size 0.25s; }
  .project h3 a:hover { background-size: 100% 2px; color: var(--accent); }
  .project p { color: var(--muted); max-width: 44rem; }
  .tags { display: flex; flex-wrap: wrap; gap: 0.4rem 0.9rem; padding: 0; margin: 0; list-style: none; font-family: var(--font-mono); font-size: 0.76rem; color: var(--accent); }
  .more { margin-top: 1rem; font-family: var(--font-mono); font-size: 0.82rem; color: var(--muted); }
  .more a { color: var(--accent); border-bottom: 1px solid currentColor; }

  /* credentials */
  .subhead { font-family: var(--font-mono); font-size: 0.74rem; text-transform: uppercase; letter-spacing: 0.12em; color: var(--muted); margin-bottom: 0.75rem; }
  .certs { list-style: none; margin: 0 0 2.5rem; padding: 0; }
  .certs li { display: flex; flex-wrap: wrap; justify-content: space-between; gap: 0.2rem 1.5rem; padding-block: 0.95rem; border-top: 1px solid var(--line); }
  .certs li:last-child { border-bottom: 1px solid var(--line); }
  .certs strong { font-weight: 600; }
  .certs span { font-family: var(--font-mono); font-size: 0.8rem; color: var(--muted); }
  .table-scroll { overflow-x: auto; }
  table { width: 100%; min-width: 560px; border-collapse: collapse; font-size: 0.95rem; }
  th { text-align: left; font-family: var(--font-mono); font-weight: 400; font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.1em; color: var(--muted); padding: 0.5rem 1rem 0.5rem 0; border-bottom: 1px solid var(--line); }
  td { padding: 0.85rem 1rem 0.85rem 0; border-bottom: 1px solid var(--line); vertical-align: top; }
  td.num, th.num { font-variant-numeric: tabular-nums; white-space: nowrap; }
  tr:first-child td { font-weight: 500; }

  /* learning */
  .learn { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 1rem; list-style: none; margin: 0; padding: 0; }
  .learn li { background: var(--surface); border: 1px solid var(--line); border-radius: 12px; padding: 1.1rem 1.2rem; min-width: 0; }
  .learn h3 { font-family: var(--font-display); font-weight: 600; font-size: 1.1rem; letter-spacing: -0.01em; }
  .learn p { margin-top: 0.35rem; color: var(--muted); font-size: 0.92rem; }

  /* contact */
  .contact h3 { font-family: var(--font-display); font-weight: 700; font-size: clamp(2rem, 6vw, 3.6rem); line-height: 1; letter-spacing: -0.03em; }
  .mail { margin-top: 1.5rem; display: flex; flex-wrap: wrap; align-items: center; gap: 0.75rem 1rem; }
  .mail code { font-family: var(--font-mono); font-size: clamp(0.95rem, 3.2vw, 1.15rem); padding: 0.45rem 0.8rem; background: var(--surface); border: 1px solid var(--line); border-radius: 8px; user-select: all; overflow-wrap: anywhere; }
  .foot { padding-block: 2rem 2.5rem; border-top: 1px solid var(--line); display: flex; flex-wrap: wrap; justify-content: space-between; gap: 0.5rem 1.5rem; font-family: var(--font-mono); font-size: 0.76rem; color: var(--muted); }

  @media (prefers-reduced-motion: reduce) {
    html { scroll-behavior: auto; }
    .project h3 a { transition: none; }
  }
</style>

<header class="nav">
  <div class="wrap">
    <a class="brand" href="#top">praveen<span>.</span>raj</a>
    <div class="nav-end">
      <nav class="links" aria-label="Sections">
        <a href="#about">About</a>
        <a href="#stack">Stack</a>
        <a href="#work">Projects</a>
        <a href="#credentials">Credentials</a>
        <a href="#contact">Contact</a>
      </nav>
      <button class="icon-btn" id="theme" type="button" aria-label="Switch colour theme" title="Switch colour theme">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><circle cx="12" cy="12" r="4"/><path d="M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4"/></svg>
      </button>
    </div>
  </div>
</header>

<main id="top">
  <section class="hero wrap">
    <p class="eyebrow"><b>Chennai, Tamil Nadu</b><span>B.Tech CSE, SRM IST</span><span>Expected 2027</span></p>
    <h1>Praveen Raj</h1>
    <p class="role">Full stack developer (MERN) and ML enthusiast</p>
    <p class="tagline">I turn network jitter into <em>predictions</em> and campus placements into <em>pipelines</em>.</p>
    <div class="cta">
      <a class="btn primary" href="#work">See projects</a>
      <a class="btn" href="https://github.com/pravynraj" target="_blank" rel="noopener">GitHub: pravynraj</a>
      <a class="btn" href="#contact">Get in touch</a>
    </div>

    <figure class="trace-card" style="margin-inline:0;margin-bottom:0">
      <div class="trace-head">
        <span>latency_ms / live simulation</span>
        <span class="legend"><span class="raw"><i></i>raw jitter</span><span class="fc"><i></i>forecast</span></span>
      </div>
      <canvas id="trace" role="img" aria-label="Animated chart of noisy network latency with a smoothed forecast line extending ahead of the current moment"></canvas>
    </figure>
  </section>

  <div class="wrap">
    <section class="sec" id="about">
      <h2>About</h2>
      <div>
        <p class="motto">Build. Break. <span>Learn.</span> Repeat.</p>
        <div class="about-grid">
          <dl class="facts">
            <div class="fact"><dt>Based in</dt><dd>Chennai, Tamil Nadu, India</dd></div>
            <div class="fact"><dt>Studying</dt><dd>B.Tech, Computer Science &amp; Engineering</dd></div>
            <div class="fact"><dt>College</dt><dd>SRM Institute of Science and Technology</dd></div>
            <div class="fact"><dt>Graduating</dt><dd>2027 (expected)</dd></div>
          </dl>
<pre class="code" tabindex="0"><span class="k">const</span> praveenRaj = {
  name: <span class="s">"Praveen Raj"</span>,
  stack: [<span class="s">"JavaScript"</span>, <span class="s">"C++"</span>, <span class="s">"Python"</span>, <span class="s">"ReactJS"</span>,
          <span class="s">"NodeJS"</span>, <span class="s">"ExpressJS"</span>, <span class="s">"MongoDB"</span>],
  currentlyLearning: [<span class="s">"Deep Learning (LSTM)"</span>, <span class="s">"AWS Cloud"</span>, <span class="s">"System Design"</span>],
  funFact: <span class="s">"I turn network jitter into predictions and campus placements into pipelines"</span>,
  motto: () =&gt; <span class="s">"Build. Break. Learn. Repeat."</span>
};</pre>
        </div>
      </div>
    </section>

    <section class="sec" id="stack">
      <h2>Stack</h2>
      <div class="stack">
        <div class="group"><h3>Languages</h3><ul class="chips"><li>C++</li><li>C</li><li>JavaScript</li><li>Python</li></ul></div>
        <div class="group"><h3>Frontend</h3><ul class="chips"><li>HTML5</li><li>CSS3</li><li>Tailwind CSS</li><li>Bootstrap</li><li>Material UI</li><li>React</li><li>Redux</li></ul></div>
        <div class="group"><h3>Backend and data</h3><ul class="chips"><li>Node.js</li><li>Express</li><li>MongoDB</li><li>Firebase</li><li>SQL</li></ul></div>
        <div class="group"><h3>Cloud and tools</h3><ul class="chips"><li>AWS</li><li>Git</li><li>GitHub</li></ul></div>
      </div>
    </section>

    <section class="sec" id="work">
      <h2>Projects</h2>
      <div>
        <div class="projects">
          <article class="project">
            <h3><a href="https://github.com/pravynraj/ai-observability-platform" target="_blank" rel="noopener">AI Assistant Observability &amp; Evaluation Platform</a></h3>
            <p>A full-stack observability platform with automatic trace-ID generation, real-time latency, token and cost tracking, and a live dashboard for monitoring AI assistant requests end to end.</p>
            <ul class="tags"><li>Python</li><li>FastAPI</li><li>React</li><li>TypeScript</li><li>PostgreSQL</li><li>Docker</li></ul>
          </article>
          <article class="project">
            <h3><a href="https://github.com/pravynraj" target="_blank" rel="noopener">Secure Multi-Tenant RAG</a></h3>
            <p>A multi-tenant retrieval-augmented generation API with tenant-isolated vector retrieval, role-based ACL validation, JWT authentication and retrieval auditing.</p>
            <ul class="tags"><li>Python</li><li>FastAPI</li><li>PostgreSQL</li><li>Qdrant</li><li>JWT</li><li>Docker</li></ul>
          </article>
          <article class="project">
            <h3><a href="https://github.com/pravynraj" target="_blank" rel="noopener">Quantum Key Distribution under Eavesdropping</a></h3>
            <p>A Python simulation of BB84 quantum key distribution that analyses how eavesdropping affects communication security.</p>
            <ul class="tags"><li>Qiskit</li><li>Python</li><li>Streamlit</li><li>Matplotlib</li><li>Quantum cryptography</li></ul>
          </article>
        </div>
        <p class="more">More on <a href="https://github.com/pravynraj" target="_blank" rel="noopener">github.com/pravynraj</a></p>
      </div>
    </section>

    <section class="sec" id="credentials">
      <h2>Credentials</h2>
      <div>
        <p class="subhead">Certifications</p>
        <ul class="certs">
          <li><strong>AWS Certified Cloud Practitioner</strong><span>Amazon Web Services, Apr 2026</span></li>
          <li><strong>AWS Solution Architect</strong><span>Amazon Web Services, 2026</span></li>
          <li><strong>Oracle Certified Foundations Associate</strong><span>Oracle, 2025</span></li>
        </ul>
        <p class="subhead">Education</p>
        <div class="table-scroll">
          <table>
            <thead><tr><th>Degree</th><th>Institution</th><th class="num">Year</th><th class="num">Score</th></tr></thead>
            <tbody>
              <tr><td>B.Tech, Computer Science &amp; Engineering</td><td>SRM Institute of Science and Technology, Chennai</td><td class="num">Expected 2027</td><td class="num">CGPA 7.5</td></tr>
              <tr><td>Class XII</td><td>Velammal Bodhi Residential School</td><td class="num">2022</td><td class="num">72%</td></tr>
              <tr><td>Class X</td><td>Velammal Bodhi Residential School</td><td class="num">2020</td><td class="num">80%</td></tr>
            </tbody>
          </table>
        </div>
      </div>
    </section>

    <section class="sec" id="learning">
      <h2>Learning now</h2>
      <ul class="learn">
        <li><h3>Deep learning</h3><p>LSTM, time-series forecasting, model optimization</p></li>
        <li><h3>Cloud and DevOps</h3><p>AWS services, deployment pipelines</p></li>
        <li><h3>System design</h3><p>Scalable backend architecture</p></li>
        <li><h3>Advanced MERN</h3><p>Performance, state management at scale</p></li>
      </ul>
    </section>

    <section class="sec contact" id="contact">
      <h2>Contact</h2>
      <div>
        <h3>Have something to build?</h3>
        <div class="mail">
          <code id="email">PR186360@gmail.com</code>
          <button class="btn primary" id="copy" type="button">Copy email</button>
          <a class="btn" href="https://github.com/pravynraj" target="_blank" rel="noopener">GitHub</a>
        </div>
      </div>
    </section>

    <footer class="foot">
      <span>Praveen Raj, Chennai</span>
      <span>Build. Break. Learn. Repeat.</span>
    </footer>
  </div>
</main>

<script>
(function () {
  var root = document.documentElement;

  /* theme toggle */
  var themeBtn = document.getElementById('theme');
  themeBtn.addEventListener('click', function () {
    var dark = root.getAttribute('data-theme') === 'dark' ||
      (!root.getAttribute('data-theme') && !matchMedia('(prefers-color-scheme: light)').matches);
    root.setAttribute('data-theme', dark ? 'light' : 'dark');
    drawStatic();
  });

  /* copy email */
  var copyBtn = document.getElementById('copy');
  var emailEl = document.getElementById('email');
  copyBtn.addEventListener('click', function () {
    var text = emailEl.textContent;
    function selectIt() {
      var r = document.createRange(); r.selectNodeContents(emailEl);
      var s = window.getSelection(); s.removeAllRanges(); s.addRange(r);
      copyBtn.textContent = 'Selected, press copy';
    }
    function done() { copyBtn.textContent = 'Copied'; setTimeout(function () { copyBtn.textContent = 'Copy email'; }, 1800); }
    try { navigator.clipboard.writeText(text).then(done, selectIt); } catch (e) { selectIt(); }
  });

  /* latency trace */
  var cv = document.getElementById('trace');
  var ctx = cv.getContext('2d');
  var reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
  var N = 120, F = 28, T = N + F, ALPHA = 0.13;
  var raw = [], ema = [], tick = 0, W = 0, H = 0, dpr = 1;

  function sample(t) {
    var v = 52 + 14 * Math.sin(t * 0.045) + 7 * Math.sin(t * 0.13 + 1.3) + (Math.random() - 0.5) * 11;
    if (Math.random() < 0.045) v += 16 + Math.random() * 30;
    return v;
  }
  function push() {
    var v = sample(tick++);
    var prev = ema.length ? ema[ema.length - 1] : v;
    raw.push(v); ema.push(prev + ALPHA * (v - prev));
    if (raw.length > N) { raw.shift(); ema.shift(); }
  }
  for (var i = 0; i < N; i++) push();

  function css(name) { return getComputedStyle(root).getPropertyValue(name).trim(); }

  function resize() {
    dpr = Math.min(window.devicePixelRatio || 1, 2);
    W = cv.clientWidth; H = cv.clientHeight;
    cv.width = Math.round(W * dpr); cv.height = Math.round(H * dpr);
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  }

  function draw() {
    if (!W || !H) return;
    var accent = css('--accent'), muted = css('--muted'), line = css('--line'), signal = css('--signal');
    var padT = 16, padB = 22, padL = 40, padR = 12;
    var plotW = W - padL - padR, plotH = H - padT - padB;
    var lo = 20, hi = 110;
    function X(i) { return padL + (i / (T - 1)) * plotW; }
    function Y(v) { return padT + plotH - ((v - lo) / (hi - lo)) * plotH; }
    ctx.clearRect(0, 0, W, H);
    ctx.font = '11px "JetBrains Mono", ui-monospace, monospace';
    ctx.textBaseline = 'middle';

    /* grid + y labels */
    ctx.lineWidth = 1;
    [30, 50, 70, 90, 110].forEach(function (g) {
      ctx.strokeStyle = line; ctx.globalAlpha = 0.7;
      ctx.beginPath(); ctx.moveTo(padL, Y(g) + 0.5); ctx.lineTo(W - padR, Y(g) + 0.5); ctx.stroke();
      ctx.globalAlpha = 1; ctx.fillStyle = muted; ctx.textAlign = 'right';
      ctx.fillText(String(g), padL - 8, Y(g));
    });

    /* future region tint */
    var nowX = X(N - 1);
    ctx.globalAlpha = 0.07; ctx.fillStyle = accent;
    ctx.fillRect(nowX, padT, W - padR - nowX, plotH);
    ctx.globalAlpha = 1;

    /* raw jitter */
    ctx.strokeStyle = muted; ctx.lineWidth = 1; ctx.globalAlpha = 0.85;
    ctx.beginPath();
    raw.forEach(function (v, i) { if (i) ctx.lineTo(X(i), Y(v)); else ctx.moveTo(X(i), Y(v)); });
    ctx.stroke(); ctx.globalAlpha = 1;

    /* smoothed history */
    ctx.strokeStyle = accent; ctx.lineWidth = 2;
    ctx.beginPath();
    ema.forEach(function (v, i) { if (i) ctx.lineTo(X(i), Y(v)); else ctx.moveTo(X(i), Y(v)); });
    ctx.stroke();

    /* forecast with widening band */
    var last = ema[N - 1], slope = (ema[N - 1] - ema[N - 9]) / 8;
    var fc = [];
    for (var k = 1; k <= F; k++) fc.push(last + slope * 10 * (1 - Math.exp(-k / 10)));
    ctx.globalAlpha = 0.16; ctx.fillStyle = accent;
    ctx.beginPath(); ctx.moveTo(nowX, Y(last));
    fc.forEach(function (v, j) { ctx.lineTo(X(N + j), Y(v + 2 + (j + 1) * 0.65)); });
    for (var j = F - 1; j >= 0; j--) ctx.lineTo(X(N + j), Y(fc[j] - 2 - (j + 1) * 0.65));
    ctx.closePath(); ctx.fill(); ctx.globalAlpha = 1;
    ctx.strokeStyle = accent; ctx.lineWidth = 2; ctx.setLineDash([5, 4]);
    ctx.beginPath(); ctx.moveTo(nowX, Y(last));
    fc.forEach(function (v, j) { ctx.lineTo(X(N + j), Y(v)); });
    ctx.stroke(); ctx.setLineDash([]);

    /* now marker */
    ctx.strokeStyle = signal; ctx.lineWidth = 1; ctx.setLineDash([2, 3]);
    ctx.beginPath(); ctx.moveTo(nowX + 0.5, padT); ctx.lineTo(nowX + 0.5, padT + plotH); ctx.stroke(); ctx.setLineDash([]);
    ctx.fillStyle = signal; ctx.beginPath(); ctx.arc(nowX, Y(last), 3.5, 0, Math.PI * 2); ctx.fill();
    ctx.textAlign = 'right'; ctx.fillText('now', nowX - 6, padT + 8);
    ctx.textAlign = 'left'; ctx.fillStyle = accent; ctx.fillText('forecast', nowX + 8, padT + 8);
    ctx.fillStyle = muted; ctx.textAlign = 'left'; ctx.fillText('ms', 6, padT + 2);
  }

  function drawStatic() { draw(); }

  var last = 0, acc = 0;
  function loop(ts) {
    var dt = Math.min(ts - last, 200); last = ts; acc += dt;
    var stepped = false;
    while (acc >= 110) { push(); acc -= 110; stepped = true; }
    if (stepped) draw();
    requestAnimationFrame(loop);
  }

  if (window.ResizeObserver) new ResizeObserver(function () { resize(); draw(); }).observe(cv);
  else window.addEventListener('resize', function () { resize(); draw(); });
  matchMedia('(prefers-color-scheme: light)').addEventListener('change', drawStatic);
  resize(); draw();
  if (!reduce) requestAnimationFrame(function (ts) { last = ts; requestAnimationFrame(loop); });
})();
</script>
