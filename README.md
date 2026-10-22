<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>BALA AI // J.A.R.V.I.S. Tactical Media Core</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;800&family=Rajdhani:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-dark: #030914;
      --bg-panel: rgba(6, 22, 46, 0.72);
      --cyan: #00f0ff;
      --cyan-dim: rgba(0, 240, 255, 0.25);
      --gold: #ffb703;
      --crimson: #e63946;
      --text-main: #d8f8ff;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background-color: var(--bg-dark);
      background-image: 
        radial-gradient(circle at 50% 50%, rgba(0, 240, 255, 0.08) 0%, transparent 70%),
        linear-gradient(rgba(0, 240, 255, 0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0, 240, 255, 0.04) 1px, transparent 1px);
      background-size: 100% 100%, 30px 30px, 30px 30px;
      color: var(--text-main);
      font-family: 'Rajdhani', sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
      display: flex;
      flex-direction: column;
    }

    /* Scanline Overlay */
    body::after {
      content: "";
      position: fixed;
      inset: 0;
      background: repeating-linear-gradient(
        0deg,
        rgba(0, 0, 0, 0.18) 0px,
        rgba(0, 0, 0, 0.18) 1px,
        transparent 1px,
        transparent 3px
      );
      pointer-events: none;
      z-index: 100;
    }

    /* Top HUD Header */
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 14px 28px;
      border-bottom: 2px solid var(--cyan);
      background: rgba(2, 12, 27, 0.9);
      box-shadow: 0 0 20px rgba(0, 240, 255, 0.2);
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 14px;
    }

    .mini-reactor {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      border: 2px dashed var(--cyan);
      box-shadow: 0 0 12px var(--cyan), inset 0 0 10px var(--cyan);
      animation: spin 8s linear infinite;
    }

    .brand h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: 1.5rem;
      letter-spacing: 3px;
      color: var(--cyan);
      text-shadow: 0 0 10px var(--cyan);
    }

    .brand span {
      color: var(--gold);
      font-size: 0.85rem;
      letter-spacing: 2px;
      display: block;
    }

    .hud-metrics {
      display: flex;
      gap: 24px;
      font-family: 'Orbitron', sans-serif;
      font-size: 0.85rem;
    }

    .metric span {
      color: var(--gold);
      font-weight: 700;
    }

    /* Main Grid Layout */
    .workspace {
      display: grid;
      grid-template-columns: 290px 1fr 320px;
      gap: 18px;
      padding: 18px;
      flex: 1;
    }

    .panel {
      background: var(--bg-panel);
      border: 1px solid var(--cyan-dim);
      position: relative;
      padding: 18px;
      backdrop-filter: blur(8px);
      box-shadow: inset 0 0 20px rgba(0, 240, 255, 0.06);
      display: flex;
      flex-direction: column;
      gap: 14px;
    }

    /* Corner HUD Brackets */
    .panel::before, .panel::after {
      content: "";
      position: absolute;
      width: 14px;
      height: 14px;
      border-color: var(--cyan);
      border-style: solid;
      pointer-events: none;
    }
    .panel::before {
      top: -1px;
      left: -1px;
      border-width: 2px 0 0 2px;
    }
    .panel::after {
      bottom: -1px;
      right: -1px;
      border-width: 0 2px 2px 0;
    }

    .panel-title {
      font-family: 'Orbitron', sans-serif;
      font-size: 0.95rem;
      letter-spacing: 2px;
      color: var(--gold);
      border-bottom: 1px solid var(--cyan-dim);
      padding-bottom: 8px;
      display: flex;
      justify-content: space-between;
    }

    /* Arc Reactor Centerpiece */
    .reactor-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 16px 0;
    }

    .arc-reactor {
      width: 130px;
      height: 130px;
      border-radius: 50%;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 0 30px rgba(0, 240, 255, 0.45), inset 0 0 25px rgba(0, 240, 255, 0.5);
      border: 3px solid var(--cyan);
    }

    .ring-1, .ring-2 {
      position: absolute;
      border-radius: 50%;
      border: 2px dashed var(--cyan);
    }

    .ring-1 {
      width: 102px;
      height: 102px;
      animation: spin 10s linear infinite;
    }

    .ring-2 {
      width: 72px;
      height: 72px;
      border-color: var(--gold);
      animation: spin-reverse 6s linear infinite;
    }

    .core-glow {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      background: radial-gradient(circle, #ffffff 10%, var(--cyan) 70%);
      box-shadow: 0 0 25px #00f0ff;
      animation: pulse 2s ease-in-out infinite;
    }

    /* Mode Selector Buttons */
    .mode-tabs {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }

    .hud-btn {
      background: rgba(0, 240, 255, 0.08);
      border: 1px solid var(--cyan);
      color: var(--cyan);
      font-family: 'Orbitron', sans-serif;
      padding: 11px 14px;
      font-size: 0.85rem;
      letter-spacing: 1.5px;
      cursor: pointer;
      transition: all 0.2s ease;
      text-transform: uppercase;
    }

    .hud-btn:hover, .hud-btn.active {
      background: var(--cyan);
      color: #020a14;
      box-shadow: 0 0 15px var(--cyan);
      font-weight: 700;
    }

    .hud-btn.execute {
      background: linear-gradient(90deg, rgba(230, 57, 70, 0.25), rgba(255, 183, 3, 0.25));
      border-color: var(--gold);
      color: var(--gold);
      font-size: 0.95rem;
      padding: 13px;
    }

    .hud-btn.execute:hover {
      background: var(--gold);
      color: #020a14;
      box-shadow: 0 0 20px var(--gold);
    }

    /* Inputs & Controls */
    label {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 1px;
      color: var(--cyan);
      display: block;
      margin-bottom: 5px;
    }

    textarea, select, input[type="text"] {
      width: 100%;
      background: rgba(1, 10, 22, 0.85);
      border: 1px solid var(--cyan-dim);
      color: var(--text-main);
      padding: 10px;
      font-family: 'Rajdhani', sans-serif;
      font-size: 1rem;
      outline: none;
    }

    textarea:focus, select:focus, input:focus {
      border-color: var(--cyan);
      box-shadow: 0 0 10px rgba(0, 240, 255, 0.3);
    }

    .param-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    /* Viewport Canvas */
    .viewport {
      flex: 1;
      border: 1px solid var(--cyan-dim);
      background: radial-gradient(circle, rgba(4, 24, 52, 0.7) 0%, rgba(1, 8, 18, 0.95) 100%);
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 330px;
      overflow: hidden;
    }

    #renderCanvas {
      width: 100%;
      height: 100%;
      display: block;
    }

    .viewport-overlay {
      position: absolute;
      top: 12px;
      left: 14px;
      font-family: 'Orbitron', sans-serif;
      font-size: 0.75rem;
      color: var(--cyan);
      pointer-events: none;
    }

    .timeline-bar {
      display: flex;
      align-items: center;
      gap: 10px;
      font-family: 'Orbitron', sans-serif;
      font-size: 0.75rem;
    }

    .progress-track {
      flex: 1;
      height: 8px;
      background: rgba(0, 240, 255, 0.12);
      border: 1px solid var(--cyan-dim);
      overflow: hidden;
    }

    .progress-fill {
      width: 0%;
      height: 100%;
      background: linear-gradient(90deg, var(--cyan), var(--gold));
      transition: width 0.2s ease;
    }

    /* Terminal Log */
    .terminal-log {
      flex: 1;
      background: rgba(1, 8, 18, 0.85);
      border: 1px solid var(--cyan-dim);
      padding: 12px;
      font-family: monospace;
      font-size: 0.85rem;
      overflow-y: auto;
      max-height: 360px;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .log-entry {
      line-height: 1.4;
      border-left: 2px solid var(--cyan);
      padding-left: 8px;
    }

    .log-entry.jarvis {
      border-color: var(--gold);
      color: #ffe8a3;
    }

    @keyframes spin { 100% { transform: rotate(360deg); } }
    @keyframes spin-reverse { 100% { transform: rotate(-360deg); } }
    @keyframes pulse {
      0%, 100% { transform: scale(1); opacity: 0.9; }
      50% { transform: scale(1.14); opacity: 1; }
    }

    @media (max-width: 1024px) {
      .workspace { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

  <header>
    <div class="brand">
      <div class="mini-reactor"></div>
      <div>
        <h1>BALA AI</h1>
        <span>MARK LXXXV // J.A.R.V.I.S. VISUAL SYNTHESIS CORE</span>
      </div>
    </div>
    <div class="hud-metrics">
      <div class="metric">CORE OUTPUT: <span id="powerReadout">99.8%</span></div>
      <div class="metric">NEURAL ENGINE: <span>ONLINE</span></div>
      <div class="metric">RENDER PROTOCOL: <span id="activeProtocol">HOLO-IMG</span></div>
    </div>
  </header>

  <main class="workspace">
    <!-- Left Column: Arc Core & Mode Diagnostics -->
    <section class="panel">
      <div class="panel-title">
        <span>ARC REACTOR TELEMETRY</span>
        <span>MK-85</span>
      </div>

      <div class="reactor-container">
        <div class="arc-reactor">
          <div class="ring-1"></div>
          <div class="ring-2"></div>
          <div class="core-glow" id="reactorGlow"></div>
        </div>
      </div>

      <div class="panel-title">
        <span>SYNTHESIS MODULE</span>
        <span>SELECT</span>
      </div>

      <div class="mode-tabs">
        <button class="hud-btn active" id="btnImageMode" onclick="switchMode('IMAGE')">IMAGE GEN</button>
        <button class="hud-btn" id="btnVideoMode" onclick="switchMode('VIDEO')">VIDEO GEN</button>
      </div>

      <div>
        <label>Preset Tactical Styles</label>
        <select id="stylePreset">
          <option value="stark">Stark Hologram Blueprint</option>
          <option value="cinema">8K Cinematic Armor Render</option>
          <option value="cyber">Neon Cybernetic Grid</option>
          <option value="thermal">Infrared Tactical Recon</option>
        </select>
      </div>

      <div class="param-grid">
        <div>
          <label>Aspect Ratio</label>
          <select id="aspectRatio">
            <option value="16:9">16:9 Widescreen</option>
            <option value="1:1">1:1 Tactical Square</option>
            <option value="9:16">9:16 Vertical HUD</option>
          </select>
        </div>
        <div>
          <label id="dynamicParamLabel">Sampling Steps</label>
          <select id="dynamicParam">
            <option value="50">50 Steps (Ultra)</option>
            <option value="30">30 Steps (Fast)</option>
          </select>
        </div>
      </div>
    </section>

    <!-- Center Column: Holographic Synthesis Viewport -->
    <section class="panel">
      <div class="panel-title">
        <span id="viewportHeader">HOLOGRAPHIC IMAGE SYNTHESIZER</span>
        <span id="renderStatus">STANDBY</span>
      </div>

      <div class="viewport">
        <div class="viewport-overlay" id="hudOverlayText">
          TARGET: AWAITING PROMPT INPUT // RES: 3840x2160
        </div>
        <canvas id="renderCanvas" width="720" height="400"></canvas>
      </div>

      <div class="timeline-bar">
        <span id="progressLabel">SYNTHESIS PROGRESS</span>
        <div class="progress-track">
          <div class="progress-fill" id="progressBar"></div>
        </div>
        <span id="progressPercent">0%</span>
      </div>

      <div>
        <label>Visual Directive Prompt</label>
        <textarea id="promptInput" rows="2" placeholder="Describe the image or multi-scene video sequence for Bala AI to synthesize...">Futuristic Mark-85 nano-tech armor schematics hovering in a high-tech workshop with golden energy sparks</textarea>
      </div>

      <button class="hud-btn execute" onclick="triggerSynthesis()">
        INITIALIZE BALA AI SYNTHESIS
      </button>
    </section>

    <!-- Right Column: J.A.R.V.I.S. Command Console -->
    <section class="panel">
      <div class="panel-title">
        <span>J.A.R.V.I.S. COMMS LOG</span>
        <span>VOICE / TEXT</span>
      </div>

      <div class="terminal-log" id="terminalLog">
        <div class="log-entry jarvis">
          [J.A.R.V.I.S.]: Welcome back, Sir. Bala AI visual and motion synthesis matrices are calibrated and awaiting your directive.
        </div>
        <div class="log-entry">
          [SYS]: GPU Tensor Clusters initialized at 3.8 TFLOPS.
        </div>
      </div>

      <div style="display:flex; gap:8px;">
        <input type="text" id="commandInput" placeholder="Issue command to J.A.R.V.I.S..." onkeydown="if(event.key==='Enter') sendCommand()" />
        <button class="hud-btn" onclick="sendCommand()">SEND</button>
      </div>
    </section>
  </main>

  <script>
    let currentMode = 'IMAGE';
    let animFrameId = null;
    const canvas = document.getElementById('renderCanvas');
    const ctx = canvas.getContext('2d');

    function logMessage(text, isJarvis = false) {
      const log = document.getElementById('terminalLog');
      const div = document.createElement('div');
      div.className = 'log-entry' + (isJarvis ? ' jarvis' : '');
      div.textContent = (isJarvis ? '[J.A.R.V.I.S.]: ' : '[USER]: ') + text;
      log.appendChild(div);
      log.scrollTop = log.scrollHeight;
    }

    function switchMode(mode) {
      currentMode = mode;
      document.getElementById('btnImageMode').classList.toggle('active', mode === 'IMAGE');
      document.getElementById('btnVideoMode').classList.toggle('active', mode === 'VIDEO');
      document.getElementById('activeProtocol').textContent = mode === 'IMAGE' ? 'HOLO-IMG' : 'KINETIC-VID';
      document.getElementById('viewportHeader').textContent = mode === 'IMAGE'
        ? 'HOLOGRAPHIC IMAGE SYNTHESIZER'
        : 'TEMPORAL MOTION VIDEO ENGINE';

      const paramLabel = document.getElementById('dynamicParamLabel');
      const paramSelect = document.getElementById('dynamicParam');
      if (mode === 'VIDEO') {
        paramLabel.textContent = 'Frame Rate & Duration';
        paramSelect.innerHTML = '<option>60 FPS // 10 Sec</option><option>120 FPS // 5 Sec</option>';
        logMessage('Switched to Temporal Video Synthesis mode, Sir. Motion vectors unlocked.', true);
      } else {
        paramLabel.textContent = 'Sampling Steps';
        paramSelect.innerHTML = '<option>50 Steps (Ultra)</option><option>30 Steps (Fast)</option>';
        logMessage('Holographic Still Image Synthesis engaged, Sir.', true);
      }
      drawIdleGrid();
    }

    function drawIdleGrid() {
      cancelAnimationFrame(animFrameId);
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.strokeStyle = 'rgba(0, 240, 255, 0.2)';
      ctx.lineWidth = 1;

      // Crosshair target
      const cx = canvas.width / 2;
      const cy = canvas.height / 2;
      ctx.beginPath();
      ctx.arc(cx, cy, 70, 0, Math.PI * 2);
      ctx.moveTo(cx - 100, cy);
      ctx.lineTo(cx + 100, cy);
      ctx.moveTo(cx, cy - 100);
      ctx.lineTo(cx, cy + 100);
      ctx.stroke();
    }

    function triggerSynthesis() {
      const prompt = document.getElementById('promptInput').value.trim() || 'Stark Armor Schematics';
      const style = document.getElementById('stylePreset').value;
      const bar = document.getElementById('progressBar');
      const pct = document.getElementById('progressPercent');
      const status = document.getElementById('renderStatus');

      status.textContent = 'SYNTHESIZING...';
      logMessage(`Initiating ${currentMode} generation for: "${prompt.slice(0, 48)}..."`, true);

      let progress = 0;
      const interval = setInterval(() => {
        progress += 5;
        bar.style.width = progress + '%';
        pct.textContent = progress + '%';

        if (progress >= 100) {
          clearInterval(interval);
          status.textContent = 'RENDER COMPLETE';
          document.getElementById('hudOverlayText').textContent =
            `MODE: ${currentMode} // STYLE: ${style.toUpperCase()} // PROMPT LOCKED`;
          logMessage(`${currentMode} synthesis complete, Sir. Displaying holographic output on main viewport.`, true);
          renderHologramOutput(prompt, style);
        }
      }, 90);
    }

    function renderHologramOutput(prompt, style) {
      cancelAnimationFrame(animFrameId);
      let tick = 0;
      const primaryColor = style === 'cinema' ? '#ffb703' : style === 'thermal' ? '#e63946' : '#00f0ff';

      function animateFrame() {
        ctx.fillStyle = 'rgba(2, 12, 26, 0.25)';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        const cx = canvas.width / 2;
        const cy = canvas.height / 2;

        // Rotating Holographic Rings
        ctx.save();
        ctx.translate(cx, cy);
        ctx.rotate(tick * 0.02);
        ctx.strokeStyle = primaryColor;
        ctx.lineWidth = 2;
        ctx.strokeRect(-90, -90, 180, 180);

        ctx.rotate(-tick * 0.04);
        ctx.strokeStyle = '#ffb703';
        ctx.beginPath();
        ctx.arc(0, 0, 115, 0, Math.PI * 1.4);
        ctx.stroke();
        ctx.restore();

        // Central Schematic Geometry
        ctx.strokeStyle = primaryColor;
        ctx.beginPath();
        ctx.arc(cx, cy, 45 + Math.sin(tick * 0.08) * 8, 0, Math.PI * 2);
        ctx.stroke();

        // HUD Telemetry Text inside Canvas
        ctx.fillStyle = '#d8f8ff';
        ctx.font = '14px Orbitron, sans-serif';
        ctx.fillText(`BALA AI // ${currentMode} ASSET`, 20, canvas.height - 45);
        ctx.font = '12px Rajdhani, sans-serif';
        ctx.fillStyle = primaryColor;
        ctx.fillText(`PROMPT: "${prompt.slice(0, 62)}"`, 20, canvas.height - 22);

        if (currentMode === 'VIDEO') {
          tick++;
          animFrameId = requestAnimationFrame(animateFrame);
        } else {
          // Draw static crisp frame for IMAGE mode
          if (tick < 2) {
            tick++;
            animFrameId = requestAnimationFrame(animateFrame);
          }
        }
      }
      animateFrame();
    }

    function sendCommand() {
      const input = document.getElementById('commandInput');
      const cmd = input.value.trim();
      if (!cmd) return;
      logMessage(cmd, false);
      input.value = '';

      setTimeout(() => {
        if (cmd.toLowerCase().includes('video')) {
          switchMode('VIDEO');
        } else if (cmd.toLowerCase().includes('image')) {
          switchMode('IMAGE');
        } else {
          logMessage(`Command acknowledged: "${cmd}". Recalibrating Bala AI synthesis parameters accordingly.`, true);
        }
      }, 300);
    }

    drawIdleGrid();
  </script>
</body>
</html>
