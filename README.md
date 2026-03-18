# tata-corner
Just a tracker!
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>📖</text></svg>">
  <title>Tata's Little Corner</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      min-height: 100vh;
      background: linear-gradient(135deg, #F7F4D5 0%, #e8e4b8 50%, #ddd9a8 100%);
      font-family: Georgia, serif;
      color: #0A3323;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 32px 16px;
      gap: 28px;
    }

    .eyebrow { font-size: 11px; letter-spacing: 6px; text-transform: uppercase; color: #105666; margin-bottom: 6px; text-align: center; }
    h1 { font-size: clamp(26px, 5vw, 38px); font-weight: normal; font-style: italic; color: #0A3323; letter-spacing: -0.5px; text-align: center; }
    h1 span { color: #D3968C; }
    #sync-status { text-align: center; font-size: 11px; color: #105666; margin-top: 4px; font-style: italic; min-height: 16px; letter-spacing: 1px; }

    .tabs { display: flex; gap: 8px; background: rgba(247,244,213,0.65); border-radius: 50px; padding: 5px; border: 1px solid rgba(16,86,102,0.25); }
    .tab { padding: 8px 18px; border-radius: 50px; border: none; cursor: pointer; font-size: 13px; font-family: Georgia, serif; background: transparent; color: #105666; transition: all 0.25s; }
    .tab.active { background: linear-gradient(135deg, #0A3323, #105666); color: #F7F4D5; font-weight: bold; }

    .timer-wrap { position: relative; width: 220px; height: 220px; }
    .timer-svg { position: absolute; transform: rotate(-90deg); width: 100%; height: 100%; }
    .timer-text { position: absolute; inset: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 4px; }
    .timer-digits { font-size: 52px; font-weight: bold; letter-spacing: -2px; color: #0A3323; font-variant-numeric: tabular-nums; transition: transform 0.15s; }
    .timer-digits.pulse { transform: scale(1.06); }
    .timer-label { font-size: 12px; color: #105666; letter-spacing: 3px; text-transform: uppercase; }

    .controls { display: flex; gap: 12px; }
    .btn-primary { padding: 14px 40px; border-radius: 50px; border: none; cursor: pointer; font-size: 16px; font-family: Georgia, serif; font-style: italic; background: linear-gradient(135deg, #0A3323, #105666); color: #F7F4D5; box-shadow: 0 4px 24px rgba(10,51,35,0.25); transition: all 0.2s; min-width: 130px; }
    .btn-primary.running { background: linear-gradient(135deg, #105666, #0A3323); }
    .btn-reset { padding: 14px 20px; border-radius: 50px; border: 1px solid rgba(16,86,102,0.35); cursor: pointer; font-size: 16px; background: transparent; color: #105666; transition: all 0.2s; font-family: Georgia, serif; }

    .stats { display: flex; gap: 24px; background: rgba(247,244,213,0.7); border-radius: 16px; padding: 14px 28px; border: 1px solid rgba(16,86,102,0.2); }
    .stat { text-align: center; }
    .stat-num { font-size: 24px; font-weight: bold; color: #D3968C; }
    .stat-lbl { font-size: 11px; color: #105666; letter-spacing: 2px; }
    .stat-divider { width: 1px; background: rgba(16,86,102,0.2); }

    .quote { font-size: 14px; font-style: italic; color: #105666; text-align: center; max-width: 320px; line-height: 1.6; padding: 12px 20px; background: rgba(247,244,213,0.65); border-radius: 12px; border: 1px solid rgba(16,86,102,0.2); transition: opacity 0.4s; }

    /* Tasks */
    .tasks-card { width: 100%; max-width: 500px; background: rgba(247,244,213,0.7); border-radius: 20px; border: 1px solid rgba(16,86,102,0.2); padding: 20px; }
    .tasks-title { font-size: 12px; letter-spacing: 4px; color: #105666; margin-bottom: 16px; text-transform: uppercase; }
    .task-input-row { display: flex; gap: 8px; margin-bottom: 16px; }
    .task-input { flex: 1; padding: 10px 14px; border-radius: 10px; border: 1px solid rgba(16,86,102,0.3); background: rgba(247,244,213,0.85); color: #0A3323; font-size: 14px; font-family: Georgia, serif; outline: none; }
    .task-input::placeholder { color: #839958; }
    .btn-add { padding: 10px 16px; border-radius: 10px; border: none; background: linear-gradient(135deg, #0A3323, #105666); color: #F7F4D5; cursor: pointer; font-size: 18px; line-height: 1; }
    .task-list { display: flex; flex-direction: column; gap: 8px; }
    .task-item { display: flex; align-items: center; gap: 12px; padding: 10px 14px; border-radius: 10px; background: rgba(247,244,213,0.75); border: 1px solid rgba(16,86,102,0.15); transition: all 0.2s; }
    .task-item.done { background: rgba(211,150,140,0.12); border-color: rgba(211,150,140,0.3); }
    .task-check { width: 20px; height: 20px; border-radius: 6px; border: 2px solid rgba(16,86,102,0.4); background: transparent; cursor: pointer; display: flex; align-items: center; justify-content: center; flex-shrink: 0; transition: all 0.2s; font-size: 12px; color: #F7F4D5; }
    .task-check.checked { border-color: #839958; background: linear-gradient(135deg, #839958, #0A3323); }
    .task-text { flex: 1; font-size: 14px; color: #0A3323; transition: all 0.2s; }
    .task-text.done { color: #839958; text-decoration: line-through; }
    .task-remove { background: transparent; border: none; color: #a8bfa8; cursor: pointer; font-size: 18px; line-height: 1; padding: 2px 4px; font-family: Georgia, serif; }
    .empty-msg { text-align: center; color: #839958; font-size: 13px; font-style: italic; padding: 12px; }

    /* Try-Out Tracker */
    .score-card { width: 100%; max-width: 500px; background: rgba(247,244,213,0.7); border-radius: 20px; border: 1px solid rgba(16,86,102,0.2); padding: 20px; }
    .score-title { font-size: 12px; letter-spacing: 4px; color: #105666; margin-bottom: 16px; text-transform: uppercase; }
    .to-list { display: flex; flex-direction: column; gap: 10px; }
    .to-item { border-radius: 12px; border: 1px solid rgba(16,86,102,0.2); background: rgba(247,244,213,0.65); overflow: hidden; }
    .to-header { display: flex; align-items: center; gap: 10px; padding: 11px 14px; cursor: pointer; user-select: none; transition: background 0.15s; }
    .to-header:hover { background: rgba(16,86,102,0.07); }
    .to-arrow { font-size: 9px; color: #105666; transition: transform 0.25s; flex-shrink: 0; }
    .to-arrow.open { transform: rotate(90deg); }
    .to-name { flex: 1; font-size: 14px; color: #0A3323; font-family: Georgia, serif; }
    .to-name-input { flex: 1; background: rgba(255,255,255,0.8); border: 1px solid rgba(16,86,102,0.3); border-radius: 7px; padding: 4px 8px; font-size: 14px; font-family: Georgia, serif; color: #0A3323; outline: none; display: none; }
    .to-remove { background: transparent; border: none; color: #a8bfa8; cursor: pointer; font-size: 17px; padding: 0 2px; line-height: 1; font-family: Georgia, serif; }
    .to-overall-score { width: 80px; padding: 4px 8px; border-radius: 20px; border: 1px solid rgba(211,150,140,0.4); background: rgba(247,244,213,0.85); color: #D3968C; font-size: 12px; font-weight: bold; font-family: Georgia, serif; outline: none; text-align: center; flex-shrink: 0; }
    .to-overall-score::placeholder { color: #a8bfa8; font-weight: normal; }
    .to-body { display: none; border-top: 1px solid rgba(16,86,102,0.15); padding: 12px 14px; background: rgba(131,153,88,0.04); }
    .to-body.open { display: block; }
    .sub-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; }
    .sub-table th { font-size: 10px; letter-spacing: 2.5px; text-transform: uppercase; color: #105666; padding: 4px 8px; text-align: left; font-weight: normal; border-bottom: 1px solid rgba(16,86,102,0.2); }
    .sub-table th:nth-child(2) { width: 70px; text-align: center; }
    .sub-table th:last-child { width: 24px; }
    .sub-row td { padding: 6px 8px; border-bottom: 1px solid rgba(16,86,102,0.08); vertical-align: top; }
    .sub-row:last-child td { border-bottom: none; }
    .sub-input { width: 100%; background: transparent; border: none; font-family: Georgia, serif; font-size: 13px; color: #0A3323; outline: none; padding: 0; }
    .sub-input:focus { background: rgba(247,244,213,0.85); border-radius: 5px; padding: 2px 5px; }
    .sub-input::placeholder { color: #a8bfa8; }
    .sub-score-input { width: 100%; background: transparent; border: none; font-family: Georgia, serif; font-size: 13px; font-weight: bold; color: #D3968C; text-align: center; outline: none; padding: 0; }
    .sub-score-input:focus { background: rgba(247,244,213,0.85); border-radius: 5px; padding: 2px 4px; }
    .sub-score-input::placeholder { color: #a8bfa8; font-weight: normal; }
    .sub-remove { background: transparent; border: none; color: #a8bfa8; cursor: pointer; font-size: 14px; padding: 0; line-height: 1; }
    .sub-add-row { display: flex; gap: 6px; margin-top: 4px; }
    .sub-add-input { flex: 1; padding: 6px 10px; border-radius: 8px; border: 1px solid rgba(16,86,102,0.25); background: rgba(247,244,213,0.8); color: #0A3323; font-size: 12px; font-family: Georgia, serif; outline: none; }
    .sub-add-input::placeholder { color: #a8bfa8; }
    .sub-add-score { width: 60px; padding: 6px 8px; border-radius: 8px; border: 1px solid rgba(16,86,102,0.25); background: rgba(247,244,213,0.8); color: #D3968C; font-size: 12px; font-family: Georgia, serif; outline: none; text-align: center; }
    .sub-add-score::placeholder { color: #a8bfa8; }
    .sub-add-btn { padding: 6px 12px; border-radius: 8px; border: none; background: linear-gradient(135deg, #0A3323, #105666); color: #F7F4D5; cursor: pointer; font-size: 15px; line-height: 1; }
    .to-add-row { display: flex; gap: 8px; margin-top: 12px; }
    .to-add-input { flex: 1; padding: 9px 13px; border-radius: 10px; border: 1px solid rgba(16,86,102,0.3); background: rgba(247,244,213,0.85); color: #0A3323; font-size: 13px; font-family: Georgia, serif; outline: none; }
    .to-add-input::placeholder { color: #a8bfa8; }
    .to-add-btn { padding: 9px 16px; border-radius: 10px; border: none; background: linear-gradient(135deg, #0A3323, #105666); color: #F7F4D5; cursor: pointer; font-size: 17px; line-height: 1; }

    /* H-30 Tracker */
    .h30-card { width: 100%; max-width: 500px; background: rgba(247,244,213,0.7); border-radius: 20px; border: 1px solid rgba(16,86,102,0.2); padding: 20px; }
    .h30-title { font-size: 12px; letter-spacing: 4px; color: #105666; margin-bottom: 4px; text-transform: uppercase; }
    .h30-subtitle { font-size: 11px; color: #839958; font-style: italic; margin-bottom: 16px; }
    .h30-progress-bar { width: 100%; height: 6px; background: rgba(16,86,102,0.1); border-radius: 99px; margin-bottom: 16px; overflow: hidden; }
    .h30-progress-fill { height: 100%; background: linear-gradient(90deg, #0A3323, #839958); border-radius: 99px; transition: width 0.4s ease; }
    .h30-progress-label { font-size: 11px; color: #105666; text-align: right; margin-top: -12px; margin-bottom: 14px; }
    .h30-list { display: flex; flex-direction: column; gap: 8px; }
    .h30-day { border-radius: 12px; border: 1px solid rgba(16,86,102,0.15); background: rgba(247,244,213,0.6); overflow: hidden; }
    .h30-day.completed { border-color: rgba(131,153,88,0.4); background: rgba(131,153,88,0.08); }
    .h30-day-header { display: flex; align-items: center; gap: 10px; padding: 10px 14px; cursor: pointer; user-select: none; transition: background 0.15s; }
    .h30-day-header:hover { background: rgba(16,86,102,0.05); }
    .h30-day-arrow { font-size: 9px; color: #105666; transition: transform 0.25s; flex-shrink: 0; }
    .h30-day-arrow.open { transform: rotate(90deg); }
    .h30-day-label { font-size: 13px; font-weight: bold; color: #0A3323; min-width: 40px; }
    .h30-day-preview { flex: 1; font-size: 11px; color: #839958; font-style: italic; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .h30-day-badges { display: flex; gap: 4px; flex-shrink: 0; }
    .h30-badge { font-size: 10px; padding: 2px 7px; border-radius: 99px; background: rgba(131,153,88,0.15); color: #839958; border: 1px solid rgba(131,153,88,0.3); }
    .h30-badge.done { background: rgba(10,51,35,0.1); color: #0A3323; border-color: rgba(10,51,35,0.2); }
    .h30-day-body { display: none; border-top: 1px solid rgba(16,86,102,0.1); padding: 12px 14px; }
    .h30-day-body.open { display: block; }
    .h30-table { width: 100%; border-collapse: collapse; }
    .h30-table th { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: #105666; padding: 4px 8px; text-align: left; font-weight: normal; border-bottom: 1px solid rgba(16,86,102,0.15); }
    .h30-table th.center { text-align: center; width: 70px; }
    .h30-table td { padding: 7px 8px; font-size: 12px; color: #0A3323; border-bottom: 1px solid rgba(16,86,102,0.06); vertical-align: middle; }
    .h30-table tr:last-child td { border-bottom: none; }
    .h30-table td.center { text-align: center; }
    .h30-subtes { font-size: 10px; font-weight: bold; color: #105666; background: rgba(16,86,102,0.08); padding: 2px 7px; border-radius: 99px; white-space: nowrap; }
    .h30-cb { width: 18px; height: 18px; border-radius: 5px; border: 2px solid rgba(16,86,102,0.3); background: rgba(247,244,213,0.8); cursor: pointer; display: inline-flex; align-items: center; justify-content: center; transition: all 0.15s; font-size: 11px; color: #F7F4D5; flex-shrink: 0; }
    .h30-cb.checked { background: linear-gradient(135deg, #0A3323, #839958); border-color: #839958; }
    .h30-cb-wrap { display: flex; justify-content: center; }
    .h30-to-row td { color: #D3968C; font-style: italic; font-size: 12px; }
    .h30-practice-note { font-size: 12px; color: #105666; font-style: italic; padding: 6px 0 10px 0; }

    /* Mobile */
    @media (max-width: 480px) {
      body { padding: 20px 12px; gap: 20px; }
      h1 { font-size: 24px; }
      .tabs { flex-wrap: wrap; justify-content: center; gap: 6px; }
      .tab { padding: 7px 14px; font-size: 12px; }
      .timer-wrap { width: 180px; height: 180px; }
      .timer-digits { font-size: 42px; }
      .btn-primary { padding: 12px 28px; font-size: 14px; min-width: 110px; }
      .btn-reset { padding: 12px 16px; font-size: 14px; }
      .stats { gap: 16px; padding: 12px 18px; }
      .stat-num { font-size: 20px; }
      .stat-lbl { font-size: 10px; }
      .quote { font-size: 13px; max-width: 100%; }
      .tasks-card, .score-card, .h30-card { max-width: 100%; padding: 16px; }
      .sub-table th:nth-child(3), .sub-table td:nth-child(3) { display: none; }
      .to-overall-score { width: 68px; font-size: 11px; }
      .h30-table th.center, .h30-table td.center { width: 52px; }
    }
    @media (max-width: 360px) {
      .timer-wrap { width: 160px; height: 160px; }
      .timer-digits { font-size: 36px; }
      .stats { gap: 10px; padding: 10px 12px; }
    }
  </style>
</head>
<body>

  <div>
    <div class="eyebrow">Student Focus Studio</div>
    <h1>Deep Work, <span>Done.</span></h1>
    <div id="sync-status">Memuat data...</div>
  </div>

  <div class="tabs">
    <button class="tab active" data-mode="focus">Focus</button>
    <button class="tab" data-mode="short">Short Break</button>
    <button class="tab" data-mode="long">Long Break</button>
  </div>

  <div class="timer-wrap">
    <svg class="timer-svg" width="220" height="220">
      <defs>
        <linearGradient id="grad" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" stop-color="#0A3323"/>
          <stop offset="100%" stop-color="#839958"/>
        </linearGradient>
      </defs>
      <circle cx="110" cy="110" r="90" fill="none" stroke="rgba(16,86,102,0.15)" stroke-width="8"/>
      <circle id="progress-ring" cx="110" cy="110" r="90" fill="none"
        stroke="url(#grad)" stroke-width="8" stroke-linecap="round"
        stroke-dasharray="565.49" stroke-dashoffset="565.49"/>
    </svg>
    <div class="timer-text">
      <div class="timer-digits" id="timer-display">25:00</div>
      <div class="timer-label" id="mode-label">Focus</div>
    </div>
  </div>

  <div class="controls">
    <button class="btn-primary" id="toggle-btn">▶ Start</button>
    <button class="btn-reset" id="reset-btn">↺</button>
  </div>

  <div class="stats">
    <div class="stat"><div class="stat-num" id="stat-sessions">0</div><div class="stat-lbl">SESSIONS</div></div>
    <div class="stat-divider"></div>
    <div class="stat"><div class="stat-num" id="stat-minutes">0m</div><div class="stat-lbl">FOCUSED</div></div>
    <div class="stat-divider"></div>
    <div class="stat"><div class="stat-num" id="stat-tasks">0/3</div><div class="stat-lbl">TASKS</div></div>
  </div>

  <div class="quote" id="quote-box">You've got this. One session at a time. 🔥</div>

  <div class="tasks-card">
    <div class="tasks-title">Today's Tasks</div>
    <div class="task-input-row">
      <input class="task-input" id="task-input" type="text" placeholder="Add a task…" />
      <button class="btn-add" id="add-btn">+</button>
    </div>
    <div class="task-list" id="task-list"></div>
  </div>

  <div class="score-card">
    <div class="score-title">Try-Out Tracker</div>
    <div class="to-list" id="to-list"></div>
    <div class="to-add-row">
      <input class="to-add-input" id="to-add-input" placeholder="Add a try-out (e.g. Mock Test 1)…" />
      <button class="to-add-btn" id="to-add-btn">+</button>
    </div>
  </div>

  <div class="h30-card">
    <div class="h30-title">📖 Tracker Belajar H-30 SNBT 2026</div>
    <div class="h30-subtitle">*TO disesuaikan jadwal kalian ya, nggak setiap hari kok ></div>
    <div class="h30-progress-bar"><div class="h30-progress-fill" id="h30-fill" style="width:0%"></div></div>
    <div class="h30-progress-label" id="h30-prog-label">0 / 0 selesai</div>
    <div class="h30-list" id="h30-list"></div>
  </div>

  <script>
    // ── JSONBin Config ──
    const BIN_ID  = "69ba1b52aa77b81da9f495fc";
    const API_KEY = "$2a$10$0XnIchPZoFt7JbhzbZp.suyhv4EYKaLevXOCqoitEPTA53DMLs5yW";
    const BIN_URL = "https://api.jsonbin.io/v3/b/" + BIN_ID;
    let saveTimer = null;

    function showSync(msg, color) {
      const el = document.getElementById("sync-status");
      if (el) { el.textContent = msg; el.style.color = color || "#105666"; }
    }

    async function loadCloud() {
      try {
        const res = await fetch(BIN_URL + "/latest", { headers: { "X-Master-Key": API_KEY } });
        const data = await res.json();
        return data.record || {};
      } catch(e) { showSync("Gagal memuat data :(", "#D3968C"); return {}; }
    }

    function scheduleSave() {
      clearTimeout(saveTimer);
      saveTimer = setTimeout(async () => {
        showSync("Menyimpan...", "#105666");
        try {
          await fetch(BIN_URL, {
            method: "PUT",
            headers: { "Content-Type": "application/json", "X-Master-Key": API_KEY },
            body: JSON.stringify({ tasks, nextId, h30State, toItems: collectToItems() })
          });
          showSync("✓ Tersimpan", "#839958");
        } catch(e) { showSync("Gagal menyimpan :(", "#D3968C"); }
      }, 1200);
    }

    function collectToItems() {
      const result = [];
      document.querySelectorAll(".to-item").forEach(div => {
        const name = div.querySelector(".to-name")?.textContent?.trim() || "";
        const overall = div.querySelector(".to-overall-score")?.value || "";
        const subs = [];
        div.querySelectorAll(".sub-row").forEach(tr => {
          const ins = tr.querySelectorAll("input");
          subs.push({ name: ins[0]?.value||"", score: ins[1]?.value||"", improve: ins[2]?.value||"" });
        });
        result.push({ name, overall, subs });
      });
      return result;
    }

    // ── Timer ──
    const DURATIONS = { focus: 25*60, short: 5*60, long: 15*60 };
    const LABELS    = { focus: "Focus", short: "Short Break", long: "Long Break" };
    const QUOTES    = [
      "You've got this. One session at a time. 🔥",
      "Progress > perfection. Start messy.",
      "Future you is cheering right now.",
      "The hardest part is starting. You already did.",
      "Small steps. Big results.",
    ];
    const CIRC = 2 * Math.PI * 90;
    let mode = "focus", secsLeft = DURATIONS.focus, running = false, timerId = null, sessions = 0;

    const dispEl   = document.getElementById("timer-display");
    const modeEl   = document.getElementById("mode-label");
    const ringEl   = document.getElementById("progress-ring");
    const startBtn = document.getElementById("toggle-btn");
    const rstBtn   = document.getElementById("reset-btn");
    const quoteEl  = document.getElementById("quote-box");
    const statS    = document.getElementById("stat-sessions");
    const statM    = document.getElementById("stat-minutes");
    const statT    = document.getElementById("stat-tasks");

    function pad(n) { return String(n).padStart(2,"0"); }
    function updateTimer() {
      dispEl.textContent = pad(Math.floor(secsLeft/60)) + ":" + pad(secsLeft%60);
      ringEl.style.strokeDashoffset = CIRC * (secsLeft / DURATIONS[mode]);
    }
    function updateStats() {
      statS.textContent = sessions;
      statM.textContent = (sessions*25) + "m";
      const done = tasks.filter(t=>t.done).length;
      statT.textContent = done + "/" + tasks.length;
    }

    document.querySelectorAll(".tab").forEach(btn => btn.addEventListener("click", () => {
      mode = btn.dataset.mode; secsLeft = DURATIONS[mode]; running = false; clearInterval(timerId);
      modeEl.textContent = LABELS[mode]; startBtn.textContent = "▶ Start"; startBtn.classList.remove("running");
      document.querySelectorAll(".tab").forEach(t => t.classList.toggle("active", t===btn));
      updateTimer();
    }));

    startBtn.addEventListener("click", () => {
      running = !running;
      dispEl.classList.add("pulse"); setTimeout(()=>dispEl.classList.remove("pulse"),300);
      if (running) {
        startBtn.textContent = "⏸ Pause"; startBtn.classList.add("running");
        timerId = setInterval(() => {
          secsLeft--; updateTimer();
          if (secsLeft <= 0) {
            clearInterval(timerId); running = false;
            startBtn.textContent = "▶ Start"; startBtn.classList.remove("running");
            if (mode==="focus") sessions++;
            quoteEl.style.opacity=0;
            setTimeout(()=>{ quoteEl.textContent=QUOTES[Math.floor(Math.random()*QUOTES.length)]; quoteEl.style.opacity=1; },400);
            updateStats();
          }
        },1000);
      } else { clearInterval(timerId); startBtn.textContent="▶ Resume"; startBtn.classList.remove("running"); }
    });

    rstBtn.addEventListener("click", () => {
      clearInterval(timerId); running=false; secsLeft=DURATIONS[mode];
      startBtn.textContent="▶ Start"; startBtn.classList.remove("running"); updateTimer();
    });

    // ── Tasks ──
    let tasks = [], nextId = 1;
    const taskListEl = document.getElementById("task-list");
    const taskInput  = document.getElementById("task-input");

    function renderTasks() {
      if (!tasks.length) {
        taskListEl.innerHTML = '<div class="empty-msg">All clear! Add something to work on.</div>';
      } else {
        taskListEl.innerHTML = tasks.map(t=>`
          <div class="task-item ${t.done?'done':''}" data-id="${t.id}">
            <div class="task-check ${t.done?'checked':''}" data-action="check">${t.done?'✓':''}</div>
            <span class="task-text ${t.done?'done':''}">${t.text}</span>
            <button class="task-remove" data-action="remove">×</button>
          </div>`).join('');
      }
      updateStats();
    }

    taskListEl.addEventListener("click", e => {
      const item = e.target.closest("[data-id]"); if (!item) return;
      const id = parseInt(item.dataset.id);
      const act = e.target.dataset.action || e.target.closest("[data-action]")?.dataset.action;
      if (act==="check") { tasks=tasks.map(t=>t.id===id?{...t,done:!t.done}:t); renderTasks(); scheduleSave(); }
      else if (act==="remove") { tasks=tasks.filter(t=>t.id!==id); renderTasks(); scheduleSave(); }
    });

    function addTask() {
      const txt = taskInput.value.trim(); if(!txt) return;
      tasks.push({id:nextId++, text:txt, done:false}); taskInput.value=""; renderTasks(); scheduleSave();
    }
    document.getElementById("add-btn").addEventListener("click", addTask);
    taskInput.addEventListener("keydown", e=>{ if(e.key==="Enter") addTask(); });

    // ── Try-Out Tracker ──
    const toListEl   = document.getElementById("to-list");
    const toAddInput = document.getElementById("to-add-input");
    const toAddBtn   = document.getElementById("to-add-btn");

    function buildSubRow(tbody, sn, sc, imp) {
      const tr = document.createElement("tr"); tr.className="sub-row";
      tr.innerHTML=`
        <td><input class="sub-input" value="${sn}" placeholder="e.g. Reading…"/></td>
        <td><input class="sub-score-input" value="${sc}" placeholder="—"/></td>
        <td><input class="sub-input" value="${imp}" placeholder="e.g. Vocabulary…"/></td>
        <td><button class="sub-remove">×</button></td>`;
      tr.querySelector(".sub-remove").addEventListener("click",()=>{ tr.remove(); scheduleSave(); });
      tr.querySelectorAll("input").forEach(i=>i.addEventListener("change", scheduleSave));
      tbody.appendChild(tr);
    }

    function createToItem(name, overall, subs) {
      const div = document.createElement("div"); div.className="to-item";
      div.innerHTML=`
        <div class="to-header">
          <span class="to-arrow">▶</span>
          <span class="to-name">${name}</span>
          <input class="to-name-input" type="text" value="${name}"/>
          <input class="to-overall-score" type="text" value="${overall||''}" placeholder="Overall score"/>
          <button class="to-remove" title="Remove">×</button>
        </div>
        <div class="to-body">
          <table class="sub-table">
            <thead><tr><th>Subtest</th><th>Score</th><th>What to Improve</th><th></th></tr></thead>
            <tbody class="sub-tbody"></tbody>
          </table>
          <div class="sub-add-row">
            <input class="sub-add-input" placeholder="Subtest name…"/>
            <input class="sub-add-score" placeholder="Score"/>
            <button class="sub-add-btn">+</button>
          </div>
        </div>`;

      const header    = div.querySelector(".to-header");
      const arrow     = div.querySelector(".to-arrow");
      const body      = div.querySelector(".to-body");
      const nameSpan  = div.querySelector(".to-name");
      const nameInput = div.querySelector(".to-name-input");
      const removeBtn = div.querySelector(".to-remove");
      const subTbody  = div.querySelector(".sub-tbody");
      const subIn     = div.querySelector(".sub-add-input");
      const subSc     = div.querySelector(".sub-add-score");
      const subBtn    = div.querySelector(".sub-add-btn");

      header.addEventListener("click", e=>{
        if(e.target===removeBtn||e.target===nameInput||e.target.classList.contains("to-overall-score")) return;
        const open=body.classList.toggle("open"); arrow.classList.toggle("open",open);
      });
      nameSpan.addEventListener("dblclick", e=>{
        e.stopPropagation(); nameSpan.style.display="none"; nameInput.style.display="block"; nameInput.focus(); nameInput.select();
      });
      nameInput.addEventListener("blur",()=>{
        nameSpan.textContent=nameInput.value.trim()||name; nameInput.style.display="none"; nameSpan.style.display=""; scheduleSave();
      });
      nameInput.addEventListener("keydown",e=>{ if(e.key==="Enter") nameInput.blur(); e.stopPropagation(); });
      removeBtn.addEventListener("click",e=>{ e.stopPropagation(); div.remove(); scheduleSave(); });
      div.querySelector(".to-overall-score").addEventListener("change", scheduleSave);

      subBtn.addEventListener("click",()=>{
        const n=subIn.value.trim(); if(!n) return;
        buildSubRow(subTbody, n, subSc.value.trim(), ""); subIn.value=""; subSc.value=""; subIn.focus(); scheduleSave();
      });
      subIn.addEventListener("keydown",e=>{ if(e.key==="Enter") subBtn.click(); });

      // populate saved subs or empty hint row
      if (subs && subs.length) subs.forEach(s=>buildSubRow(subTbody, s.name||"", s.score||"", s.improve||""));
      else buildSubRow(subTbody,"","","");

      toListEl.appendChild(div);
    }

    // fix typo helper
    function scheduleS() { scheduleSave(); }
    function scheduleToSave() { scheduleSave(); }

    toAddBtn.addEventListener("click",()=>{ const v=toAddInput.value.trim(); if(!v) return; createToItem(v,"",[]); toAddInput.value=""; scheduleSave(); });
    toAddInput.addEventListener("keydown",e=>{ if(e.key==="Enter") toAddBtn.click(); });

    // ── H-30 Tracker ──
    const H30_DATA = [
      { day:"H-30", items:[{s:"PU",m:"Kesesuaian Pernyataan"},{s:"PK/PM",m:"Operasi Bilangan"},{s:"LBI",m:"Informasi Tersurat & Tersirat"}], to:true },
      { day:"H-29", items:[{s:"PPU/PBM",m:"Kata"},{s:"PK/PM",m:"KPK & FPB"},{s:"LBE",m:"Synonym & Antonym"}], to:true },
      { day:"H-28", items:[{s:"PU",m:"Argumentasi (Memperlemah & Memperkuat)"},{s:"PK/PM",m:"Persamaan Garis Lurus"},{s:"LBI",m:"Tema, Topik & Judul"}], to:true },
      { day:"H-27", items:[{s:"PPU/PBM",m:"Frasa & Klausa"},{s:"PK/PM",m:"Sistem Persamaan Linear"},{s:"LBE",m:"Word Meaning"}], to:true },
      { day:"H-26", items:[{s:"PU",m:"Modus Ponens, Tollens, & Negasi"},{s:"PK/PM",m:"Fungsi Linear"},{s:"LBI/PBM/PPU",m:"Makna Kata"}], to:true },
      { day:"H-25", items:[{s:"PPU/PBM",m:"Pola Kalimat"},{s:"PK/PM",m:"Pertidaksamaan Linear"},{s:"LBE",m:"Main Idea & Topic"}], to:true },
      { day:"H-24", items:[{s:"PU",m:"Silogisme & Ekuivalensi"},{s:"PK/PM",m:"Persamaan & Fungsi Kuadrat"},{s:"LBI/PPU/PBM",m:"Gagasan Pokok"}], to:true },
      { day:"H-23", items:[{s:"PPU/PBM",m:"Konjungsi"},{s:"PK/PM",m:"Barisan & Deret Aritmetika"},{s:"LBE",m:"True or False Statement"}], to:true },
      { day:"H-22", items:[{s:"PU",m:"Logika Kuantor"},{s:"PK/PM",m:"Barisan & Deret Geometri"},{s:"LBI/PPU/PBM",m:"Kesimpulan"}], to:true },
      { day:"H-21", items:[{s:"PPU",m:"Kalimat Logis & Efektif"},{s:"PK/PM",m:"Titik, Garis, & Sudut"},{s:"LBE",m:"Finding Detailed Information"}], to:true },
      { day:"H-20", items:[{s:"PU",m:"Kualitas Simpulan"},{s:"PK/PM",m:"Bangun Datar"},{s:"LBI/PPU",m:"Majas"}], to:true },
      { day:"H-19", items:[{s:"PPU/PBM",m:"Inti Kalimat & Kalimat Inti"},{s:"PK/PM",m:"Trigonometri Dasar"},{s:"LBE",m:"Purpose of The Text"}], to:true },
      { day:"H-18", items:[{s:"PU",m:"Pola Bilangan"},{s:"PK/PM",m:"Bangun Ruang Sisi Tegak"},{s:"LBI/PBM/PPU",m:"Fakta & Opini"}], to:true },
      { day:"H-17", items:[{s:"PPU/PBM",m:"Kepaduan Paragraf"},{s:"PK/PM",m:"Bangun Ruang Sisi Lengkung"},{s:"LBE",m:"Reference"}], to:true },
      { day:"H-16", items:[{s:"PU",m:"Perbandingan"},{s:"PK/PM",m:"Jarak Titik, Garis, Bidang"},{s:"LBI/PPU",m:"Sikap & Tujuan Penulis"}], to:true },
      { day:"H-15", items:[{s:"PPU",m:"Kata Berpasangan Tetap"},{s:"PK/PM",m:"Kaidah Pencacahan"},{s:"LBE",m:"Author's Opinion & Attitude"}], to:true },
      { day:"H-14", items:[{s:"PBM",m:"Penggunaan Huruf"},{s:"PK",m:"Peluang"},{s:"LBI",m:"Latar & Alur"}], to:true },
      { day:"H-13", items:[{s:"PPU",m:"Hiponim & Hipernim"},{s:"PU/PM",m:"Interpretasi Data"},{s:"LBE",m:"Restatement"}], to:true },
      { day:"H-12", items:[{s:"PBM",m:"Penulisan Kata"},{s:"PK",m:"Fungsi Komposisi & Invers"},{s:"LBI",m:"Tokoh & Penokohan"}], to:true },
      { day:"H-11", items:[{s:"PPU",m:"Sinonim & Antonim"},{s:"PM",m:"Jarak, Kecepatan, Waktu"},{s:"LBE",m:"Opinion Thread"}], to:true },
      { day:"H-10", items:[{s:"PBM",m:"Penggunaan Tanda Baca"},{s:"PK",m:"Himpunan & Diagram Venn"},{s:"LBI",m:"Sudut Pandang & Amanat"}], to:true },
      { day:"H-9",  items:[{s:"PPU",m:"Bahasa Baku"},{s:"PM",m:"Kecepatan & Debit"},{s:"LBE",m:"Comparing Two Texts/Paired Texts"}], to:true },
      { day:"H-8",  practice:true, items:[{s:"PU",m:""},{s:"PPU",m:""},{s:"PBM",m:""},{s:"PK",m:""}] },
      { day:"H-7",  practice:true, items:[{s:"LBI",m:""},{s:"LBE",m:""},{s:"PM",m:""}] },
      { day:"H-6",  practice:true, items:[{s:"PU",m:""},{s:"PPU",m:""},{s:"PBM",m:""},{s:"PK",m:""}] },
      { day:"H-5",  practice:true, items:[{s:"LBI",m:""},{s:"LBE",m:""},{s:"PM",m:""}] },
      { day:"H-4",  practice:true, items:[{s:"PU",m:""},{s:"PPU",m:""},{s:"PBM",m:""},{s:"PK",m:""}] },
      { day:"H-3",  practice:true, items:[{s:"LBI",m:""},{s:"LBE",m:""},{s:"PM",m:""}] },
      { day:"H-2",  rest:true },
      { day:"H-1",  rest:true },
    ];

    const h30ListEl = document.getElementById("h30-list");
    const h30Fill   = document.getElementById("h30-fill");
    const h30Label  = document.getElementById("h30-prog-label");
    let h30State    = {};

    function h30Progress() {
      let total=0, checked=0;
      H30_DATA.forEach(d=>{
        if(d.rest) return;
        if(d.practice){ total+=d.items.length*2+2; return; }
        total+=d.items.length*2; if(d.to) total+=2;
      });
      checked = Object.values(h30State).filter(Boolean).length;
      const pct = total>0?Math.round(checked/total*100):0;
      h30Fill.style.width=pct+"%";
      h30Label.textContent=checked+" / "+total+" selesai ("+pct+"%)";
    }

    function makeCb(key) {
      const wrap=document.createElement("div"); wrap.className="h30-cb-wrap"; wrap.dataset.key=key;
      const cb=document.createElement("div"); cb.className="h30-cb"+(h30State[key]?" checked":""); cb.textContent=h30State[key]?"✓":"";
      cb.addEventListener("click", e=>{
        e.stopPropagation();
        h30State[key]=!h30State[key];
        cb.classList.toggle("checked",h30State[key]); cb.textContent=h30State[key]?"✓":"";
        const dayEl=cb.closest(".h30-day");
        if(dayEl){
          const allDone=[...dayEl.querySelectorAll(".h30-cb")].every(c=>c.classList.contains("checked"));
          dayEl.classList.toggle("completed",allDone);
          const badge=dayEl.querySelector(".h30-badge");
          if(badge){ badge.textContent=allDone?"✓ Selesai":""; badge.classList.toggle("done",allDone); badge.style.display=allDone?"":"none"; }
        }
        h30Progress(); scheduleSave();
      });
      wrap.appendChild(cb); return wrap;
    }

    function buildH30() {
      h30ListEl.innerHTML="";
      H30_DATA.forEach((d,di)=>{
        const dayDiv=document.createElement("div"); dayDiv.className="h30-day";
        const header=document.createElement("div"); header.className="h30-day-header";
        const arrow=document.createElement("span"); arrow.className="h30-day-arrow"; arrow.textContent="▶";
        const lbl=document.createElement("span"); lbl.className="h30-day-label"; lbl.textContent=d.day;
        const prev=document.createElement("span"); prev.className="h30-day-preview";
        if(d.rest) prev.textContent="Istirahat 😌";
        else if(d.practice) prev.textContent="Satu Paket Latihan Soal";
        else prev.textContent=d.items.map(i=>i.m).join(", ");
        const badges=document.createElement("div"); badges.className="h30-day-badges";
        const badge=document.createElement("span"); badge.className="h30-badge"; badge.style.display="none";
        badges.appendChild(badge);
        header.appendChild(arrow); header.appendChild(lbl); header.appendChild(prev); header.appendChild(badges);

        const body=document.createElement("div"); body.className="h30-day-body";
        if(d.rest){
          body.innerHTML=`<div style="text-align:center;padding:14px 0;font-size:13px;color:#839958;font-style:italic;">Istirahat total — jangan belajar ya! Tidur cukup, makan enak 😴🍜</div>`;
        } else if(d.practice){
          body.innerHTML=`<div class="h30-practice-note">📝 Kerjakan satu paket latihan soal penuh hari ini</div>`;
          const tbl=document.createElement("table"); tbl.className="h30-table";
          tbl.innerHTML=`<thead><tr><th>Subtes</th><th class="center">Selesai</th><th class="center">Latsol</th></tr></thead>`;
          const tbody=document.createElement("tbody");
          d.items.forEach((item,ii)=>{
            const tr=document.createElement("tr");
            tr.innerHTML=`<td><span class="h30-subtes">${item.s}</span></td>`;
            const t1=document.createElement("td"); t1.className="center"; t1.appendChild(makeCb(`${di}-${ii}-bm`)); tr.appendChild(t1);
            const t2=document.createElement("td"); t2.className="center"; t2.appendChild(makeCb(`${di}-${ii}-ls`)); tr.appendChild(t2);
            tbody.appendChild(tr);
          });
          const toTr=document.createElement("tr"); toTr.className="h30-to-row";
          toTr.innerHTML=`<td style="font-style:italic;color:#D3968C;font-size:11px;">TO</td>`;
          const t3=document.createElement("td"); t3.className="center"; t3.appendChild(makeCb(`${di}-to`)); toTr.appendChild(t3);
          const t4=document.createElement("td"); t4.className="center"; t4.appendChild(makeCb(`${di}-eval`)); toTr.appendChild(t4);
          tbody.appendChild(toTr); tbl.appendChild(tbody); body.appendChild(tbl);
        } else {
          const tbl=document.createElement("table"); tbl.className="h30-table";
          tbl.innerHTML=`<thead><tr><th>Subtes</th><th>Materi</th><th class="center">Belajar</th><th class="center">Latsol</th></tr></thead>`;
          const tbody=document.createElement("tbody");
          d.items.forEach((item,ii)=>{
            const tr=document.createElement("tr");
            tr.innerHTML=`<td><span class="h30-subtes">${item.s}</span></td><td>${item.m}</td>`;
            const t1=document.createElement("td"); t1.className="center"; t1.appendChild(makeCb(`${di}-${ii}-bm`)); tr.appendChild(t1);
            const t2=document.createElement("td"); t2.className="center"; t2.appendChild(makeCb(`${di}-${ii}-ls`)); tr.appendChild(t2);
            tbody.appendChild(tr);
          });
          if(d.to){
            const toTr=document.createElement("tr"); toTr.className="h30-to-row";
            toTr.innerHTML=`<td colspan="2" style="font-style:italic;color:#D3968C;font-size:11px;">TO + Evaluasi TO</td>`;
            const t3=document.createElement("td"); t3.className="center"; t3.appendChild(makeCb(`${di}-to`)); toTr.appendChild(t3);
            const t4=document.createElement("td"); t4.className="center"; t4.appendChild(makeCb(`${di}-eval`)); toTr.appendChild(t4);
            tbody.appendChild(toTr);
          }
          tbl.appendChild(tbody); body.appendChild(tbl);
        }
        header.addEventListener("click",()=>{ const open=body.classList.toggle("open"); arrow.classList.toggle("open",open); });
        dayDiv.appendChild(header); dayDiv.appendChild(body); h30ListEl.appendChild(dayDiv);
      });

      // restore completed badges
      H30_DATA.forEach((_,di)=>{
        const dayDiv=h30ListEl.children[di];
        if(!dayDiv) return;
        const allCbs=[...dayDiv.querySelectorAll(".h30-cb")];
        if(allCbs.length && allCbs.every(c=>c.classList.contains("checked"))){
          dayDiv.classList.add("completed");
          const badge=dayDiv.querySelector(".h30-badge");
          if(badge){ badge.textContent="✓ Selesai"; badge.classList.add("done"); badge.style.display=""; }
        }
      });
    }

    // ── Boot ──
    async function boot() {
      showSync("Memuat data...", "#105666");
      updateTimer(); renderTasks(); buildH30(); h30Progress();

      const cloud = await loadCloud();

      if (cloud.tasks)    { tasks=cloud.tasks; nextId=cloud.nextId||tasks.length+1; renderTasks(); }
      if (cloud.h30State) { h30State=cloud.h30State; buildH30(); h30Progress(); }
      if (cloud.toItems && cloud.toItems.length) {
        cloud.toItems.forEach(item=>createToItem(item.name||"", item.overall||"", item.subs||[]));
      } else {
        createToItem("Mock Test 1","",[]); createToItem("Mock Test 2","",[]); 
      }

      showSync("✓ Data dimuat", "#839958");
    }

    boot();
  </script>
</body>
</html>
