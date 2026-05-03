<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Visual T&R Navigation · JetRacer · CVPR 2026</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Bebas+Neue&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #080c10;
    --surface: #0d1520;
    --border: #1a2d45;
    --accent: #00d4ff;
    --accent2: #ff6b35;
    --accent3: #39ff14;
    --text: #c8dae8;
    --muted: #4a6278;
    --heading: #e8f4ff;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 16px;
    line-height: 1.7;
    overflow-x: hidden;
  }

  /* GRID BACKGROUND */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* NAV */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 2.5rem;
    height: 60px;
    background: rgba(8,12,16,0.85);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
  }
  nav .logo {
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    color: var(--accent);
    letter-spacing: 0.1em;
  }
  nav ul {
    list-style: none;
    display: flex;
    gap: 2rem;
  }
  nav ul a {
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem;
    color: var(--muted);
    text-decoration: none;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    transition: color 0.2s;
  }
  nav ul a:hover { color: var(--accent); }

  /* HERO */
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 120px 2rem 80px;
    overflow: hidden;
  }

  .hero-glow {
    position: absolute;
    width: 700px; height: 700px;
    background: radial-gradient(circle, rgba(0,212,255,0.08) 0%, transparent 70%);
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    pointer-events: none;
  }

  .conference-badge {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background: rgba(0,212,255,0.08);
    border: 1px solid rgba(0,212,255,0.25);
    border-radius: 2px;
    padding: 0.4rem 1rem;
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem;
    letter-spacing: 0.15em;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 2.5rem;
    animation: fadeUp 0.6s ease both;
  }
  .conference-badge::before {
    content: '';
    width: 6px; height: 6px;
    background: var(--accent);
    border-radius: 50%;
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.4; transform: scale(0.8); }
  }

  h1 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(3.5rem, 9vw, 8rem);
    color: var(--heading);
    line-height: 0.95;
    letter-spacing: 0.02em;
    margin-bottom: 0.5rem;
    animation: fadeUp 0.6s 0.1s ease both;
  }

  h1 em {
    font-style: normal;
    color: var(--accent);
  }

  .subtitle {
    font-family: 'Space Mono', monospace;
    font-size: 0.85rem;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 2.5rem;
    animation: fadeUp 0.6s 0.2s ease both;
  }

  .authors {
    display: flex;
    gap: 2.5rem;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 3rem;
    animation: fadeUp 0.6s 0.3s ease both;
  }
  .author {
    text-align: center;
  }
  .author-name {
    font-weight: 500;
    color: var(--heading);
    font-size: 0.95rem;
  }
  .author-roll {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--muted);
    letter-spacing: 0.05em;
    margin-top: 2px;
  }

  .hero-links {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    justify-content: center;
    animation: fadeUp 0.6s 0.4s ease both;
  }
  .btn {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.7rem 1.6rem;
    border-radius: 2px;
    font-family: 'Space Mono', monospace;
    font-size: 0.72rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    text-decoration: none;
    transition: all 0.2s;
    cursor: pointer;
    border: none;
  }
  .btn-primary {
    background: var(--accent);
    color: #000;
  }
  .btn-primary:hover {
    background: #fff;
  }
  .btn-outline {
    background: transparent;
    color: var(--text);
    border: 1px solid var(--border);
  }
  .btn-outline:hover {
    border-color: var(--accent);
    color: var(--accent);
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }

  /* SECTIONS */
  section {
    position: relative;
    z-index: 1;
    max-width: 1100px;
    margin: 0 auto;
    padding: 100px 2rem;
  }

  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
    max-width: 60px;
  }

  h2 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(2.5rem, 5vw, 4rem);
    color: var(--heading);
    line-height: 1;
    margin-bottom: 2rem;
    letter-spacing: 0.02em;
  }

  h3 {
    font-family: 'Space Mono', monospace;
    font-size: 0.85rem;
    color: var(--accent);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 0.75rem;
  }

  p { color: var(--text); margin-bottom: 1rem; }

  /* ABSTRACT */
  .abstract-box {
    background: var(--surface);
    border: 1px solid var(--border);
    border-left: 3px solid var(--accent);
    padding: 2.5rem;
    border-radius: 0 4px 4px 0;
    font-size: 1.05rem;
    line-height: 1.85;
    color: var(--text);
  }

  /* PIPELINE */
  .pipeline-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
    gap: 1.5rem;
    margin-top: 3rem;
  }

  .pipeline-card {
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 2rem;
    border-radius: 4px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s, transform 0.3s;
  }
  .pipeline-card:hover {
    border-color: var(--accent);
    transform: translateY(-4px);
  }
  .pipeline-card::before {
    content: attr(data-num);
    position: absolute;
    top: -10px; right: 10px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 5rem;
    color: rgba(0,212,255,0.05);
    line-height: 1;
    pointer-events: none;
  }
  .pipeline-card .card-icon {
    font-size: 1.6rem;
    margin-bottom: 1rem;
  }
  .pipeline-card .card-title {
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    color: var(--accent);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 0.5rem;
  }
  .pipeline-card p {
    font-size: 0.88rem;
    color: var(--muted);
    margin: 0;
  }

  /* TECH SPECS */
  .specs-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
    margin-top: 3rem;
  }
  @media (max-width: 640px) { .specs-grid { grid-template-columns: 1fr; } }

  .spec-block {
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 1.75rem;
    border-radius: 4px;
  }
  .spec-block h3 { margin-bottom: 1rem; }
  .spec-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem 0;
    border-bottom: 1px solid var(--border);
    font-size: 0.85rem;
  }
  .spec-row:last-child { border-bottom: none; }
  .spec-key { color: var(--muted); font-family: 'Space Mono', monospace; font-size: 0.72rem; }
  .spec-val { color: var(--heading); font-weight: 500; }
  .spec-val.accent { color: var(--accent); font-family: 'Space Mono', monospace; }

  /* ARCHITECTURE FLOW */
  .flow {
    display: flex;
    align-items: center;
    gap: 0;
    overflow-x: auto;
    padding: 2rem 0;
    margin-top: 2rem;
  }
  .flow-node {
    flex-shrink: 0;
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 1rem 1.25rem;
    border-radius: 4px;
    text-align: center;
    min-width: 130px;
    transition: border-color 0.3s;
  }
  .flow-node:hover { border-color: var(--accent); }
  .flow-node .fn-icon { font-size: 1.25rem; margin-bottom: 0.4rem; }
  .flow-node .fn-title {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--accent);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 0.2rem;
  }
  .flow-node .fn-sub { font-size: 0.72rem; color: var(--muted); }
  .flow-arrow {
    flex-shrink: 0;
    width: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--muted);
    font-size: 1.2rem;
  }

  /* TIER CASCADE */
  .tiers {
    margin-top: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1px;
  }
  .tier {
    display: grid;
    grid-template-columns: 80px 1fr auto;
    align-items: center;
    gap: 1.5rem;
    padding: 1.1rem 1.5rem;
    background: var(--surface);
    border: 1px solid var(--border);
    transition: border-color 0.3s;
  }
  .tier:first-child { border-radius: 4px 4px 0 0; }
  .tier:last-child { border-radius: 0 0 4px 4px; }
  .tier:hover { border-color: var(--accent); z-index: 1; }
  .tier-num {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 2rem;
    color: rgba(0,212,255,0.3);
  }
  .tier-title {
    font-family: 'Space Mono', monospace;
    font-size: 0.78rem;
    color: var(--heading);
    margin-bottom: 0.2rem;
  }
  .tier-desc { font-size: 0.82rem; color: var(--muted); }
  .tier-condition {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--accent2);
    text-align: right;
    white-space: nowrap;
  }

  /* METRICS */
  .metrics-row {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 1.5rem;
    margin-top: 3rem;
  }
  .metric-card {
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 1.75rem;
    border-radius: 4px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .metric-card::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
  }
  .metric-val {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 3rem;
    color: var(--accent);
    line-height: 1;
    margin-bottom: 0.5rem;
  }
  .metric-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  /* RESOURCE CHARTS */
  .charts-area {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 1.5rem;
    margin-top: 3rem;
  }
  .chart-card {
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 1.5rem;
    border-radius: 4px;
  }
  .chart-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.25rem;
  }
  .chart-title {
    font-family: 'Space Mono', monospace;
    font-size: 0.72rem;
    color: var(--accent);
    text-transform: uppercase;
    letter-spacing: 0.1em;
  }
  .chart-phase {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--muted);
  }
  .bar-group { margin-bottom: 0.75rem; }
  .bar-label {
    display: flex;
    justify-content: space-between;
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--muted);
    margin-bottom: 4px;
  }
  .bar-track {
    height: 8px;
    background: rgba(255,255,255,0.05);
    border-radius: 2px;
    overflow: hidden;
  }
  .bar-fill {
    height: 100%;
    border-radius: 2px;
    transition: width 1s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  }
  .bar-cpu { background: var(--accent); }
  .bar-ram { background: var(--accent2); }
  .bar-gpu { background: var(--accent3); }

  /* FAILURE CASES */
  .failure-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
    margin-top: 3rem;
  }
  @media (max-width: 640px) { .failure-grid { grid-template-columns: 1fr; } }

  .failure-card {
    background: var(--surface);
    border: 1px solid rgba(255,107,53,0.2);
    padding: 1.75rem;
    border-radius: 4px;
    border-left: 3px solid var(--accent2);
  }
  .failure-card h3 { color: var(--accent2); }
  .failure-card p { font-size: 0.88rem; color: var(--muted); }

  /* TEAM */
  .team-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 1.5rem;
    margin-top: 3rem;
  }
  .team-card {
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 2rem;
    border-radius: 4px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s;
  }
  .team-card:hover { border-color: var(--accent); }
  .team-avatar {
    width: 52px; height: 52px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.5rem;
    color: #000;
    margin-bottom: 1.25rem;
  }
  .team-name {
    font-weight: 600;
    color: var(--heading);
    font-size: 1rem;
    margin-bottom: 0.2rem;
  }
  .team-roll {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    color: var(--accent);
    letter-spacing: 0.1em;
    margin-bottom: 1rem;
  }
  .team-contrib {
    font-size: 0.83rem;
    color: var(--muted);
    line-height: 1.6;
  }
  .contrib-tag {
    display: inline-block;
    background: rgba(0,212,255,0.08);
    border: 1px solid rgba(0,212,255,0.2);
    border-radius: 2px;
    padding: 0.15rem 0.5rem;
    font-family: 'Space Mono', monospace;
    font-size: 0.62rem;
    color: var(--accent);
    margin: 0.2rem 0.2rem 0 0;
    white-space: nowrap;
  }

  /* EQUATION */
  .eq-block {
    background: rgba(0,0,0,0.4);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 1.25rem 2rem;
    margin: 1.5rem 0;
    font-family: 'Space Mono', monospace;
    font-size: 0.9rem;
    color: var(--accent3);
    text-align: center;
    overflow-x: auto;
  }

  /* FOOTER */
  footer {
    position: relative;
    z-index: 1;
    border-top: 1px solid var(--border);
    padding: 3rem 2rem;
    text-align: center;
  }
  footer .footer-badge {
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem;
    color: var(--muted);
    letter-spacing: 0.1em;
  }
  footer .footer-badge span { color: var(--accent); }

  /* DIVIDER */
  .divider {
    border: none;
    border-top: 1px solid var(--border);
    margin: 0;
  }

  /* SCROLL REVEAL */
  .reveal {
    opacity: 0;
    transform: translateY(24px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: none;
  }

  /* NEXT STEPS LIST */
  .steps-list {
    margin-top: 2rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  .step-item {
    display: flex;
    gap: 1.25rem;
    align-items: flex-start;
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 1.25rem 1.5rem;
    border-radius: 4px;
    transition: border-color 0.3s;
  }
  .step-item:hover { border-color: var(--accent3); }
  .step-dot {
    width: 8px; height: 8px;
    background: var(--accent3);
    border-radius: 50%;
    flex-shrink: 0;
    margin-top: 0.55rem;
  }
  .step-text { font-size: 0.9rem; color: var(--text); }
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <div class="logo">VTR // JetRacer // CVPR 2026</div>
  <ul>
    <li><a href="#abstract">Abstract</a></li>
    <li><a href="#system">System</a></li>
    <li><a href="#results">Results</a></li>
    <li><a href="#team">Team</a></li>
  </ul>
</nav>

<!-- HERO -->
<div class="hero">
  <div class="hero-glow"></div>

  <div class="conference-badge">CVPR 2026 &nbsp;·&nbsp; Paper #1234 &nbsp;·&nbsp; Confidential Review</div>

  <h1>Visual <em>Teach</em><br>&amp; Repeat</h1>
  <div class="subtitle">Navigation on Edge Devices using JetRacer</div>

  <div class="authors">
    <div class="author">
      <div class="author-name">Rudra Narnoli</div>
      <div class="author-roll">Roll #2024485</div>
    </div>
    <div class="author">
      <div class="author-name">Ashutosh Bhardwaj</div>
      <div class="author-roll">Roll #2024135</div>
    </div>
    <div class="author">
      <div class="author-name">Anurag</div>
      <div class="author-roll">Roll #2023112</div>
    </div>
  </div>

  <div class="hero-links">
    <a class="btn btn-primary" href="#abstract">↓ Read Paper</a>
    <a class="btn btn-outline" href="#system">System Architecture</a>
    <a class="btn btn-outline" href="#team">Contributors</a>
  </div>
</div>

<hr class="divider">

<!-- ABSTRACT -->
<section id="abstract">
  <div class="section-label">01 &nbsp; Overview</div>
  <h2>Abstract</h2>
  <div class="abstract-box reveal">
    We present an engineering implementation of a vision-based teach-and-repeat navigation system deployed on an Ackermann-steered JetRacer platform. The system learns a route from human demonstration and later replays it autonomously using monocular visual input, odometry, and lightweight waypoint tracking. During the <strong>teach phase</strong>, spatially spaced waypoints are recorded together with pose and image data. During the <strong>repeat phase</strong>, a pose-file localiser publishes the next goal waypoint, which is tracked by an Ackermann-constrained Pure-Pursuit controller through ROS topic interfaces. The full pipeline is designed for real-time execution on resource-limited embedded hardware and is intended for structured indoor and semi-structured outdoor environments where GPS is unavailable and heavy SLAM pipelines are undesirable. Experimental observations show that waypoint density, frame consistency, and lookahead tuning strongly affect tracking stability and localization robustness.
  </div>

  <div class="metrics-row reveal">
    <div class="metric-card">
      <div class="metric-val">20Hz</div>
      <div class="metric-label">Control Loop Rate</div>
    </div>
    <div class="metric-card">
      <div class="metric-val">15Hz</div>
      <div class="metric-label">Visual Localisation</div>
    </div>
    <div class="metric-card">
      <div class="metric-val">4GB</div>
      <div class="metric-label">Jetson Nano RAM</div>
    </div>
    <div class="metric-card">
      <div class="metric-val">0.40</div>
      <div class="metric-label">Speed (m/s)</div>
    </div>
    <div class="metric-card">
      <div class="metric-val">5m</div>
      <div class="metric-label">Indoor Route Length</div>
    </div>
  </div>
</section>

<hr class="divider">

<!-- SYSTEM -->
<section id="system">
  <div class="section-label">02 &nbsp; Architecture</div>
  <h2>System Design</h2>

  <p class="reveal">The pipeline operates across four concurrent threads, covering image preprocessing, odometry integration, visual localisation, and pure-pursuit control. Each thread communicates via ROS topic interfaces, enabling real-time execution on the Jetson Nano.</p>

  <!-- FLOW -->
  <div class="flow reveal">
    <div class="flow-node">
      <div class="fn-icon">📷</div>
      <div class="fn-title">CSI Camera</div>
      <div class="fn-sub">/csi_cam_0/image_raw</div>
    </div>
    <div class="flow-arrow">→</div>
    <div class="flow-node">
      <div class="fn-icon">🖼️</div>
      <div class="fn-title">Preprocessor</div>
      <div class="fn-sub">320×240 · NCC Descriptor</div>
    </div>
    <div class="flow-arrow">→</div>
    <div class="flow-node">
      <div class="fn-icon">📍</div>
      <div class="fn-title">Localiser</div>
      <div class="fn-sub">15 Hz · Pose-file</div>
    </div>
    <div class="flow-arrow">→</div>
    <div class="flow-node">
      <div class="fn-icon">🎯</div>
      <div class="fn-title">Goal Waypoint</div>
      <div class="fn-sub">/goal · Pose2D</div>
    </div>
    <div class="flow-arrow">→</div>
    <div class="flow-node">
      <div class="fn-icon">🚗</div>
      <div class="fn-title">Pure Pursuit</div>
      <div class="fn-sub">Ackermann Controller</div>
    </div>
    <div class="flow-arrow">→</div>
    <div class="flow-node">
      <div class="fn-icon">⚙️</div>
      <div class="fn-title">JetRacer</div>
      <div class="fn-sub">/cmd_vel → I2C</div>
    </div>
  </div>

  <!-- 4 THREADS -->
  <h3 style="margin-top: 3rem;">Concurrent Thread Architecture</h3>
  <div class="pipeline-grid reveal">
    <div class="pipeline-card" data-num="1">
      <div class="card-icon">🖼️</div>
      <div class="card-title">Thread 1 — Image Preprocessing</div>
      <p>BGR → Grayscale → 320×240 px (INTER_AREA) → Gaussian normalization (σ=1.2) → top 20% crop → float32 NCC descriptor.</p>
    </div>
    <div class="pipeline-card" data-num="2">
      <div class="card-icon">📐</div>
      <div class="card-title">Thread 2 — Odometry Integration</div>
      <p>Unicycle Euler integration over /odom_raw twist. Produces /odom_combined and broadcasts odom→base_link TF.</p>
    </div>
    <div class="pipeline-card" data-num="3">
      <div class="card-icon">🔍</div>
      <div class="card-title">Thread 3 — Visual Localisation</div>
      <p>Four-step cascade at 15 Hz: dead-reckoning advance → NCC index search (window=10) → yaw correction → carrot projection for goal.</p>
    </div>
    <div class="pipeline-card" data-num="4">
      <div class="card-icon">🎮</div>
      <div class="card-title">Thread 4 — Pure-Pursuit Control</div>
      <p>Ackermann geometry with L=0.16 m, Ld=0.28 m, max steer=28°. Outputs /cmd_vel forwarded via I2C to servo and ESC.</p>
    </div>
  </div>

  <!-- EQUATIONS -->
  <h3 style="margin-top: 3rem;">Key Equations</h3>
  <div class="eq-block reveal">
    Odometry: &nbsp; x += v·cos(yaw)·dt &nbsp;|&nbsp; y += v·sin(yaw)·dt &nbsp;|&nbsp; yaw += ω·dt
  </div>
  <div class="eq-block reveal">
    Carrot Projection: &nbsp; g_x = r_x + L·cos(ψ) &nbsp;|&nbsp; g_y = r_y + L·sin(ψ) &nbsp;|&nbsp; L ∈ [0.20, 0.40] m
  </div>
  <div class="eq-block reveal">
    Pure-Pursuit: &nbsp; δ = arctan(2L·sin(α) / L_d) &nbsp;|&nbsp; ω = v·tan(δ) / L
  </div>

  <!-- YAW CASCADE -->
  <h3 style="margin-top: 3rem;">Yaw Correction Cascade</h3>
  <div class="tiers reveal">
    <div class="tier">
      <div class="tier-num">01</div>
      <div>
        <div class="tier-title">LK Hybrid — ORB Features + Partial Affine</div>
        <div class="tier-desc">Active when LK ORB keyframe match confidence is sufficient. Strongest correction signal.</div>
      </div>
      <div class="tier-condition">confidence ≥ 0.20</div>
    </div>
    <div class="tier">
      <div class="tier-num">02</div>
      <div>
        <div class="tier-title">NCC Phase — Pixel Shift → Yaw</div>
        <div class="tier-desc">Phase correlation yaw estimate. Good for texturally rich regions.</div>
      </div>
      <div class="tier-condition">NCC score ≥ 0.08</div>
    </div>
    <div class="tier">
      <div class="tier-num">03</div>
      <div>
        <div class="tier-title">LK Flow — Low-gain Heading Fallback</div>
        <div class="tier-desc">IIR-smoothed lateral optical flow. Last resort with signal present.</div>
      </div>
      <div class="tier-condition">lat. flow &gt; 0.5 px</div>
    </div>
    <div class="tier">
      <div class="tier-num">04</div>
      <div>
        <div class="tier-title">Blind Coast — No Correction</div>
        <div class="tier-desc">Robot coasts forward with no visual input. Final fallback.</div>
      </div>
      <div class="tier-condition">last resort</div>
    </div>
  </div>

  <!-- SPECS -->
  <h3 style="margin-top: 3rem;">Technical Specifications</h3>
  <div class="specs-grid reveal">
    <div class="spec-block">
      <h3>Platform</h3>
      <div class="spec-row"><span class="spec-key">Hardware</span><span class="spec-val">JetRacer (Ackermann)</span></div>
      <div class="spec-row"><span class="spec-key">Compute</span><span class="spec-val">Jetson Nano 4GB</span></div>
      <div class="spec-row"><span class="spec-key">Camera</span><span class="spec-val">CSI Monocular RGB</span></div>
      <div class="spec-row"><span class="spec-key">Sensing</span><span class="spec-val">IMU + Wheel Encoders</span></div>
      <div class="spec-row"><span class="spec-key">Middleware</span><span class="spec-val">ROS</span></div>
    </div>
    <div class="spec-block">
      <h3>Control Parameters</h3>
      <div class="spec-row"><span class="spec-key">Wheelbase (L)</span><span class="spec-val accent">0.16 m</span></div>
      <div class="spec-row"><span class="spec-key">Lookahead (Ld)</span><span class="spec-val accent">0.28 m</span></div>
      <div class="spec-row"><span class="spec-key">Speed</span><span class="spec-val accent">0.40 m/s</span></div>
      <div class="spec-row"><span class="spec-key">Max Steer</span><span class="spec-val accent">28°</span></div>
      <div class="spec-row"><span class="spec-key">Image Resolution</span><span class="spec-val accent">320 × 240 px</span></div>
    </div>
  </div>
</section>

<hr class="divider">

<!-- RESULTS -->
<section id="results">
  <div class="section-label">03 &nbsp; Evaluation</div>
  <h2>Results &amp; Analysis</h2>

  <p class="reveal">Experiments are conducted on indoor routes of approximately 5 m, with repeated teach and repeat trials on the JetRacer platform. The robot is evaluated under routine laboratory lighting and moderate motion speeds.</p>

  <!-- RESOURCE CHARTS -->
  <div class="charts-area reveal">
    <div class="chart-card">
      <div class="chart-header">
        <div class="chart-title">Resource Utilization</div>
        <div class="chart-phase">TEACH Phase</div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>CPU</span><span>~62%</span></div>
        <div class="bar-track"><div class="bar-fill bar-cpu" style="width:62%"></div></div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>RAM</span><span>~45%</span></div>
        <div class="bar-track"><div class="bar-fill bar-ram" style="width:45%"></div></div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>GPU</span><span>~10%</span></div>
        <div class="bar-track"><div class="bar-fill bar-gpu" style="width:10%"></div></div>
      </div>
    </div>

    <div class="chart-card">
      <div class="chart-header">
        <div class="chart-title">Resource Utilization</div>
        <div class="chart-phase">REPEAT Phase</div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>CPU</span><span>~78%</span></div>
        <div class="bar-track"><div class="bar-fill bar-cpu" style="width:78%"></div></div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>RAM</span><span>~50%</span></div>
        <div class="bar-track"><div class="bar-fill bar-ram" style="width:50%"></div></div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>GPU</span><span>~15%</span></div>
        <div class="bar-track"><div class="bar-fill bar-gpu" style="width:15%"></div></div>
      </div>
    </div>

    <div class="chart-card">
      <div class="chart-header">
        <div class="chart-title">Resource Utilization</div>
        <div class="chart-phase">IDLE Phase</div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>CPU</span><span>~20%</span></div>
        <div class="bar-track"><div class="bar-fill bar-cpu" style="width:20%"></div></div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>RAM</span><span>~30%</span></div>
        <div class="bar-track"><div class="bar-fill bar-ram" style="width:30%"></div></div>
      </div>
      <div class="bar-group">
        <div class="bar-label"><span>GPU</span><span>~5%</span></div>
        <div class="bar-track"><div class="bar-fill bar-gpu" style="width:5%"></div></div>
      </div>
    </div>
  </div>

  <!-- FAILURE CASES -->
  <h3 style="margin-top: 3.5rem;">Failure Modes</h3>
  <div class="failure-grid reveal">
    <div class="failure-card">
      <h3>Reflective Surfaces</h3>
      <p>Specular reflections corrupt the NCC descriptor, causing the localiser to lock onto incorrect keyframes and diverge from the intended path.</p>
    </div>
    <div class="failure-card">
      <h3>Moving Objects in Frame</h3>
      <p>Dynamic scene elements break the static-world assumption, degrading both NCC-based and optical-flow-based yaw correction.</p>
    </div>
    <div class="failure-card">
      <h3>Mechanical Slip</h3>
      <p>Lateral slip from insufficient traction during aggressive turns causes trajectory drift. The bicycle model assumption breaks under real-world tire friction and servo non-linearities.</p>
    </div>
    <div class="failure-card">
      <h3>Low-Texture Corridors</h3>
      <p>ORB-based pipeline loses inlier count in uniform corridor walls, making geometric verification unstable and reducing repeat reliability.</p>
    </div>
  </div>

  <!-- NEXT STEPS -->
  <h3 style="margin-top: 3.5rem;">Next Steps</h3>
  <div class="steps-list reveal">
    <div class="step-item">
      <div class="step-dot"></div>
      <div class="step-text">Stabilize waypoint progression and align teach/repeat coordinate conventions; reduce turn failures.</div>
    </div>
    <div class="step-item">
      <div class="step-dot"></div>
      <div class="step-text">Improve visual front-end through stronger preprocessing and more reliable feature extraction (SIFT, FLANN).</div>
    </div>
    <div class="step-item">
      <div class="step-dot"></div>
      <div class="step-text">Incorporate visual-inertial fusion so visual evidence corrects odometry rather than dominating it.</div>
    </div>
    <div class="step-item">
      <div class="step-dot"></div>
      <div class="step-text">Enable operation in dynamic environments with moving objects; sharpen turning capability for tighter curves.</div>
    </div>
    <div class="step-item">
      <div class="step-dot"></div>
      <div class="step-text">Systematic evaluation across multiple routes reporting completion rate, endpoint error, drift, and compute usage.</div>
    </div>
  </div>
</section>

<hr class="divider">

<!-- TEAM -->
<section id="team">
  <div class="section-label">04 &nbsp; Contributors</div>
  <h2>Team</h2>

  <div class="team-grid reveal">
    <div class="team-card">
      <div class="team-avatar">RN</div>
      <div class="team-name">Rudra Narnoli</div>
      <div class="team-roll">Roll #2024485</div>
      <div>
        <span class="contrib-tag">Naive QVPR</span>
        <span class="contrib-tag">Failure Analysis</span>
        <span class="contrib-tag">Evaluation Metrics</span>
      </div>
      <p class="team-contrib" style="margin-top:0.75rem;">Implemented the initial QVPR pipeline and fixed failure points caused by repetitive objects in the scene.</p>
    </div>
    <div class="team-card">
      <div class="team-avatar">AB</div>
      <div class="team-name">Ashutosh Bhardwaj</div>
      <div class="team-roll">Roll #2024135</div>
      <div>
        <span class="contrib-tag">Workspace Setup</span>
        <span class="contrib-tag">Camera Calibration</span>
        <span class="contrib-tag">ORB-based VTR</span>
        <span class="contrib-tag">LK Tracker</span>
      </div>
      <p class="team-contrib" style="margin-top:0.75rem;">Led the ORB-based VTR implementation and integrated LK Tracker improvements over the QVPR baseline.</p>
    </div>
    <div class="team-card">
      <div class="team-avatar">AN</div>
      <div class="team-name">Anurag</div>
      <div class="team-roll">Roll #2023112</div>
      <div>
        <span class="contrib-tag">CLAHE</span>
        <span class="contrib-tag">VIO Integration</span>
        <span class="contrib-tag">SIFT</span>
        <span class="contrib-tag">FLANN</span>
      </div>
      <p class="team-contrib" style="margin-top:0.75rem;">Enhanced QVPR with CLAHE contrast normalisation and conducted experiments with SIFT and FLANN-based matching.</p>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <p class="footer-badge">
    <span>CVPR 2026</span> · Submission #1234 · Confidential Review Copy · Do Not Distribute
  </p>
  <p class="footer-badge" style="margin-top:0.5rem; font-size:0.6rem;">
    Built with ROS · Python · OpenCV · Jetson Nano
  </p>
</footer>

<script>
  // Scroll reveal
  const reveals = document.querySelectorAll('.reveal');
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.classList.add('visible');
        observer.unobserve(e.target);
      }
    });
  }, { threshold: 0.1 });
  reveals.forEach(r => observer.observe(r));

  // Animate bars on scroll
  const bars = document.querySelectorAll('.bar-fill');
  const barObserver = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.style.width = e.target.style.width; // trigger reflow
        barObserver.unobserve(e.target);
      }
    });
  }, { threshold: 0.5 });
  bars.forEach(b => {
    const w = b.style.width;
    b.style.width = '0%';
    setTimeout(() => barObserver.observe(b), 100);
    b.dataset.target = w;
  });

  // Re-animate bars when chart cards come into view
  const chartCards = document.querySelectorAll('.chart-card');
  const chartObserver = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.querySelectorAll('.bar-fill').forEach(bar => {
          bar.style.width = bar.dataset.target || bar.style.width;
        });
        chartObserver.unobserve(e.target);
      }
    });
  }, { threshold: 0.3 });
  chartCards.forEach(c => chartObserver.observe(c));
</script>
</body>
</html>
