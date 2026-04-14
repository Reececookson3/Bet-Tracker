# Bet-Tracker
Way to track all my bets.
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bet Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;600;700;800&family=Barlow:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f; --surface: #13131a; --surface2: #1c1c27; --border: #2a2a3a;
    --accent: #c8f135; --accent2: #35f1c8; --red: #f13535; --gold: #f1c835;
    --text: #e8e8f0; --muted: #6b6b80;
    --horse: #f1c835; --nba: #f17435; --football: #35c8f1; --other: #c835f1;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: 'Barlow', sans-serif; font-weight: 300; min-height: 100vh; padding: 0 0 80px; }

  header { background: var(--surface); border-bottom: 1px solid var(--border); padding: 20px 20px 16px; position: sticky; top: 0; z-index: 100; }
  header h1 { font-family: 'Barlow Condensed', sans-serif; font-weight: 800; font-size: 26px; letter-spacing: 2px; text-transform: uppercase; color: var(--accent); }
  header p { font-size: 12px; color: var(--muted); margin-top: 2px; letter-spacing: 1px; text-transform: uppercase; }

  .stats-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1px; background: var(--border); border-bottom: 1px solid var(--border); }
  .stat { background: var(--surface); padding: 14px 16px; text-align: center; }
  .stat-val { font-family: 'Barlow Condensed', sans-serif; font-size: 22px; font-weight: 700; line-height: 1; }
  .stat-val.pos { color: var(--accent); } .stat-val.neg { color: var(--red); } .stat-val.neutral { color: var(--text); }
  .stat-label { font-size: 9px; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; margin-top: 4px; }

  .tabs { display: flex; background: var(--surface); border-bottom: 1px solid var(--border); overflow-x: auto; }
  .tab { flex: 1; min-width: 65px; padding: 14px 4px; text-align: center; font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; color: var(--muted); cursor: pointer; border-bottom: 2px solid transparent; transition: all 0.2s; border: none; background: none; white-space: nowrap; }
  .tab.active { color: var(--accent); border-bottom: 2px solid var(--accent); }
  .tab:hover:not(.active) { color: var(--text); }

  .panel { display: none; padding: 16px; } .panel.active { display: block; }

  .bet-type-toggle { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 16px; }
  .type-btn { padding: 12px; border: 1px solid var(--border); border-radius: 6px; background: var(--surface2); color: var(--muted); font-family: 'Barlow Condensed', sans-serif; font-size: 14px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; cursor: pointer; transition: all 0.2s; text-align: center; }
  .type-btn.active { border-color: var(--accent); color: var(--accent); background: rgba(200,241,53,0.08); }

  .form-card { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 16px; margin-bottom: 16px; }
  .form-card h3 { font-family: 'Barlow Condensed', sans-serif; font-size: 14px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); margin-bottom: 14px; }

  .sport-selector { display: grid; grid-template-columns: repeat(4, 1fr); gap: 6px; margin-bottom: 14px; }
  .sport-btn { padding: 10px 4px; border: 1px solid var(--border); border-radius: 6px; background: var(--surface2); color: var(--muted); font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; cursor: pointer; transition: all 0.2s; text-align: center; }
  .sport-btn.active-horse { border-color: var(--horse); color: var(--horse); background: rgba(241,200,53,0.08); }
  .sport-btn.active-nba { border-color: var(--nba); color: var(--nba); background: rgba(241,116,53,0.08); }
  .sport-btn.active-football { border-color: var(--football); color: var(--football); background: rgba(53,200,241,0.08); }
  .sport-btn.active-other { border-color: var(--other); color: var(--other); background: rgba(200,53,241,0.08); }

  .fields { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .field { display: flex; flex-direction: column; gap: 5px; }
  .field.full { grid-column: 1 / -1; }

  label { font-size: 10px; font-weight: 500; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; }

  input, select, textarea { background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; color: var(--text); font-family: 'Barlow', sans-serif; font-size: 15px; padding: 10px 12px; width: 100%; outline: none; transition: border-color 0.2s; -webkit-appearance: none; }
  input:focus, select:focus, textarea:focus { border-color: var(--accent); }
  select option { background: var(--surface2); }
  textarea { resize: vertical; min-height: 60px; font-size: 14px; }

  /* EACH WAY */
  .ew-toggle { display: flex; align-items: center; gap: 10px; padding: 10px 12px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; cursor: pointer; margin-bottom: 10px; }
  .ew-toggle input[type=checkbox] { width: 18px; height: 18px; accent-color: var(--horse); cursor: pointer; flex-shrink: 0; }
  .ew-toggle-label { font-family: 'Barlow Condensed', sans-serif; font-size: 13px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; color: var(--muted); }
  .ew-toggle.active .ew-toggle-label { color: var(--horse); }
  .ew-fields { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .ew-info { font-size: 11px; color: var(--horse); padding: 8px 10px; background: rgba(241,200,53,0.07); border: 1px solid rgba(241,200,53,0.2); border-radius: 6px; line-height: 1.5; }

  .result-btns { display: grid; grid-template-columns: repeat(4, 1fr); gap: 6px; margin-top: 2px; }
  .result-btn { padding: 10px 4px; border-radius: 6px; border: 1px solid var(--border); background: var(--surface2); color: var(--muted); font-family: 'Barlow Condensed', sans-serif; font-size: 12px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; cursor: pointer; transition: all 0.2s; }
  .result-btn.sel-win { border-color: var(--accent); color: var(--accent); background: rgba(200,241,53,0.08); }
  .result-btn.sel-loss { border-color: var(--red); color: var(--red); background: rgba(241,53,53,0.08); }
  .result-btn.sel-void { border-color: var(--muted); color: var(--text); background: rgba(107,107,128,0.15); }
  .result-btn.sel-pending { border-color: var(--gold); color: var(--gold); background: rgba(241,200,53,0.08); }
  /* EW results — 5 buttons */
  .result-btns-ew { display: grid; grid-template-columns: repeat(5, 1fr); gap: 5px; margin-top: 2px; }
  .result-btn.sel-ew-placed { border-color: var(--horse); color: var(--horse); background: rgba(241,200,53,0.08); }

  .add-btn { width: 100%; margin-top: 14px; padding: 14px; background: var(--accent); color: #0a0a0f; border: none; border-radius: 6px; font-family: 'Barlow Condensed', sans-serif; font-size: 16px; font-weight: 800; letter-spacing: 2px; text-transform: uppercase; cursor: pointer; transition: opacity 0.2s; }
  .add-btn:hover { opacity: 0.88; }

  /* ACCA */
  .acca-section { background: var(--surface2); border: 1px solid var(--border); border-radius: 8px; padding: 14px; margin-bottom: 14px; }
  .acca-section h4 { font-family: 'Barlow Condensed', sans-serif; font-size: 12px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); margin-bottom: 12px; }
  .acca-leg { background: var(--surface); border: 1px solid var(--border); border-radius: 6px; padding: 10px 12px; margin-bottom: 8px; }
  .acca-leg.voided { opacity: 0.5; border-color: var(--muted); }
  .acca-leg-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
  .acca-leg-num { font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 700; letter-spacing: 1px; color: var(--accent); text-transform: uppercase; }
  .acca-leg-num.voided-label { color: var(--muted); }
  .leg-actions { display: flex; gap: 6px; align-items: center; }
  .void-leg-btn { background: none; border: 1px solid var(--border); border-radius: 4px; color: var(--muted); font-family: 'Barlow Condensed', sans-serif; font-size: 10px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; padding: 3px 7px; cursor: pointer; transition: all 0.2s; }
  .void-leg-btn.active { border-color: var(--gold); color: var(--gold); }
  .remove-leg-btn { background: none; border: none; color: var(--red); font-size: 16px; cursor: pointer; padding: 0 4px; line-height: 1; }
  .leg-fields { display: grid; grid-template-columns: 1fr 80px; gap: 8px; }
  .add-leg-btn { width: 100%; padding: 10px; background: transparent; border: 1px dashed var(--border); border-radius: 6px; color: var(--accent2); font-family: 'Barlow Condensed', sans-serif; font-size: 13px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; cursor: pointer; transition: all 0.2s; margin-top: 4px; }
  .add-leg-btn:hover { border-color: var(--accent2); }
  .acca-odds-display { display: flex; justify-content: space-between; align-items: center; padding: 10px 12px; background: rgba(200,241,53,0.06); border: 1px solid rgba(200,241,53,0.2); border-radius: 6px; margin-top: 10px; }
  .acca-odds-label { font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 1px; color: var(--muted); text-transform: uppercase; }
  .acca-odds-val { font-family: 'Barlow Condensed', sans-serif; font-size: 18px; font-weight: 800; color: var(--accent); }

  /* BET CARDS */
  .bet-card { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 14px; margin-bottom: 8px; position: relative; overflow: hidden; }
  .bet-card::before { content: ''; position: absolute; left: 0; top: 0; bottom: 0; width: 3px; }
  .bet-card.horse::before { background: var(--horse); }
  .bet-card.nba::before { background: var(--nba); }
  .bet-card.football::before { background: var(--football); }
  .bet-card.other::before { background: var(--other); }
  .bet-card.acca::before { background: linear-gradient(180deg, var(--accent), var(--accent2)); }
  .bet-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 8px; }
  .bet-name { font-family: 'Barlow Condensed', sans-serif; font-size: 17px; font-weight: 700; color: var(--text); line-height: 1.1; flex: 1; padding-right: 8px; }
  .bet-result { font-family: 'Barlow Condensed', sans-serif; font-size: 12px; font-weight: 700; letter-spacing: 1px; padding: 3px 8px; border-radius: 4px; text-transform: uppercase; flex-shrink: 0; }
  .bet-result.win { background: rgba(200,241,53,0.12); color: var(--accent); }
  .bet-result.loss { background: rgba(241,53,53,0.12); color: var(--red); }
  .bet-result.void { background: rgba(107,107,128,0.15); color: var(--muted); }
  .bet-result.pending { background: rgba(241,200,53,0.12); color: var(--gold); }
  .bet-result.placed { background: rgba(241,200,53,0.12); color: var(--horse); }
  .bet-meta { display: flex; gap: 12px; flex-wrap: wrap; }
  .bet-meta span { font-size: 12px; color: var(--muted); }
  .bet-meta strong { color: var(--text); font-weight: 500; }
  .bet-pl { font-family: 'Barlow Condensed', sans-serif; font-size: 15px; font-weight: 700; margin-top: 8px; }
  .bet-pl.pos { color: var(--accent); } .bet-pl.neg { color: var(--red); } .bet-pl.neutral { color: var(--muted); }
  .bet-notes { font-size: 12px; color: var(--muted); margin-top: 6px; font-style: italic; line-height: 1.4; }
  .bet-actions { display: flex; gap: 8px; margin-top: 10px; flex-wrap: wrap; }
  .action-btn { font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; padding: 5px 10px; border-radius: 4px; border: 1px solid var(--border); background: transparent; color: var(--muted); cursor: pointer; }
  .action-btn.settle { border-color: var(--accent2); color: var(--accent2); }
  .action-btn.delete { border-color: var(--red); color: var(--red); }
  .acca-legs-list { margin-top: 8px; border-top: 1px solid var(--border); padding-top: 8px; }
  .acca-leg-item { display: flex; justify-content: space-between; font-size: 12px; padding: 3px 0; border-bottom: 1px solid rgba(255,255,255,0.04); }
  .acca-leg-item:last-child { border-bottom: none; }
  .acca-leg-item.voided-leg .acca-leg-name { text-decoration: line-through; color: var(--muted); }
  .acca-leg-name { color: var(--text); flex: 1; }
  .acca-leg-odds { color: var(--accent2); font-family: 'Barlow Condensed', sans-serif; font-weight: 600; margin-left: 8px; }
  .acca-leg-odds.voided-odds { color: var(--muted); text-decoration: line-through; }

  .empty-state { text-align: center; padding: 40px 20px; color: var(--muted); }
  .empty-state .icon { font-size: 36px; margin-bottom: 12px; }
  .empty-state p { font-size: 14px; line-height: 1.5; }

  /* STATS */
  .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 16px 14px; }
  .stat-card h4 { font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); margin-bottom: 8px; }
  .stat-card .big { font-family: 'Barlow Condensed', sans-serif; font-size: 28px; font-weight: 800; line-height: 1; }
  .stat-card .big.pos { color: var(--accent); } .stat-card .big.neg { color: var(--red); }
  .stat-card .sub { font-size: 11px; color: var(--muted); margin-top: 4px; }

  .sport-breakdown { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 16px 14px; margin-top: 10px; }
  .sport-breakdown h4 { font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); margin-bottom: 12px; }
  .sport-row { display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px solid var(--border); }
  .sport-row:last-child { border-bottom: none; }
  .sport-dot { width: 8px; height: 8px; border-radius: 50%; margin-right: 8px; display: inline-block; flex-shrink: 0; }
  .sport-row-left { display: flex; align-items: center; font-size: 14px; }
  .sport-row-right { font-family: 'Barlow Condensed', sans-serif; font-size: 15px; font-weight: 700; }
  .sport-row-right.pos { color: var(--accent); } .sport-row-right.neg { color: var(--red); } .sport-row-right.neutral { color: var(--muted); }

  /* MONTHLY P&L */
  .monthly-breakdown { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 16px 14px; margin-top: 10px; }
  .monthly-breakdown h4 { font-family: 'Barlow Condensed', sans-serif; font-size: 11px; font-weight: 600; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); margin-bottom: 12px; }
  .month-bar-row { margin-bottom: 10px; }
  .month-bar-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 4px; }
  .month-label { font-family: 'Barlow Condensed', sans-serif; font-size: 13px; font-weight: 600; color: var(--text); }
  .month-pl { font-family: 'Barlow Condensed', sans-serif; font-size: 14px; font-weight: 700; }
  .month-pl.pos { color: var(--accent); } .month-pl.neg { color: var(--red); } .month-pl.neutral { color: var(--muted); }
  .month-bar-track { height: 6px; background: var(--surface2); border-radius: 3px; overflow: hidden; }
  .month-bar-fill { height: 100%; border-radius: 3px; transition: width 0.4s ease; min-width: 2px; }
  .month-bar-fill.pos { background: var(--accent); } .month-bar-fill.neg { background: var(--red); }
  .month-sub { font-size: 11px; color: var(--muted); margin-top: 2px; }
  .no-monthly { font-size: 13px; color: var(--muted); text-align: center; padding: 20px 0; }

  /* MODAL */
  .modal-bg { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 200; align-items: flex-end; justify-content: center; }
  .modal-bg.open { display: flex; }
  .modal { background: var(--surface); border: 1px solid var(--border); border-radius: 16px 16px 0 0; padding: 24px 20px 40px; width: 100%; max-width: 500px; }
  .modal h3 { font-family: 'Barlow Condensed', sans-serif; font-size: 18px; font-weight: 800; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 16px; }
  .modal-btns { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 16px; }
  .modal-btn { padding: 14px; border-radius: 6px; border: none; font-family: 'Barlow Condensed', sans-serif; font-size: 15px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; cursor: pointer; }
  .modal-btn.win-btn { background: var(--accent); color: #0a0a0f; }
  .modal-btn.loss-btn { background: var(--red); color: #fff; }
  .modal-btn.placed-btn { background: rgba(241,200,53,0.15); color: var(--horse); border: 1px solid var(--horse); }
  .modal-btn.void-btn { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
  .modal-btn.cancel-btn { background: transparent; color: var(--muted); border: 1px solid var(--border); }
  .modal-ew-note { font-size: 12px; color: var(--horse); background: rgba(241,200,53,0.07); border: 1px solid rgba(241,200,53,0.2); border-radius: 6px; padding: 8px 10px; margin-top: 10px; line-height: 1.5; }

  .filter-row { display: flex; gap: 6px; margin-bottom: 14px; overflow-x: auto; padding-bottom: 2px; }
  .filter-btn { flex-shrink: 0; padding: 6px 12px; border-radius: 20px; border: 1px solid var(--border); background: var(--surface); color: var(--muted); font-family: 'Barlow Condensed', sans-serif; font-size: 12px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; cursor: pointer; transition: all 0.2s; }
  .filter-btn.active { border-color: var(--accent); color: var(--accent); background: rgba(200,241,53,0.08); }

  .export-btn { width: 100%; padding: 13px; background: var(--surface); border: 1px solid var(--border); border-radius: 6px; color: var(--text); font-family: 'Barlow Condensed', sans-serif; font-size: 14px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase; cursor: pointer; margin-top: 14px; transition: border-color 0.2s; }
  .export-btn:hover { border-color: var(--accent2); color: var(--accent2); }

  .sport-tag { font-size: 10px; font-family: 'Barlow Condensed', sans-serif; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; padding: 2px 6px; border-radius: 3px; margin-left: 6px; }
  .sport-tag.horse { background: rgba(241,200,53,0.15); color: var(--horse); }
  .sport-tag.nba { background: rgba(241,116,53,0.15); color: var(--nba); }
  .sport-tag.football { background: rgba(53,200,241,0.15); color: var(--football); }
  .sport-tag.other { background: rgba(200,53,241,0.15); color: var(--other); }
  .sport-tag.acca { background: rgba(200,241,53,0.1); color: var(--accent); }
  .sport-tag.ew { background: rgba(241,200,53,0.15); color: var(--horse); }

  .toast { position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%) translateY(100px); background: var(--accent); color: #0a0a0f; font-family: 'Barlow Condensed', sans-serif; font-size: 14px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; padding: 10px 20px; border-radius: 6px; z-index: 300; transition: transform 0.3s ease; white-space: nowrap; }
  .toast.show { transform: translateX(-50%) translateY(0); }
</style>
</head>
<body>

<header>
  <h1>⚡ Bet Tracker</h1>
  <p id="bankroll-display">P&L: £0.00 · 0 pending</p>
</header>

<div class="stats-bar">
  <div class="stat"><div class="stat-val neutral" id="stat-total">0</div><div class="stat-label">Bets</div></div>
  <div class="stat"><div class="stat-val neutral" id="stat-wr">—</div><div class="stat-label">Win Rate</div></div>
  <div class="stat"><div class="stat-val neutral" id="stat-pl">£0</div><div class="stat-label">P&L</div></div>
  <div class="stat"><div class="stat-val neutral" id="stat-roi">—</div><div class="stat-label">ROI</div></div>
</div>

<div class="tabs">
  <button class="tab active" onclick="showTab('add',this)">+ Add</button>
  <button class="tab" onclick="showTab('bets',this)">My Bets</button>
  <button class="tab" onclick="showTab('stats',this)">Stats</button>
  <button class="tab" onclick="showTab('monthly',this)">Monthly</button>
</div>

<!-- ADD BET PANEL -->
<div class="panel active" id="panel-add">
  <div class="bet-type-toggle">
    <button class="type-btn active" id="type-single" onclick="setBetType('single')">🎯 Single</button>
    <button class="type-btn" id="type-acca" onclick="setBetType('acca')">🔗 Acca / Multiple</button>
  </div>

  <!-- SINGLE FORM -->
  <div id="single-form">
    <div class="form-card">
      <h3>Sport</h3>
      <div class="sport-selector">
        <button class="sport-btn" id="btn-horse" onclick="setSport('horse')">🐎 Racing</button>
        <button class="sport-btn" id="btn-nba" onclick="setSport('nba')">🏀 NBA</button>
        <button class="sport-btn" id="btn-football" onclick="setSport('football')">⚽ Football</button>
        <button class="sport-btn" id="btn-other" onclick="setSport('other')">🎲 Other</button>
      </div>

      <!-- Each-way toggle (only shows for horse) -->
      <div id="ew-section" style="display:none">
        <label class="ew-toggle" id="ew-toggle-label" onclick="toggleEW()">
          <input type="checkbox" id="ew-checkbox" onclick="event.stopPropagation(); toggleEW()">
          <span class="ew-toggle-label">Each Way Bet</span>
        </label>
        <div id="ew-fields" style="display:none">
          <div class="ew-fields">
            <div class="field">
              <label>Place Terms (e.g. 1/4)</label>
              <select id="ew-terms">
                <option value="0.25">1/4 odds</option>
                <option value="0.2">1/5 odds</option>
                <option value="0.333">1/3 odds</option>
              </select>
            </div>
            <div class="field">
              <label>Places Paid</label>
              <select id="ew-places">
                <option value="2">2 places</option>
                <option value="3">3 places</option>
                <option value="4">4 places</option>
                <option value="5">5 places</option>
              </select>
            </div>
          </div>
          <div class="ew-info" id="ew-info">Stake doubles — e.g. £5 EW = £10 total (£5 win + £5 place)</div>
        </div>
      </div>

      <div class="fields">
        <div class="field full">
          <label>Bet Description</label>
          <input type="text" id="bet-name" placeholder="e.g. Frankel 3:30 Ascot / Lakers ML / Arsenal to win">
        </div>
        <div class="field full" id="field-race" style="display:none">
          <label>Race / Venue</label>
          <input type="text" id="bet-race" placeholder="e.g. 3:30 Ascot">
        </div>
        <div class="field full" id="field-other-sport" style="display:none">
          <label>Sport / Event type</label>
          <input type="text" id="bet-other-sport" placeholder="e.g. Tennis, Darts, Cricket">
        </div>
        <div class="field">
          <label>Market</label>
          <input type="text" id="bet-market" placeholder="Win / BTTS / Points">
        </div>
        <div class="field">
          <label>Odds (decimal)</label>
          <input type="number" id="bet-odds" placeholder="3.50" step="0.01" min="1" oninput="updateEWInfo()">
        </div>
        <div class="field">
          <label id="stake-label">Stake (£)</label>
          <input type="number" id="bet-stake" placeholder="5.00" step="0.50" min="0.01" oninput="updateEWInfo()">
        </div>
        <div class="field">
          <label>Date</label>
          <input type="date" id="bet-date">
        </div>
        <div class="field full">
          <label>Bookmaker</label>
          <input type="text" id="bet-bookie" placeholder="Bet365">
        </div>
        <div class="field full" id="result-field">
          <label>Result</label>
          <div class="result-btns" id="result-btns-single">
            <button class="result-btn sel-pending" id="rbtn-pending" onclick="setResult('pending')">Pending</button>
            <button class="result-btn" id="rbtn-win" onclick="setResult('win')">Win</button>
            <button class="result-btn" id="rbtn-loss" onclick="setResult('loss')">Loss</button>
            <button class="result-btn" id="rbtn-void" onclick="setResult('void')">Void</button>
          </div>
          <!-- EW result buttons (5 options) -->
          <div class="result-btns-ew" id="result-btns-ew" style="display:none">
            <button class="result-btn sel-pending" id="ewrbtn-pending" onclick="setResult('pending')">Pending</button>
            <button class="result-btn" id="ewrbtn-win" onclick="setResult('win')">Win</button>
            <button class="result-btn" id="ewrbtn-ew-placed" onclick="setResult('ew-placed')">Placed</button>
            <button class="result-btn" id="ewrbtn-loss" onclick="setResult('loss')">Loss</button>
            <button class="result-btn" id="ewrbtn-void" onclick="setResult('void')">Void</button>
          </div>
        </div>
        <div class="field full">
          <label>Notes (optional)</label>
          <textarea id="bet-notes" placeholder="Tip source, reasoning, form notes..."></textarea>
        </div>
      </div>
      <button class="add-btn" onclick="addSingleBet()">ADD BET</button>
    </div>
  </div>

  <!-- ACCA FORM -->
  <div id="acca-form" style="display:none">
    <div class="form-card">
      <h3>Acca Details</h3>
      <div class="fields">
        <div class="field full">
          <label>Acca Name (optional)</label>
          <input type="text" id="acca-name" placeholder="e.g. Saturday 4-fold">
        </div>
        <div class="field">
          <label>Stake (£)</label>
          <input type="number" id="acca-stake" placeholder="5.00" step="0.50" min="0.01" oninput="updateAccaOdds()">
        </div>
        <div class="field">
          <label>Date</label>
          <input type="date" id="acca-date">
        </div>
        <div class="field full">
          <label>Bookmaker</label>
          <input type="text" id="acca-bookie" placeholder="Bet365">
        </div>
        <div class="field full">
          <label>Notes (optional)</label>
          <textarea id="acca-notes" placeholder="Reasoning, tip source..."></textarea>
        </div>
      </div>
    </div>
    <div class="acca-section">
      <h4>Selections — tap Void on any leg to remove it from odds</h4>
      <div id="acca-legs-container"></div>
      <button class="add-leg-btn" onclick="addLeg()">+ Add Selection</button>
      <div class="acca-odds-display">
        <span class="acca-odds-label">Combined Odds</span>
        <span class="acca-odds-val" id="acca-combined-odds">—</span>
      </div>
    </div>
    <div class="form-card">
      <h3>Result</h3>
      <div class="result-btns">
        <button class="result-btn sel-pending" id="arbtn-pending" onclick="setAccaResult('pending')">Pending</button>
        <button class="result-btn" id="arbtn-win" onclick="setAccaResult('win')">Win</button>
        <button class="result-btn" id="arbtn-loss" onclick="setAccaResult('loss')">Loss</button>
        <button class="result-btn" id="arbtn-void" onclick="setAccaResult('void')">Void</button>
      </div>
      <button class="add-btn" onclick="addAccaBet()">ADD ACCA</button>
    </div>
  </div>
</div>

<!-- BETS PANEL -->
<div class="panel" id="panel-bets">
  <div class="filter-row">
    <button class="filter-btn active" onclick="setFilter('all',this)">All</button>
    <button class="filter-btn" onclick="setFilter('pending',this)">Pending</button>
    <button class="filter-btn" onclick="setFilter('acca',this)">🔗 Accas</button>
    <button class="filter-btn" onclick="setFilter('horse',this)">🐎 Racing</button>
    <button class="filter-btn" onclick="setFilter('nba',this)">🏀 NBA</button>
    <button class="filter-btn" onclick="setFilter('football',this)">⚽ Football</button>
    <button class="filter-btn" onclick="setFilter('other',this)">🎲 Other</button>
    <button class="filter-btn" onclick="setFilter('win',this)">Wins</button>
    <button class="filter-btn" onclick="setFilter('loss',this)">Losses</button>
  </div>
  <div id="bets-list"></div>
  <button class="export-btn" onclick="exportCSV()">⬇ Export to CSV</button>
</div>

<!-- STATS PANEL -->
<div class="panel" id="panel-stats">
  <div class="stats-grid">
    <div class="stat-card"><h4>Total P&L</h4><div class="big neutral" id="s-pl">£0.00</div><div class="sub" id="s-pl-sub">No settled bets yet</div></div>
    <div class="stat-card"><h4>Win Rate</h4><div class="big neutral" id="s-wr">—</div><div class="sub" id="s-wr-sub">0W / 0L</div></div>
    <div class="stat-card"><h4>ROI</h4><div class="big neutral" id="s-roi">—</div><div class="sub">Return on investment</div></div>
    <div class="stat-card"><h4>Avg Odds</h4><div class="big neutral" id="s-avgodds">—</div><div class="sub">On winning bets</div></div>
    <div class="stat-card"><h4>Biggest Win</h4><div class="big pos" id="s-bigwin">—</div><div class="sub" id="s-bigwin-sub"></div></div>
    <div class="stat-card"><h4>Pending</h4><div class="big neutral" id="s-pending">0</div><div class="sub">bets to settle</div></div>
  </div>
  <div class="sport-breakdown">
    <h4>By Sport / Type</h4>
    <div class="sport-row"><div class="sport-row-left"><span class="sport-dot" style="background:var(--horse)"></span>Horse Racing</div><div class="sport-row-right neutral" id="sb-horse">£0.00</div></div>
    <div class="sport-row"><div class="sport-row-left"><span class="sport-dot" style="background:var(--nba)"></span>NBA</div><div class="sport-row-right neutral" id="sb-nba">£0.00</div></div>
    <div class="sport-row"><div class="sport-row-left"><span class="sport-dot" style="background:var(--football)"></span>Football</div><div class="sport-row-right neutral" id="sb-football">£0.00</div></div>
    <div class="sport-row"><div class="sport-row-left"><span class="sport-dot" style="background:var(--other)"></span>Other</div><div class="sport-row-right neutral" id="sb-other">£0.00</div></div>
    <div class="sport-row"><div class="sport-row-left"><span class="sport-dot" style="background:var(--accent)"></span>Accas</div><div class="sport-row-right neutral" id="sb-acca">£0.00</div></div>
  </div>
</div>

<!-- MONTHLY PANEL -->
<div class="panel" id="panel-monthly">
  <div class="monthly-breakdown">
    <h4>Monthly P&L</h4>
    <div id="monthly-chart"></div>
  </div>
  <div class="stats-grid" style="margin-top:10px">
    <div class="stat-card"><h4>Best Month</h4><div class="big pos" id="m-best">—</div><div class="sub" id="m-best-sub"></div></div>
    <div class="stat-card"><h4>Worst Month</h4><div class="big neg" id="m-worst">—</div><div class="sub" id="m-worst-sub"></div></div>
    <div class="stat-card"><h4>Profitable Months</h4><div class="big neutral" id="m-profit-count">—</div><div class="sub" id="m-profit-sub">out of 0</div></div>
    <div class="stat-card"><h4>Avg / Month</h4><div class="big neutral" id="m-avg">—</div><div class="sub">settled bets only</div></div>
  </div>
</div>

<!-- SETTLE MODAL -->
<div class="modal-bg" id="settle-modal">
  <div class="modal">
    <h3>Settle Bet</h3>
    <p id="settle-bet-name" style="font-size:15px;color:var(--muted);margin-bottom:4px;"></p>
    <div class="field" style="margin-top:12px;">
      <label>Return amount (£) — auto-filled, adjust if needed</label>
      <input type="number" id="settle-return" placeholder="0.00" step="0.01" min="0">
    </div>
    <div id="settle-ew-note" class="modal-ew-note" style="display:none"></div>
    <div class="modal-btns" id="settle-btns">
      <button class="modal-btn win-btn" onclick="confirmSettle('win')">✓ Win</button>
      <button class="modal-btn loss-btn" onclick="confirmSettle('loss')">✗ Loss</button>
      <button class="modal-btn void-btn" onclick="confirmSettle('void')">— Void</button>
      <button class="modal-btn cancel-btn" onclick="closeModal()">Cancel</button>
    </div>
    <!-- EW settle buttons -->
    <div id="settle-ew-btns" style="display:none">
      <div class="modal-btns">
        <button class="modal-btn win-btn" onclick="confirmSettle('win')">✓ Win (both)</button>
        <button class="modal-btn placed-btn" onclick="confirmSettle('ew-placed')">🏅 Placed only</button>
      </div>
      <div class="modal-btns" style="margin-top:8px">
        <button class="modal-btn loss-btn" onclick="confirmSettle('loss')">✗ Loss</button>
        <button class="modal-btn cancel-btn" onclick="closeModal()">Cancel</button>
      </div>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
  let bets = JSON.parse(localStorage.getItem('bets') || '[]');
  let currentSport = '';
  let currentResult = 'pending';
  let currentAccaResult = 'pending';
  let currentFilter = 'all';
  let currentBetType = 'single';
  let isEW = false;
  let settleIndex = null;
  let accaLegs = [];

  const MONTHS = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];

  document.getElementById('bet-date').valueAsDate = new Date();
  document.getElementById('acca-date').valueAsDate = new Date();
  addLeg(); addLeg();

  function showToast(msg) {
    const t = document.getElementById('toast');
    t.textContent = msg; t.classList.add('show');
    setTimeout(() => t.classList.remove('show'), 2000);
  }

  function showTab(tab, el) {
    document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.getElementById('panel-' + tab).classList.add('active');
    el.classList.add('active');
    if (tab === 'bets') renderBets();
    if (tab === 'stats') renderStats();
    if (tab === 'monthly') renderMonthly();
  }

  function setBetType(type) {
    currentBetType = type;
    document.getElementById('type-single').className = 'type-btn' + (type === 'single' ? ' active' : '');
    document.getElementById('type-acca').className = 'type-btn' + (type === 'acca' ? ' active' : '');
    document.getElementById('single-form').style.display = type === 'single' ? 'block' : 'none';
    document.getElementById('acca-form').style.display = type === 'acca' ? 'block' : 'none';
  }

  function setSport(sport) {
    currentSport = sport;
    ['horse','nba','football','other'].forEach(s => document.getElementById('btn-' + s).className = 'sport-btn');
    document.getElementById('btn-' + sport).className = 'sport-btn active-' + sport;
    document.getElementById('field-race').style.display = sport === 'horse' ? 'flex' : 'none';
    document.getElementById('field-other-sport').style.display = sport === 'other' ? 'flex' : 'none';
    document.getElementById('ew-section').style.display = sport === 'horse' ? 'block' : 'none';
    if (sport !== 'horse') { isEW = false; document.getElementById('ew-checkbox').checked = false; document.getElementById('ew-fields').style.display = 'none'; document.getElementById('ew-toggle-label').classList.remove('active'); showEWResultBtns(false); }
    const m = document.getElementById('bet-market');
    if (sport === 'horse') m.placeholder = 'Win / Each Way / Place';
    else if (sport === 'nba') m.placeholder = 'ML / Spread / Points Prop';
    else if (sport === 'football') m.placeholder = 'Match Result / BTTS / Goals';
    else m.placeholder = 'Market / Bet type';
  }

  function toggleEW() {
    isEW = !isEW;
    document.getElementById('ew-checkbox').checked = isEW;
    document.getElementById('ew-fields').style.display = isEW ? 'grid' : 'none';
    document.getElementById('ew-toggle-label').className = 'ew-toggle' + (isEW ? ' active' : '');
    showEWResultBtns(isEW);
    updateEWInfo();
  }

  function showEWResultBtns(show) {
    document.getElementById('result-btns-single').style.display = show ? 'none' : 'grid';
    document.getElementById('result-btns-ew').style.display = show ? 'grid' : 'none';
    if (!show && !['pending','win','loss','void'].includes(currentResult)) { currentResult = 'pending'; }
    setResult(currentResult);
  }

  function updateEWInfo() {
    if (!isEW) return;
    const odds = parseFloat(document.getElementById('bet-odds').value) || 0;
    const stake = parseFloat(document.getElementById('bet-stake').value) || 0;
    const terms = parseFloat(document.getElementById('ew-terms').value);
    const placeOdds = ((odds - 1) * terms) + 1;
    const total = stake * 2;
    const winReturn = (odds * stake).toFixed(2);
    const placeReturn = (placeOdds * stake).toFixed(2);
    document.getElementById('ew-info').textContent =
      `Total stake: £${total.toFixed(2)} | Win return: £${winReturn} | Place-only return: £${placeReturn}`;
  }

  function calcEWReturn(bet, result) {
    const stake = parseFloat(bet.stake);
    const odds = parseFloat(bet.odds);
    const terms = parseFloat(bet.ewTerms || 0.25);
    const placeOdds = ((odds - 1) * terms) + 1;
    if (result === 'win') return (odds * stake) + (placeOdds * stake);
    if (result === 'ew-placed') return placeOdds * stake;
    return 0;
  }

  function setResult(r) {
    currentResult = r;
    if (isEW) {
      ['pending','win','ew-placed','loss','void'].forEach(x => {
        const btn = document.getElementById('ewrbtn-' + x);
        if (btn) btn.className = 'result-btn' + (x === r ? ' sel-' + (x === 'ew-placed' ? 'ew-placed' : x) : '');
      });
    } else {
      ['pending','win','loss','void'].forEach(x => {
        const btn = document.getElementById('rbtn-' + x);
        if (btn) btn.className = 'result-btn' + (x === r ? ' sel-' + r : '');
      });
    }
  }

  function setAccaResult(r) {
    currentAccaResult = r;
    ['pending','win','loss','void'].forEach(x => {
      document.getElementById('arbtn-' + x).className = 'result-btn' + (x === r ? ' sel-' + r : '');
    });
  }

  // ---- ACCA LEGS ----
  function addLeg() {
    accaLegs.push({ name: '', odds: '', voided: false });
    renderLegs();
  }

  function removeLeg(i) {
    if (accaLegs.length <= 2) { showToast('Need at least 2 legs'); return; }
    accaLegs.splice(i, 1);
    renderLegs(); updateAccaOdds();
  }

  function toggleVoidLeg(i) {
    accaLegs[i].voided = !accaLegs[i].voided;
    renderLegs(); updateAccaOdds();
  }

  function renderLegs() {
    document.getElementById('acca-legs-container').innerHTML = accaLegs.map((leg, i) => `
      <div class="acca-leg${leg.voided ? ' voided' : ''}">
        <div class="acca-leg-header">
          <span class="acca-leg-num${leg.voided ? ' voided-label' : ''}">Leg ${i+1}${leg.voided ? ' — VOIDED' : ''}</span>
          <div class="leg-actions">
            <button class="void-leg-btn${leg.voided ? ' active' : ''}" onclick="toggleVoidLeg(${i})">${leg.voided ? 'Unvoid' : 'Void'}</button>
            <button class="remove-leg-btn" onclick="removeLeg(${i})">✕</button>
          </div>
        </div>
        <div class="leg-fields">
          <input type="text" placeholder="Selection (e.g. Arsenal to win)" value="${leg.name}" oninput="accaLegs[${i}].name=this.value">
          <input type="number" placeholder="Odds" step="0.01" min="1" value="${leg.odds}" oninput="accaLegs[${i}].odds=this.value; updateAccaOdds()" ${leg.voided ? 'disabled' : ''}>
        </div>
      </div>`).join('');
  }

  function updateAccaOdds() {
    const activeLegs = accaLegs.filter(l => !l.voided && parseFloat(l.odds) >= 1);
    if (activeLegs.length < 1) { document.getElementById('acca-combined-odds').textContent = '—'; return; }
    const combined = activeLegs.reduce((a, l) => a * parseFloat(l.odds), 1);
    const voidCount = accaLegs.filter(l => l.voided).length;
    const stake = parseFloat(document.getElementById('acca-stake').value) || 0;
    const ret = stake > 0 ? `  ·  Pot. return £${(combined * stake).toFixed(2)}` : '';
    const voidNote = voidCount > 0 ? ` (${voidCount} voided)` : '';
    document.getElementById('acca-combined-odds').textContent = combined.toFixed(2) + voidNote + ret;
  }

  // ---- P&L CALC ----
  function calcPL(bet) {
    if (bet.result === 'pending') return null;
    if (bet.result === 'void') return 0;
    if (bet.result === 'loss') return -parseFloat(bet.stake) * (bet.isEW ? 2 : 1);
    if (bet.result === 'ew-placed') {
      const ret = bet.actualReturn > 0 ? bet.actualReturn : calcEWReturn(bet, 'ew-placed');
      return ret - (parseFloat(bet.stake) * 2);
    }
    if (bet.result === 'win' && bet.isEW) {
      const ret = bet.actualReturn > 0 ? bet.actualReturn : calcEWReturn(bet, 'win');
      return ret - (parseFloat(bet.stake) * 2);
    }
    // Regular win
    const ret = bet.actualReturn > 0 ? bet.actualReturn : (parseFloat(bet.odds) * parseFloat(bet.stake));
    return ret - parseFloat(bet.stake);
  }

  // ---- ADD BETS ----
  function addSingleBet() {
    const name = document.getElementById('bet-name').value.trim();
    const odds = parseFloat(document.getElementById('bet-odds').value);
    const stake = parseFloat(document.getElementById('bet-stake').value);
    if (!name) { showToast('Add a bet description'); return; }
    if (!currentSport) { showToast('Select a sport'); return; }
    if (!odds || odds < 1) { showToast('Enter valid odds'); return; }
    if (!stake || stake <= 0) { showToast('Enter a stake'); return; }

    bets.unshift({
      id: Date.now(), type: 'single', name, sport: currentSport,
      otherSport: document.getElementById('bet-other-sport').value.trim(),
      race: document.getElementById('bet-race').value.trim(),
      market: document.getElementById('bet-market').value.trim(),
      odds, stake,
      isEW,
      ewTerms: parseFloat(document.getElementById('ew-terms').value),
      ewPlaces: parseInt(document.getElementById('ew-places').value),
      date: document.getElementById('bet-date').value,
      bookie: document.getElementById('bet-bookie').value.trim(),
      result: currentResult,
      notes: document.getElementById('bet-notes').value.trim(),
      actualReturn: 0
    });

    save(); updateHeader();
    ['bet-name','bet-race','bet-other-sport','bet-market','bet-odds','bet-stake','bet-notes'].forEach(id => document.getElementById(id).value = '');
    document.getElementById('bet-date').valueAsDate = new Date();
    document.getElementById('field-race').style.display = 'none';
    document.getElementById('field-other-sport').style.display = 'none';
    document.getElementById('ew-section').style.display = 'none';
    document.getElementById('ew-fields').style.display = 'none';
    document.getElementById('ew-checkbox').checked = false;
    document.getElementById('ew-toggle-label').classList.remove('active');
    currentSport = ''; currentResult = 'pending'; isEW = false;
    ['horse','nba','football','other'].forEach(s => document.getElementById('btn-' + s).className = 'sport-btn');
    showEWResultBtns(false); setResult('pending');
    showToast('Bet added ✓');
  }

  function addAccaBet() {
    const stake = parseFloat(document.getElementById('acca-stake').value);
    if (!stake || stake <= 0) { showToast('Enter a stake'); return; }
    const filled = accaLegs.filter(l => l.name.trim() && parseFloat(l.odds) >= 1);
    const activeLegs = filled.filter(l => !l.voided);
    if (activeLegs.length < 2 && filled.length < 2) { showToast('Need at least 2 valid legs'); return; }
    const combined = activeLegs.reduce((a, l) => a * parseFloat(l.odds), 1);
    const name = document.getElementById('acca-name').value.trim() || `${filled.length}-Fold Acca`;

    bets.unshift({
      id: Date.now(), type: 'acca', sport: 'acca', name,
      legs: filled.map(l => ({ name: l.name.trim(), odds: parseFloat(l.odds), voided: l.voided })),
      odds: parseFloat(combined.toFixed(2)), stake,
      date: document.getElementById('acca-date').value,
      bookie: document.getElementById('acca-bookie').value.trim(),
      result: currentAccaResult,
      notes: document.getElementById('acca-notes').value.trim(),
      actualReturn: 0
    });

    save(); updateHeader();
    ['acca-name','acca-stake','acca-bookie','acca-notes'].forEach(id => document.getElementById(id).value = '');
    document.getElementById('acca-date').valueAsDate = new Date();
    document.getElementById('acca-combined-odds').textContent = '—';
    currentAccaResult = 'pending'; setAccaResult('pending');
    accaLegs = []; addLeg(); addLeg();
    showToast('Acca added ✓');
  }

  // ---- RENDER ----
  function sportTag(b) {
    if (b.type === 'acca') return `<span class="sport-tag acca">🔗 acca</span>`;
    if (b.sport === 'horse') return `<span class="sport-tag horse">🐎 racing${b.isEW ? ' · E/W' : ''}</span>`;
    if (b.sport === 'nba') return `<span class="sport-tag nba">🏀 nba</span>`;
    if (b.sport === 'football') return `<span class="sport-tag football">⚽ football</span>`;
    return `<span class="sport-tag other">🎲 ${b.otherSport || 'other'}</span>`;
  }

  function setFilter(f, el) {
    currentFilter = f;
    document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
    el.classList.add('active');
    renderBets();
  }

  function renderBets() {
    const list = document.getElementById('bets-list');
    const filtered = bets.filter(b => {
      if (currentFilter === 'all') return true;
      if (currentFilter === 'pending') return b.result === 'pending';
      if (currentFilter === 'acca') return b.type === 'acca';
      if (['horse','nba','football','other'].includes(currentFilter)) return b.sport === currentFilter && b.type !== 'acca';
      return b.result === currentFilter;
    });

    if (!filtered.length) {
      list.innerHTML = `<div class="empty-state"><div class="icon">📋</div><p>No bets here yet.<br>Add your first bet above!</p></div>`;
      return;
    }

    list.innerHTML = filtered.map(b => {
      const pl = calcPL(b);
      const plText = pl === null ? '' : (pl >= 0 ? `+£${pl.toFixed(2)}` : `-£${Math.abs(pl).toFixed(2)}`);
      const plClass = pl === null ? '' : (pl > 0 ? 'pos' : pl < 0 ? 'neg' : 'neutral');
      const ri = bets.indexOf(b);
      const cardClass = b.type === 'acca' ? 'acca' : b.sport;
      const resultLabel = b.result === 'ew-placed' ? 'placed' : b.result;

      const legs = b.type === 'acca' && b.legs ? `
        <div class="acca-legs-list">
          ${b.legs.map(l => `
            <div class="acca-leg-item${l.voided ? ' voided-leg' : ''}">
              <span class="acca-leg-name">${l.name}</span>
              <span class="acca-leg-odds${l.voided ? ' voided-odds' : ''}">@ ${l.odds}${l.voided ? ' (void)' : ''}</span>
            </div>`).join('')}
        </div>` : '';

      const ewNote = b.isEW ? `<div style="font-size:11px;color:var(--horse);margin-top:4px;">Each-Way · ${b.ewTerms ? `1/${Math.round(1/b.ewTerms)} odds · ${b.ewPlaces} places` : ''}</div>` : '';

      return `
        <div class="bet-card ${cardClass}">
          <div class="bet-top">
            <div class="bet-name">${b.name}${sportTag(b)}</div>
            <span class="bet-result ${resultLabel}">${resultLabel}</span>
          </div>
          <div class="bet-meta">
            <span>📅 <strong>${b.date || '—'}</strong></span>
            <span>Odds <strong>${b.odds}</strong></span>
            <span>Stake <strong>£${(parseFloat(b.stake) * (b.isEW ? 2 : 1)).toFixed(2)}${b.isEW ? ' (EW)' : ''}</strong></span>
            ${b.bookie ? `<span>📍 <strong>${b.bookie}</strong></span>` : ''}
            ${b.market ? `<span>🎯 <strong>${b.market}</strong></span>` : ''}
            ${b.type === 'acca' && b.legs ? `<span>🔗 <strong>${b.legs.length} legs</strong></span>` : ''}
          </div>
          ${ewNote}${legs}
          ${plText ? `<div class="bet-pl ${plClass}">${plText}</div>` : ''}
          ${b.notes ? `<div class="bet-notes">📝 ${b.notes}</div>` : ''}
          <div class="bet-actions">
            ${b.result === 'pending' ? `<button class="action-btn settle" onclick="openSettle(${ri})">Settle</button>` : ''}
            <button class="action-btn delete" onclick="deleteBet(${ri})">Delete</button>
          </div>
        </div>`;
    }).join('');
  }

  function openSettle(i) {
    settleIndex = i;
    const bet = bets[i];
    document.getElementById('settle-bet-name').textContent = bet.name;

    const isEWBet = bet.isEW;
    document.getElementById('settle-btns').style.display = isEWBet ? 'none' : 'block';
    document.getElementById('settle-ew-btns').style.display = isEWBet ? 'block' : 'none';

    if (isEWBet) {
      const winRet = calcEWReturn(bet, 'win').toFixed(2);
      const placeRet = calcEWReturn(bet, 'ew-placed').toFixed(2);
      document.getElementById('settle-ew-note').style.display = 'block';
      document.getElementById('settle-ew-note').textContent =
        `Each-Way: Win return = £${winRet} | Place-only return = £${placeRet}. Adjust return amount if bookmaker differs.`;
      document.getElementById('settle-return').value = winRet;
    } else {
      document.getElementById('settle-ew-note').style.display = 'none';
      document.getElementById('settle-return').value = (bet.odds * bet.stake).toFixed(2);
    }

    document.getElementById('settle-modal').classList.add('open');
  }

  function closeModal() {
    document.getElementById('settle-modal').classList.remove('open');
    settleIndex = null;
  }

  function confirmSettle(result) {
    if (settleIndex === null) return;
    const ret = parseFloat(document.getElementById('settle-return').value) || 0;
    bets[settleIndex].result = result;
    bets[settleIndex].actualReturn = (result === 'win' || result === 'ew-placed') ? ret : 0;
    save(); updateHeader(); closeModal(); renderBets();
    showToast(`Settled as ${result === 'ew-placed' ? 'placed' : result} ✓`);
  }

  function deleteBet(i) {
    if (!confirm('Delete this bet?')) return;
    bets.splice(i, 1);
    save(); updateHeader(); renderBets();
    showToast('Deleted');
  }

  // ---- STATS ----
  function renderStats() {
    const settled = bets.filter(b => b.result !== 'pending' && b.result !== 'void');
    const wins = settled.filter(b => b.result === 'win' || b.result === 'ew-placed');
    const losses = settled.filter(b => b.result === 'loss');
    const pending = bets.filter(b => b.result === 'pending');
    const staked = settled.reduce((a, b) => a + parseFloat(b.stake) * (b.isEW ? 2 : 1), 0);
    const pl = settled.reduce((a, b) => a + calcPL(b), 0);
    const roi = staked > 0 ? (pl / staked) * 100 : null;

    const plEl = document.getElementById('s-pl');
    plEl.textContent = (pl >= 0 ? '+' : '') + '£' + pl.toFixed(2);
    plEl.className = 'big ' + (pl > 0 ? 'pos' : pl < 0 ? 'neg' : 'neutral');
    document.getElementById('s-pl-sub').textContent = `From ${settled.length} settled bets`;
    document.getElementById('s-wr').textContent = settled.length ? ((wins.length / settled.length) * 100).toFixed(0) + '%' : '—';
    document.getElementById('s-wr-sub').textContent = `${wins.length}W / ${losses.length}L`;

    const roiEl = document.getElementById('s-roi');
    roiEl.textContent = roi !== null ? (roi >= 0 ? '+' : '') + roi.toFixed(1) + '%' : '—';
    roiEl.className = 'big ' + (roi > 0 ? 'pos' : roi < 0 ? 'neg' : 'neutral');
    document.getElementById('s-avgodds').textContent = wins.length ? (wins.reduce((a, b) => a + b.odds, 0) / wins.length).toFixed(2) : '—';

    const bigWin = wins.length ? Math.max(...wins.map(b => calcPL(b))) : null;
    const bigBet = bigWin !== null ? wins.find(b => calcPL(b) === bigWin) : null;
    document.getElementById('s-bigwin').textContent = bigWin !== null ? '+£' + bigWin.toFixed(2) : '—';
    document.getElementById('s-bigwin-sub').textContent = bigBet ? bigBet.name.substring(0, 22) : '';
    document.getElementById('s-pending').textContent = pending.length;

    ['horse','nba','football','other'].forEach(sport => {
      const sp = settled.filter(b => b.sport === sport && b.type !== 'acca');
      const spPL = sp.reduce((a, b) => a + calcPL(b), 0);
      const el = document.getElementById('sb-' + sport);
      el.textContent = (spPL >= 0 ? '+' : '') + '£' + spPL.toFixed(2);
      el.className = 'sport-row-right ' + (spPL > 0 ? 'pos' : spPL < 0 ? 'neg' : 'neutral');
    });

    const accas = settled.filter(b => b.type === 'acca');
    const accaPL = accas.reduce((a, b) => a + calcPL(b), 0);
    const ae = document.getElementById('sb-acca');
    ae.textContent = (accaPL >= 0 ? '+' : '') + '£' + accaPL.toFixed(2);
    ae.className = 'sport-row-right ' + (accaPL > 0 ? 'pos' : accaPL < 0 ? 'neg' : 'neutral');
  }

  // ---- MONTHLY ----
  function renderMonthly() {
    const settled = bets.filter(b => b.result !== 'pending' && b.result !== 'void' && b.date);
    if (!settled.length) {
      document.getElementById('monthly-chart').innerHTML = '<div class="no-monthly">No settled bets yet — come back once you\'ve got some results in.</div>';
      ['m-best','m-worst','m-avg'].forEach(id => document.getElementById(id).textContent = '—');
      document.getElementById('m-profit-count').textContent = '—';
      return;
    }

    // Group by YYYY-MM
    const byMonth = {};
    settled.forEach(b => {
      const key = b.date.substring(0, 7); // "2025-03"
      if (!byMonth[key]) byMonth[key] = { pl: 0, count: 0, staked: 0 };
      const pl = calcPL(b);
      byMonth[key].pl += pl;
      byMonth[key].count++;
      byMonth[key].staked += parseFloat(b.stake) * (b.isEW ? 2 : 1);
    });

    const keys = Object.keys(byMonth).sort();
    const values = keys.map(k => byMonth[k].pl);
    const maxAbs = Math.max(...values.map(Math.abs), 0.01);

    // Chart
    document.getElementById('monthly-chart').innerHTML = keys.map(k => {
      const d = byMonth[k];
      const [yr, mo] = k.split('-');
      const label = MONTHS[parseInt(mo) - 1] + ' ' + yr.substring(2);
      const barPct = Math.min(100, (Math.abs(d.pl) / maxAbs) * 100);
      const cls = d.pl >= 0 ? 'pos' : 'neg';
      return `
        <div class="month-bar-row">
          <div class="month-bar-header">
            <span class="month-label">${label}</span>
            <span class="month-pl ${cls}">${d.pl >= 0 ? '+' : ''}£${d.pl.toFixed(2)}</span>
          </div>
          <div class="month-bar-track"><div class="month-bar-fill ${cls}" style="width:${barPct}%"></div></div>
          <div class="month-sub">${d.count} bets · £${d.staked.toFixed(2)} staked</div>
        </div>`;
    }).join('');

    // Summary cards
    const best = Math.max(...values);
    const worst = Math.min(...values);
    const bestKey = keys[values.indexOf(best)];
    const worstKey = keys[values.indexOf(worst)];
    const profitMonths = values.filter(v => v > 0).length;
    const avg = values.reduce((a, b) => a + b, 0) / values.length;

    const fmt = (k) => { const [yr, mo] = k.split('-'); return MONTHS[parseInt(mo)-1] + ' ' + yr.substring(2); };

    document.getElementById('m-best').textContent = '+£' + best.toFixed(2);
    document.getElementById('m-best-sub').textContent = fmt(bestKey);
    document.getElementById('m-worst').textContent = (worst >= 0 ? '+' : '-') + '£' + Math.abs(worst).toFixed(2);
    document.getElementById('m-worst-sub').textContent = fmt(worstKey);
    document.getElementById('m-profit-count').textContent = profitMonths;
    document.getElementById('m-profit-sub').textContent = `out of ${keys.length} months`;
    const avgEl = document.getElementById('m-avg');
    avgEl.textContent = (avg >= 0 ? '+' : '') + '£' + avg.toFixed(2);
    avgEl.className = 'big ' + (avg > 0 ? 'pos' : avg < 0 ? 'neg' : 'neutral');
  }

  function updateHeader() {
    const pl = bets.filter(b => b.result !== 'pending' && b.result !== 'void').reduce((a, b) => a + calcPL(b), 0);
    const pending = bets.filter(b => b.result === 'pending').length;
    document.getElementById('bankroll-display').textContent = `P&L: ${pl >= 0 ? '+' : ''}£${pl.toFixed(2)} · ${pending} pending`;
    updateStatBar();
  }

  function updateStatBar() {
    const settled = bets.filter(b => b.result !== 'pending' && b.result !== 'void');
    const wins = settled.filter(b => b.result === 'win' || b.result === 'ew-placed');
    const pl = settled.reduce((a, b) => a + calcPL(b), 0);
    const staked = settled.reduce((a, b) => a + parseFloat(b.stake) * (b.isEW ? 2 : 1), 0);
    const roi = staked > 0 ? (pl / staked) * 100 : null;
    const wr = settled.length ? ((wins.length / settled.length) * 100).toFixed(0) + '%' : '—';
    document.getElementById('stat-total').textContent = bets.length;
    const wrEl = document.getElementById('stat-wr'); wrEl.textContent = wr; wrEl.className = 'stat-val ' + (wins.length ? 'pos' : 'neutral');
    const plEl = document.getElementById('stat-pl'); plEl.textContent = (pl >= 0 ? '+' : '') + '£' + pl.toFixed(2); plEl.className = 'stat-val ' + (pl > 0 ? 'pos' : pl < 0 ? 'neg' : 'neutral');
    const roiEl = document.getElementById('stat-roi'); roiEl.textContent = roi !== null ? (roi >= 0 ? '+' : '') + roi.toFixed(0) + '%' : '—'; roiEl.className = 'stat-val ' + (roi > 0 ? 'pos' : roi < 0 ? 'neg' : 'neutral');
  }

  function exportCSV() {
    if (!bets.length) { showToast('No bets to export'); return; }
    const headers = ['Date','Type','Sport','Bet','EachWay','EWTerms','EWPlaces','Legs','Market','Odds','Stake(each)','TotalStake','Result','P&L','Return','Bookmaker','Notes'];
    const rows = bets.map(b => {
      const pl = calcPL(b);
      const legs = b.legs ? b.legs.map(l => `${l.name} @ ${l.odds}${l.voided?' (void)':''}`).join(' | ') : '';
      const totalStake = (parseFloat(b.stake) * (b.isEW ? 2 : 1)).toFixed(2);
      return [b.date, b.type||'single', b.sport, b.name, b.isEW?'Yes':'No', b.ewTerms||'', b.ewPlaces||'',
        legs, b.market||'', b.odds, b.stake, totalStake, b.result,
        pl !== null ? pl.toFixed(2) : '', b.actualReturn||'', b.bookie||'', b.notes||'']
        .map(v => `"${(v||'').toString().replace(/"/g,'""')}"`).join(',');
    });
    const csv = [headers.join(','), ...rows].join('\n');
    const a = document.createElement('a');
    a.href = URL.createObjectURL(new Blob([csv], {type:'text/csv'}));
    a.download = 'bets_' + new Date().toISOString().split('T')[0] + '.csv';
    a.click();
    showToast('CSV exported ✓');
  }

  function save() { localStorage.setItem('bets', JSON.stringify(bets)); }
  updateHeader();
</script>
</body>
</html>
