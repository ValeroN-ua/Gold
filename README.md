<html lang="uk">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>GOLD — Панель управління</title>
<style>
  :root {
    --bg: #0a0a0b;
    --panel: #111112;
    --panel-2: #18181a;
    --line: #1f1f22;
    --text: #f1f1f2;
    --muted: #6d6d73;
    --red: #e9453b;
    --green: #4cc35a;
    --gold: #e8b84a;
    --crowns: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='64' height='64' viewBox='0 0 64 64'%3E%3Cg fill='%23e9453b' fill-opacity='.10'%3E%3Cpath d='M8 20l5 4 5-8 5 8 5-4-2 12H10zM9 34h18v2H9z'/%3E%3Cpath d='M40 52l5 4 5-8 5 8 5-4-2 12H42zM41 66h18v2H41z' transform='translate(0 -18)'/%3E%3C/g%3E%3C/svg%3E");
    color-scheme: dark;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html { font-size: 15px; }
  body {
    background:
      radial-gradient(900px 500px at 8% -5%, rgba(233,69,59,.16), transparent 65%),
      radial-gradient(700px 420px at 100% 100%, rgba(233,69,59,.07), transparent 70%),
      var(--crowns),
      var(--bg);
    background-attachment: fixed;
    color: var(--text);
    font-family: "Inter", "Segoe UI", system-ui, -apple-system, sans-serif;
    min-height: 100vh;
    padding: clamp(14px, 2vw, 28px);
  }
  .app {
    max-width: 1400px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1fr;
    gap: clamp(14px, 1.6vw, 24px);
    align-items: start;
  }
  
  .sidebar {
    background: var(--panel);
    border: 1px solid var(--line);
    border-radius: 18px;
    overflow: hidden;
  }
  .org-head {
    padding: 20px;
    background:
      var(--crowns),
      linear-gradient(135deg, rgba(233,69,59,.34), rgba(233,69,59,.05) 75%),
      #1a0e0e;
    display: flex; gap: 14px; align-items: center;
  }
  .logo {
    width: 60px; height: 60px; flex: none;
    border-radius: 50%;
    display: grid; place-items: center;
    background: radial-gradient(circle at 35% 30%, #3a2f12, #0d0b05 70%);
    border: 2px solid var(--gold);
    color: var(--gold);
    font-weight: 800; font-size: 14px; letter-spacing: .5px;
    box-shadow: 0 0 0 3px rgba(232,184,74,.15), 0 6px 20px rgba(0,0,0,.5);
  }
  .org-name { font-size: 18px; font-weight: 700; }
  .menu-label { padding: 18px 24px 8px; font-size: 12px; color: var(--muted); letter-spacing: .06em; }
  nav { padding-bottom: 12px; }
  nav button {
    width: 100%;
    display: flex; align-items: center; gap: 16px;
    padding: 15px 24px;
    background: none; border: 0;
    border-left: 3px solid transparent;
    color: #a2a2a8;
    font: inherit; font-size: 16px; text-align: left;
    cursor: pointer;
    transition: background .15s, color .15s;
  }
  nav button svg { width: 20px; height: 20px; fill: currentColor; flex: none; }
  nav button:hover { color: var(--text); background: rgba(255,255,255,.025); }
  nav button.active {
    color: var(--text);
    border-left-color: var(--red);
    background: linear-gradient(90deg, rgba(233,69,59,.14), transparent 85%);
  }
  nav button.active svg { color: var(--red); }
  button:focus-visible, input:focus-visible { outline: 2px solid var(--red); outline-offset: 2px; }

  main { min-width: 0; }
  .empty { color: var(--muted); padding: 40px 8px; font-size: 16px; }
  #calc[hidden], #stats[hidden], #report[hidden], #truck[hidden], .empty[hidden] { display: none; }

  h1 { font-size: 24px; font-weight: 700; }
  .sub { color: var(--muted); margin: 4px 0 20px; }

  .calc {
    background:
      linear-gradient(180deg, rgba(233,69,59,.10), transparent 140px),
      var(--panel);
    border: 1px solid var(--line);
    border-radius: 14px;
    max-width: 760px;
  }
  .field { padding: 20px 24px; border-bottom: 1px solid var(--line); }
  .field label, .field .label { display: block; color: var(--muted); margin-bottom: 10px; }

  .money { position: relative; }
  .money span {
    position: absolute; left: 16px; top: 50%; transform: translateY(-50%);
    color: var(--muted); font-weight: 700; pointer-events: none;
  }
  input[type="number"] {
    width: 100%;
    padding: 15px 16px 15px 34px;
    background: var(--panel-2);
    border: 1px solid #2a2a2e;
    border-radius: 10px;
    color: var(--text);
    font: inherit; font-size: 17px; font-weight: 700;
    -moz-appearance: textfield;
  }
  input[type="number"]::-webkit-outer-spin-button,
  input[type="number"]::-webkit-inner-spin-button { -webkit-appearance: none; }
  input[type="number"]:focus { border-color: var(--red); outline: none; }

  /* dropdown */
  .dd { position: relative; }
  .dd-btn {
    width: 100%;
    display: flex; justify-content: space-between; align-items: center; gap: 12px;
    padding: 15px 16px;
    background: var(--panel-2);
    border: 1px solid #2a2a2e;
    border-radius: 10px;
    color: var(--text);
    font: inherit; font-size: 16px; text-align: left;
    cursor: pointer;
  }
  .dd-btn.placeholder { color: var(--muted); }
  .dd-btn svg { width: 16px; height: 16px; fill: none; stroke: currentColor; stroke-width: 2; flex: none; transition: transform .15s; }
  .dd.open .dd-btn { border-color: var(--red); }
  .dd.open .dd-btn svg { transform: rotate(180deg); }
  .dd-list {
    position: absolute; left: 0; right: 0; top: calc(100% + 6px); z-index: 5;
    background: #161618;
    border: 1px solid #2a2a2e;
    border-radius: 10px;
    padding: 6px;
    box-shadow: 0 14px 30px rgba(0,0,0,.55);
  }
  .dd-list { max-height: 320px; overflow-y: auto; }
  .dd-list[hidden] { display: none; }
  .opt-name { min-width: 0; overflow-wrap: anywhere; }
  .dd-btn span#ddText { min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .opt {
    display: flex; align-items: center; gap: 12px;
    padding: 12px 12px;
    border-radius: 8px;
    cursor: pointer;
    user-select: none;
  }
  .opt:hover { background: #222225; }
  .opt input { position: absolute; opacity: 0; pointer-events: none; }
  .box {
    width: 20px; height: 20px; flex: none;
    border: 1.5px solid #46464b;
    border-radius: 5px;
    display: grid; place-items: center;
  }
  .box svg { width: 13px; height: 13px; fill: none; stroke: #fff; stroke-width: 3; opacity: 0; }
  .opt input:checked + .box { background: var(--red); border-color: var(--red); }
  .opt input:checked + .box svg { opacity: 1; }
  .opt input:focus-visible + .box { outline: 2px solid var(--red); outline-offset: 2px; }

  /* result */
  .result { padding: 20px 24px 8px; }
  .result .label { color: var(--muted); margin-bottom: 6px; }
  .rows { list-style: none; }
  .rows li {
    display: flex; justify-content: space-between; align-items: center;
    padding: 14px 0;
    border-bottom: 1px solid var(--line);
  }
  .rows li:last-child { border-bottom: 0; }
  .rows .name { font-weight: 600; }
  .rows .sum { font-weight: 800; font-size: 18px; color: var(--green); }
  .hint { color: var(--muted); padding: 10px 0 18px; }

  .actions { padding: 8px 24px 24px; }
  .primary {
    width: 100%;
    padding: 16px;
    background: var(--red);
    border: 0;
    border-radius: 10px;
    color: #fff; font: inherit; font-size: 16px; font-weight: 700;
    cursor: pointer;
    box-shadow: 0 6px 18px rgba(233,69,59,.3);
    transition: filter .15s, opacity .15s;
  }
  .primary:hover:not(:disabled) { filter: brightness(1.1); }
  .primary:disabled { opacity: .35; cursor: not-allowed; box-shadow: none; }

  .stats-block { max-width: 760px; }
  .stats-head {
    display: flex; justify-content: space-between; align-items: center;
    gap: 16px; margin-bottom: 16px;
  }
  .stats-head h1 { font-size: 24px; font-weight: 700; }
  .ghost {
    display: inline-flex; align-items: center; gap: 8px;
    padding: 11px 18px;
    background: var(--panel-2);
    border: 1px solid #2a2a2e;
    border-radius: 10px;
    color: var(--text); font: inherit; font-weight: 600;
    cursor: pointer;
  }
  .ghost:hover { background: #222225; }
  .ghost svg { width: 16px; height: 16px; fill: none; stroke: currentColor; stroke-width: 2; }

  table.stats {
    width: 100%;
    border-collapse: separate; border-spacing: 0;
    background:
      linear-gradient(180deg, rgba(233,69,59,.10), transparent 120px),
      var(--panel);
    border: 1px solid var(--line);
    border-radius: 14px;
    overflow: hidden;
  }
  .stats th {
    padding: 14px 24px;
    text-align: left;
    font-weight: 500; font-size: 14px;
    color: var(--muted);
    border-bottom: 1px solid var(--line);
  }
  .stats td { padding: 16px 24px; border-bottom: 1px solid var(--line); transition: background .2s, color .2s; }
  .stats tr:last-child td { border-bottom: 0; }
  .stats th:nth-child(2), .stats td:nth-child(2) { text-align: right; }
  .stats th:nth-child(3), .stats td:nth-child(3) { text-align: center; width: 120px; }
  .stats td:first-child { font-weight: 600; overflow-wrap: anywhere; }
  .stats td:nth-child(2) { font-weight: 800; font-variant-numeric: tabular-nums; }
  .stats tr.paid td { background: rgba(76,195,90,.16); color: var(--green); }
  .stats tr.paid td { box-shadow: inset 0 1px 0 rgba(76,195,90,.25), inset 0 -1px 0 rgba(76,195,90,.25); }

  .pay { position: relative; display: inline-block; cursor: pointer; }
  .pay input { position: absolute; opacity: 0; width: 100%; height: 100%; margin: 0; cursor: pointer; }
  .pay .box { width: 24px; height: 24px; border-radius: 6px; }
  .pay input:checked + .box { background: var(--green); border-color: var(--green); }
  .pay input:focus-visible + .box { outline: 2px solid var(--red); outline-offset: 2px; }

  .head-actions { display: flex; gap: 10px; flex-wrap: wrap; }
  .ghost.save { background: rgba(233,69,59,.14); border-color: rgba(233,69,59,.45); }
  .ghost.save:hover:not(:disabled) { background: rgba(233,69,59,.24); }
  .ghost:disabled { opacity: .4; cursor: not-allowed; }
  .stats-block.wide { max-width: 760px; }
  .report th:nth-child(n+2), .report td:nth-child(n+2) { text-align: right; width: auto; }
  .report td:nth-child(n+2) { font-weight: 700; font-variant-numeric: tabular-nums; }
  .report td.zero { color: #4a4a50; font-weight: 500; }
  .report td:nth-child(5) { color: var(--green); }
  .report td.zero:nth-child(5) { color: #4a4a50; }
  .report th:last-child, .report td:last-child { text-align: right; width: 120px; }
  .sr { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); }
  .click-btn {
    padding: 8px 18px;
    background: var(--red);
    border: 0; border-radius: 8px;
    color: #fff; font: inherit; font-weight: 700; font-size: 14px;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(233,69,59,.28);
    transition: transform .08s, filter .15s;
  }
  .click-btn:hover { filter: brightness(1.12); }
  .click-btn:active { transform: scale(.93); }

  /* кнопка з кількістю контрактів */
  .count-btn { display: inline-flex; align-items: baseline; justify-content: center; gap: 8px; min-width: 132px; }
  .count-btn b { font-size: 18px; font-weight: 800; font-variant-numeric: tabular-nums; line-height: 1; }
  .count-btn span { font-size: 12px; font-weight: 600; opacity: .85; }
  .count-btn.zero { background: #26262a; color: #8a8a90; box-shadow: none; border: 1px solid #323237; }
  .report th:last-child, .report td:last-child { width: 210px; white-space: nowrap; }
  .week-range { margin: -6px 0 14px; }
  .ghost.armed { background: rgba(233,69,59,.25); border-color: var(--red); color: #fff; }

  .modal {
    position: fixed; inset: 0; z-index: 50;
    display: grid; place-items: center;
    padding: 16px;
    background: rgba(5,5,6,.7);
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
    animation: fade .2s ease;
  }
  .modal[hidden] { display: none; }
  .modal-card {
    position: relative;
    width: min(600px, 100%);
    max-height: 92vh; overflow-y: auto;
    background:
      var(--crowns),
      radial-gradient(520px 260px at 0% 0%, rgba(233,69,59,.30), transparent 70%),
      linear-gradient(180deg, #151517, #0f0f10);
    border: 1px solid #2a2a2e;
    border-radius: 22px;
    padding: 26px;
    box-shadow: 0 30px 80px rgba(0,0,0,.7), 0 0 0 1px rgba(233,69,59,.12);
    animation: pop .28s cubic-bezier(.2,.9,.3,1.2);
  }
  .modal-close {
    position: absolute; top: 16px; right: 16px;
    width: 38px; height: 38px;
    display: grid; place-items: center;
    background: rgba(255,255,255,.05);
    border: 1px solid #2a2a2e; border-radius: 50%;
    color: #b5b5ba; cursor: pointer;
  }
  .modal-close:hover { background: rgba(233,69,59,.2); color: #fff; }
  .modal-close svg { width: 16px; height: 16px; fill: none; stroke: currentColor; stroke-width: 2.2; stroke-linecap: round; }

  .m-head { display: flex; align-items: center; gap: 16px; padding-right: 48px; margin-bottom: 22px; }
  .m-avatar {
    width: 56px; height: 56px; flex: none;
    display: grid; place-items: center;
    border-radius: 16px;
    background: linear-gradient(145deg, var(--red), #8f2019);
    font-size: 24px; font-weight: 800; text-transform: uppercase;
    box-shadow: 0 8px 22px rgba(233,69,59,.4);
  }
  .m-title { min-width: 0; }
  .m-sub { color: var(--muted); font-size: 13px; letter-spacing: .05em; text-transform: uppercase; }
  .m-title h3 { font-size: 20px; font-weight: 700; overflow-wrap: anywhere; margin-top: 2px; }

  .m-body { display: flex; align-items: center; gap: 28px; flex-wrap: wrap; justify-content: center; }
  .donut { position: relative; width: 220px; height: 220px; flex: none; }
  .donut svg { width: 100%; height: 100%; display: block; filter: drop-shadow(0 0 14px rgba(233,69,59,.18)); }
  .donut circle.track { fill: none; stroke: #202023; }
  .donut circle.val {
    fill: none; stroke-linecap: round;
    transform: rotate(-90deg); transform-origin: 110px 110px;
    transition: stroke-dashoffset 1.1s cubic-bezier(.2,.8,.2,1);
  }
  .donut-center {
    position: absolute; inset: 0;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    text-align: center; pointer-events: none;
  }
  .donut-center b { font-size: 38px; font-weight: 800; line-height: 1; font-variant-numeric: tabular-nums; }
  .donut-center span { color: var(--muted); font-size: 12px; margin-top: 6px; letter-spacing: .04em; text-transform: uppercase; }

  .legend { list-style: none; flex: 1; min-width: 200px; display: grid; gap: 10px; }
  .legend li {
    display: grid; grid-template-columns: 12px 1fr auto; align-items: center; gap: 12px;
    padding: 12px 14px;
    background: rgba(255,255,255,.035);
    border: 1px solid #222226;
    border-radius: 12px;
    opacity: 0; transform: translateX(14px);
    animation: slide .45s ease forwards;
  }
  .legend .dot { width: 12px; height: 12px; border-radius: 50%; box-shadow: 0 0 12px currentColor; }
  .legend .lbl { color: #b9b9be; }
  .legend .num { font-weight: 800; font-size: 18px; text-align: right; font-variant-numeric: tabular-nums; line-height: 1.1; }
  .legend .pct { display: block; color: var(--muted); font-weight: 500; font-size: 12px; }

  .m-earn {
    margin-top: 22px;
    display: flex; justify-content: space-between; align-items: center; gap: 16px; flex-wrap: wrap;
    padding: 18px 20px;
    border-radius: 16px;
    background: linear-gradient(135deg, rgba(76,195,90,.18), rgba(76,195,90,.04));
    border: 1px solid rgba(76,195,90,.3);
  }
  .m-earn-label { color: #8fbf96; font-size: 13px; }
  .m-earn-sum { color: var(--green); font-size: 30px; font-weight: 800; margin-top: 2px; font-variant-numeric: tabular-nums; }
  .m-earn-share {
    padding: 8px 14px; border-radius: 999px;
    background: rgba(0,0,0,.35); border: 1px solid rgba(76,195,90,.3);
    color: #b8e6bd; font-size: 14px; font-weight: 600;
  }
  .m-earn-share:empty { display: none; }

  @keyframes fade { from { opacity: 0; } }
  @keyframes pop { from { opacity: 0; transform: translateY(14px) scale(.95); } }
  @keyframes slide { to { opacity: 1; transform: none; } }
  @media (prefers-reduced-motion: reduce) {
    .modal, .modal-card, .legend li { animation: none; opacity: 1; transform: none; }
    .donut circle.val { transition: none; }
  }
  @media (max-width: 520px) { .modal-card { padding: 20px; } .donut { width: 190px; height: 190px; } }

  @media (min-width: 1100px) {
    .calc { max-width: 980px; display: grid; grid-template-columns: 1fr 1fr; }
    .calc .field { grid-column: 1; }
    .calc .result { grid-column: 2; grid-row: 1 / span 2; border-left: 1px solid var(--line); padding-top: 22px; }
    .calc .actions { grid-column: 1 / -1; border-top: 1px solid var(--line); padding-top: 20px; }
    .stats-block, .stats-block.wide { max-width: 1100px; }
    .calc { max-width: 1100px; }
  }
  /* великий монітор */
  @media (min-width: 1600px) {
    html { font-size: 16px; }
    .calc, .stats-block, .stats-block.wide { max-width: 1400px; }
    .empty { font-size: 18px; }
  }
  /* невисокий ноутбук */
  @media (min-width: 1100px) and (max-height: 800px) {
    html { font-size: 14px; }
    nav button { padding: 12px 24px; }
  }
  /* планшет і телефон */
  @media (max-width: 900px) {
    .calc, .stats-block, .stats-block.wide { max-width: none; }
  }
  @media all {
    nav { display: flex; overflow-x: auto; padding: 0 12px 12px; gap: 8px; scrollbar-width: none; }
    nav::-webkit-scrollbar { display: none; }
    nav button {
      width: auto; flex: none; white-space: nowrap;
      padding: 12px 16px; gap: 10px;
      border: 1px solid var(--line); border-radius: 10px;
    }
    nav button.active { border-color: rgba(233,69,59,.5); }
    .menu-label { padding: 14px 20px 8px; }
  }
  @media (max-width: 520px) {
    body { padding: 12px; }
    .field, .result { padding-left: 18px; padding-right: 18px; }
    .actions { padding-left: 18px; padding-right: 18px; }
    .stats th, .stats td { padding-left: 14px; padding-right: 14px; }
    .stats-head { flex-wrap: wrap; }
    .head-actions { width: 100%; }
    .head-actions .ghost { flex: 1; justify-content: center; }
  }

  .app { position: relative; z-index: 1; }

  /* плаваючі корони на фоні */
  .crowns-bg { position: fixed; inset: 0; z-index: 0; overflow: hidden; pointer-events: none; }
  .crowns-bg span {
    position: absolute; bottom: -70px; left: var(--x);
    width: var(--s); height: var(--s);
    background: var(--c) center / contain no-repeat;
    opacity: 0;
    animation: floatUp var(--d) linear var(--dl) infinite;
  }
  @keyframes floatUp {
    0%   { transform: translate(0, 0) rotate(-10deg); opacity: 0; }
    10%  { opacity: var(--o); }
    50%  { transform: translate(var(--sway), -55vh) rotate(8deg); }
    90%  { opacity: var(--o); }
    100% { transform: translate(calc(var(--sway) * -.6), -118vh) rotate(-8deg); opacity: 0; }
  }

  .logo { position: relative; animation: logoGlow 3.4s ease-in-out infinite; }
  .logo::before {
    content: ""; position: absolute; inset: -7px; border-radius: 50%; pointer-events: none;
    background: conic-gradient(from 0deg, transparent 0 55%, rgba(232,184,74,.95) 85%, transparent 100%);
    -webkit-mask: radial-gradient(farthest-side, transparent calc(100% - 3px), #000 calc(100% - 2px));
            mask: radial-gradient(farthest-side, transparent calc(100% - 3px), #000 calc(100% - 2px));
    animation: spin 3.2s linear infinite;
    filter: drop-shadow(0 0 6px rgba(232,184,74,.8));
  }
  @keyframes logoGlow {
    0%, 100% { box-shadow: 0 0 0 3px rgba(232,184,74,.15), 0 6px 20px rgba(0,0,0,.5), 0 0 12px rgba(232,184,74,.15); }
    50%      { box-shadow: 0 0 0 3px rgba(232,184,74,.3),  0 6px 20px rgba(0,0,0,.5), 0 0 30px rgba(232,184,74,.55); }
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  .org-head { position: relative; overflow: hidden; }
  .org-head::after {
    content: ""; position: absolute; top: 0; bottom: 0; left: -40%; width: 30%; pointer-events: none;
    background: linear-gradient(100deg, transparent, rgba(255,214,120,.16), transparent);
    transform: skewX(-20deg);
    animation: headShine 6.5s ease-in-out infinite;
  }
  @keyframes headShine { 0%, 55% { left: -40%; } 100% { left: 140%; } }

  nav button.active svg { animation: iconPulse 2.2s ease-in-out infinite; }
  @keyframes iconPulse { 50% { filter: drop-shadow(0 0 6px rgba(233,69,59,.9)); transform: scale(1.12); } }

  .enter .calc, .enter .stats-block > .stats-head, .enter table.stats { animation: rise .55s cubic-bezier(.2,.8,.2,1) both; }
  .enter table.stats { animation-delay: .08s; }
  .enter .calc .field { animation: rise .5s cubic-bezier(.2,.8,.2,1) both; }
  .enter .calc .field:nth-child(1) { animation-delay: .1s; }
  .enter .calc .field:nth-child(2) { animation-delay: .2s; }
  .enter .calc .result { animation: rise .5s .3s cubic-bezier(.2,.8,.2,1) both; }
  .enter .calc .actions { animation: rise .5s .4s cubic-bezier(.2,.8,.2,1) both; }
  .enter .stats tbody tr { animation: rowIn .45s cubic-bezier(.2,.8,.2,1) both; animation-delay: calc(var(--i, 0) * 40ms + .15s); }
  @keyframes rise  { from { opacity: 0; transform: translateY(18px); } }
  @keyframes rowIn { from { opacity: 0; transform: translateX(-16px); } }

  h1::after {
    content: ""; display: block; height: 2px; margin-top: 8px; width: 56px;
    background: linear-gradient(90deg, var(--red), var(--gold)); border-radius: 2px;
    box-shadow: 0 0 10px rgba(232,184,74,.6);
  }
  .enter h1::after { animation: grow .8s .15s cubic-bezier(.2,.8,.2,1) both; }
  @keyframes grow { from { width: 0; } }

  .stats tr.flash td { animation: flashRow 1.4s ease-out; }
  @keyframes flashRow { 0% { background: rgba(232,184,74,.45); } 100% { background: transparent; } }
  .stats tr.paid.flash td { animation: none; }

  .primary, .click-btn { position: relative; overflow: hidden; }
  .primary::after, .click-btn::after {
    content: ""; position: absolute; top: 0; bottom: 0; left: -60%; width: 40%; pointer-events: none;
    background: linear-gradient(100deg, transparent, rgba(255,255,255,.4), transparent);
    transform: skewX(-20deg);
  }
  .primary:hover:not(:disabled)::after, .click-btn:hover::after { left: 130%; transition: left .7s ease; }
  .primary:not(:disabled) { animation: btnPulse 2.6s ease-in-out infinite; }
  @keyframes btnPulse { 0%, 100% { box-shadow: 0 6px 18px rgba(233,69,59,.3); } 50% { box-shadow: 0 6px 30px rgba(233,69,59,.65), 0 0 0 4px rgba(233,69,59,.12); } }

  .spark { position: fixed; z-index: 100; pointer-events: none; left: 0; top: 0; will-change: transform, opacity; }
  .spark.coin {
    width: 13px; height: 13px; border-radius: 50%;
    background: radial-gradient(circle at 35% 30%, #fff3b0, #e8b84a 55%, #9a6f12);
    box-shadow: 0 0 8px rgba(232,184,74,.9);
  }
  .spark.crown { width: 20px; height: 20px; background: var(--c) center / contain no-repeat; }
  .shock {
    position: fixed; z-index: 99; pointer-events: none; width: 20px; height: 20px; border-radius: 50%;
    border: 2px solid rgba(232,184,74,.9); box-shadow: 0 0 16px rgba(232,184,74,.7);
  }

  .donut::before {
    content: ""; position: absolute; inset: -12px; border-radius: 50%; pointer-events: none;
    border: 1px dashed rgba(232,184,74,.45); animation: spin 30s linear infinite;
  }
  .m-avatar { animation: logoGlow 3.4s ease-in-out infinite; }

  @media (prefers-reduced-motion: reduce) {
    .crowns-bg { display: none; }
    .logo, .logo::before, .org-head::after, nav button.active svg, .primary:not(:disabled), .donut::before, .m-avatar,
    .enter .calc, .enter .calc .field, .enter .calc .result, .enter .calc .actions, .enter table.stats,
    .enter .stats-block > .stats-head, .enter .stats tbody tr, .enter h1::after, .stats tr.flash td { animation: none; }
  }

  .truck-panel {
    position: relative; overflow: hidden;
    max-width: 980px; min-height: 460px;
    border-radius: 16px;
    border: 1px solid #3b3322;
    background:
      radial-gradient(520px 220px at 50% 108%, rgba(255,180,0,.16), transparent 70%),
      linear-gradient(180deg, #05070b 0%, #0d121a 55%, #161a22 100%);
    box-shadow: 0 20px 50px rgba(0,0,0,.5);
  }
  .hazard {
    position: relative; z-index: 3; height: 16px;
    background: repeating-linear-gradient(-45deg, #ffb400 0 16px, #111 16px 32px);
    background-size: 45.25px 100%;
    animation: hazardMove 1.6s linear infinite;
  }
  @keyframes hazardMove { to { background-position-x: 45.25px; } }

  .stars {
    position: absolute; inset: 0; pointer-events: none;
    background:
      radial-gradient(1.5px 1.5px at 12% 18%, #fff, transparent),
      radial-gradient(1px 1px at 27% 34%, #cfd8ff, transparent),
      radial-gradient(1.5px 1.5px at 44% 12%, #fff, transparent),
      radial-gradient(1px 1px at 61% 26%, #ffe9b0, transparent),
      radial-gradient(1.5px 1.5px at 78% 16%, #fff, transparent),
      radial-gradient(1px 1px at 90% 38%, #cfd8ff, transparent),
      radial-gradient(1px 1px at 6% 46%, #fff, transparent),
      radial-gradient(1px 1px at 70% 48%, #ffe9b0, transparent);
    animation: twinkle 4s ease-in-out infinite;
  }
  @keyframes twinkle { 50% { opacity: .45; } }

  .truck-center {
    position: absolute; z-index: 2; left: 0; right: 0; top: 36%;
    transform: translateY(-50%);
    text-align: center; padding: 0 16px;
  }
  .road-sign {
    display: inline-block; margin-bottom: 18px;
    padding: 7px 20px;
    background: #0c6b3b; color: #fff;
    border: 2px solid #fff; border-radius: 8px;
    box-shadow: 0 0 0 3px #0c6b3b, 0 8px 20px rgba(0,0,0,.5);
    font-weight: 800; font-size: 14px; letter-spacing: .22em; text-transform: uppercase;
  }
  .soon { display: flex; align-items: center; justify-content: center; gap: clamp(12px, 3vw, 32px); }
  .soon h2 {
    font-size: clamp(34px, 8vw, 92px);
    font-weight: 900; line-height: 1;
    letter-spacing: .1em; text-transform: uppercase;
    color: #ffb400;
    text-shadow: 0 4px 0 #7a5200, 0 0 30px rgba(255,180,0,.45);
    animation: signGlow 3s ease-in-out infinite;
  }
  @keyframes signGlow { 50% { text-shadow: 0 4px 0 #7a5200, 0 0 52px rgba(255,180,0,.8); } }
  .beacon {
    flex: none; width: clamp(12px, 2vw, 20px); height: clamp(12px, 2vw, 20px); border-radius: 50%;
    background: #ff9d00; box-shadow: 0 0 18px 4px rgba(255,157,0,.8);
    animation: beacon 1s steps(1) infinite;
  }
  .beacon.b2 { animation-delay: .5s; }
  @keyframes beacon { 0% { opacity: 1; } 50% { opacity: .15; box-shadow: none; } }
  .truck-center p { margin-top: 18px; color: #9aa3b0; letter-spacing: .08em; }

  .horizon {
    position: absolute; left: 0; right: 0; bottom: 124px; height: 34px; z-index: 1; pointer-events: none;
    background:
      radial-gradient(circle 2px at 14px 18px, rgba(255,190,60,.85), transparent),
      radial-gradient(circle 1.5px at 44px 8px, rgba(255,255,255,.7), transparent),
      radial-gradient(circle 2px at 70px 24px, rgba(255,120,60,.8), transparent);
    background-size: 96px 34px; background-repeat: repeat-x;
    animation: farLights 8s linear infinite;
  }
  @keyframes farLights { to { background-position-x: -96px; } }

  .road {
    position: absolute; left: 0; right: 0; bottom: 16px; height: 124px; z-index: 2;
    background: linear-gradient(180deg, #1b1d22, #101114);
    border-top: 3px solid #2a2c33;
  }
  .truck-svg {
    position: absolute; left: 50%; bottom: 30px;
    width: min(380px, 78%); height: auto; overflow: visible;
    transform: translateX(-50%);
    animation: bob .7s ease-in-out infinite;
    filter: drop-shadow(0 6px 10px rgba(0,0,0,.6));
  }
  @keyframes bob { 50% { transform: translateX(-50%) translateY(-1.5px); } }
  .wheel { transform-box: fill-box; transform-origin: center; animation: spin .55s linear infinite; }
  .lane {
    position: absolute; left: 0; right: 0; bottom: 10px; height: 5px;
    background: repeating-linear-gradient(90deg, #ffb400 0 44px, transparent 44px 96px);
    animation: laneMove .45s linear infinite;
    box-shadow: 0 0 10px rgba(255,180,0,.35);
  }
  @keyframes laneMove { to { background-position-x: -96px; } }
  @keyframes spin { to { transform: rotate(360deg); } }

  #truck.enter .truck-panel { animation: rise .55s cubic-bezier(.2,.8,.2,1) both; }

  @media (max-width: 520px) {
    .truck-panel { min-height: 420px; }
    .road-sign { font-size: 12px; letter-spacing: .16em; }
  }
  @media (prefers-reduced-motion: reduce) {
    .hazard, .stars, .soon h2, .beacon, .horizon, .truck-svg, .wheel, .lane, #truck.enter .truck-panel { animation: none; }
  }

  #truck {
    position: fixed; inset: 0; z-index: 60;
    background: #05070b;
    animation: pageIn .45s cubic-bezier(.2,.8,.2,1);
  }
  #truck[hidden] { display: none; }
  #truck .truck-panel {
    position: absolute; inset: 0;
    max-width: none; min-height: 0;
    border: 0; border-radius: 0; box-shadow: none;
    animation: none;
  }
  #truck .truck-svg { width: clamp(320px, 54vw, 860px); }
  .truck-back {
    position: absolute; z-index: 6; top: 32px; left: 24px;
    display: inline-flex; align-items: center; gap: 8px;
    padding: 11px 20px 11px 14px;
    background: rgba(5,7,11,.75);
    border: 2px solid #ffb400; border-radius: 10px;
    color: #ffb400; font: inherit; font-weight: 800; letter-spacing: .12em; text-transform: uppercase;
    cursor: pointer;
    backdrop-filter: blur(4px);
    transition: background .15s, color .15s, transform .1s;
  }
  .truck-back svg { width: 18px; height: 18px; fill: none; stroke: currentColor; stroke-width: 3; stroke-linecap: round; stroke-linejoin: round; }
  .truck-back:hover { background: #ffb400; color: #111; }
  .truck-back:active { transform: scale(.96); }
  @keyframes pageIn { from { opacity: 0; transform: scale(1.03); } }
  @media (max-width: 520px) { .truck-back { top: 28px; left: 14px; padding: 9px 14px 9px 10px; } }
  @media (prefers-reduced-motion: reduce) { #truck { animation: none; } }

  .la { position: absolute; inset: 0; z-index: 0; overflow: hidden; pointer-events: none; }
  .la-sky {
    position: absolute; inset: 0;
    background:
      radial-gradient(900px 340px at 74% 84%, rgba(255,140,60,.55), transparent 70%),
      radial-gradient(700px 300px at 18% 90%, rgba(190,60,110,.35), transparent 70%),
      linear-gradient(180deg, #05070b 0%, #0b0c1e 36%, #261433 60%, #5f2340 78%, #bd5130 92%, #ee9843 100%);
  }
  .stars { z-index: 1; }
  .truck-panel > .hazard:last-child { position: absolute; left: 0; right: 0; bottom: 0; }
  .layer {
    position: absolute; left: 0; bottom: 140px;
    height: var(--h); width: calc(100% + var(--tile));
    animation: scrollX var(--t) linear infinite;
    will-change: transform;
  }
  .layer.rev { animation-direction: reverse; }
  .layer > i {
    position: absolute; inset: 0;
    background-repeat: repeat-x; background-size: var(--tile) var(--h); background-position: 0 100%;
  }
  .layer > .wa { animation: winGlow 5s ease-in-out infinite; }
  .layer > .wb { animation: winGlow 7s ease-in-out -3s infinite; }
  @keyframes scrollX { to { transform: translateX(calc(var(--tile) * -1)); } }
  @keyframes winGlow { 0%, 100% { opacity: .2; } 50% { opacity: 1; } }

  .sl {
    position: absolute; bottom: 140px; width: 110px; height: 440px;
    transform-origin: 50% 100%;
    background: linear-gradient(0deg, rgba(255,240,200,.34), rgba(255,240,200,0));
    clip-path: polygon(46% 100%, 54% 100%, 100% 0, 0 0);
    animation: slSweep 7s ease-in-out infinite alternate;
  }
  .sl1 { left: 12%; transform: rotate(-24deg); }
  .sl2 { left: 84%; animation-duration: 9s; animation-delay: -4s; }
  @keyframes slSweep { from { transform: rotate(-26deg); } to { transform: rotate(26deg); } }

  .heli { position: absolute; top: 11%; left: 0; width: 90px; animation: heliFly 34s linear -9s infinite; }
  .heli svg { display: block; width: 90px; height: auto; animation: heliBob 2.4s ease-in-out infinite; }
  .heli .rotor { transform-box: fill-box; transform-origin: center; animation: rotorSpin .12s linear infinite; }
  .heli .nav { animation: navBlink 1.1s steps(1) infinite; }
  .heli .beam {
    position: absolute; left: -22px; top: 40px; width: 140px; height: 360px;
    transform-origin: 50% 0;
    background: linear-gradient(180deg, rgba(255,255,225,.5), rgba(255,255,225,0));
    clip-path: polygon(46% 0, 54% 0, 100% 100%, 0 100%);
    animation: beamSway 5s ease-in-out infinite alternate;
  }
  @keyframes heliFly { from { transform: translateX(-18vw); } to { transform: translateX(118vw); } }
  @keyframes heliBob { 50% { transform: translateY(5px) rotate(1.500deg); } }
  @keyframes rotorSpin { 0%, 100% { transform: scaleX(1); } 50% { transform: scaleX(.25); } }
  @keyframes navBlink { 50% { opacity: 0; } }
  @keyframes beamSway { from { transform: rotate(-9deg); } to { transform: rotate(11deg); } }

  .plane { position: absolute; left: 0; top: 7%; animation: planeFly 58s linear -24s infinite; }
  .plane.p2 { top: 19%; animation-duration: 82s; animation-delay: -60s; animation-direction: reverse; }
  .plane i { display: block; width: 4px; height: 4px; border-radius: 50%; background: #fff; box-shadow: 0 0 8px 2px rgba(255,255,255,.7); animation: strobe 1.8s steps(1) infinite; }
  .plane i::after { content: ""; position: absolute; left: -14px; top: 0; width: 3px; height: 3px; border-radius: 50%; background: #ff3b30; box-shadow: 0 0 8px 2px rgba(255,59,48,.7); }
  @keyframes planeFly { from { transform: translateX(-6vw); } to { transform: translateX(106vw); } }
  @keyframes strobe { 0%, 88% { opacity: .15; } 90%, 100% { opacity: 1; } }

  @media (prefers-reduced-motion: reduce) {
    .layer, .layer > .wa, .layer > .wb, .sl, .heli, .heli svg, .heli .rotor, .heli .nav, .heli .beam, .plane, .plane i { animation: none; }
    .heli, .plane { display: none; }
  }

  .moon {
    position: absolute; top: 9%; right: 13%; width: 54px; height: 54px; border-radius: 50%;
    background: radial-gradient(circle at 36% 34%, #fffaf0, #e8dcc2 58%, #b3a283);
    box-shadow: 0 0 30px 8px rgba(255,236,200,.32), 0 0 90px 30px rgba(255,200,140,.10);
    animation: moonFloat 50s ease-in-out infinite alternate;
  }
  @keyframes moonFloat { to { transform: translate(-60px, 12px); } }

  .smoke circle { fill: #aeb4bb; opacity: 0; transform-box: fill-box; transform-origin: center; animation: puff 2.4s ease-out infinite; }
  @keyframes puff {
    0%   { opacity: 0;   transform: translate(0, 0) scale(.6); }
    12%  { opacity: .42; }
    100% { opacity: 0;   transform: translate(-95px, -48px) scale(4.4); }
  }
  .dust circle { fill: #8c7d68; opacity: 0; transform-box: fill-box; transform-origin: center; animation: dust 1.35s ease-out infinite; }
  @keyframes dust {
    0%   { opacity: 0;   transform: translate(0, 0) scale(.5); }
    15%  { opacity: .38; }
    100% { opacity: 0;   transform: translate(-70px, -16px) scale(3.2); }
  }

  .road::before {
    content: ""; position: absolute; inset: 0; pointer-events: none;
    background:
      linear-gradient(90deg, transparent, rgba(255,255,255,.16), transparent) 0 20% / 300px 1px repeat-x,
      linear-gradient(90deg, transparent, rgba(255,255,255,.12), transparent) 120px 52% / 520px 1.5px repeat-x,
      linear-gradient(90deg, transparent, rgba(255,255,255,.10), transparent) 60px 82% / 240px 1px repeat-x;
    animation: streaks .6s linear infinite;
  }
  @keyframes streaks { to { background-position: -300px 20%, -400px 52%, -180px 82%; } }

  .traffic { position: absolute; left: 0; right: 0; top: 0; height: 30px; overflow: hidden; pointer-events: none; }
  .car { position: absolute; left: 100%; top: 4px; width: 92px; height: auto; animation: carPass 8s linear infinite; }
  .car.c1 { animation-duration: 7s;  animation-delay: -2s; }
  .car.c2 { animation-duration: 11s; animation-delay: -7s; top: 7px; width: 80px; }
  .car.c3 { animation-duration: 9s;  animation-delay: -1s; top: 3px; width: 100px; }
  @keyframes carPass { to { transform: translateX(calc(-100vw - 360px)); } }

  @media (prefers-reduced-motion: reduce) {
    .moon, .smoke circle, .dust circle, .road::before, .car { animation: none; }
    .smoke, .dust, .traffic { display: none; }
  }

  nav button.extra-tab svg { width:20px;height:20px;fill:none;stroke:currentColor;stroke-width:1.8;stroke-linecap:round;stroke-linejoin:round; }
  .extra-section[hidden] { display:none; }
  .stats-block.wide { max-width:1100px; }
  .toolbar,.add-row,.settings-actions { display:flex;gap:10px;align-items:center;flex-wrap:wrap;margin:0 0 16px; }
  .toolbar input,.add-row input,.settings-card input,.dark-select { min-width:0;background:var(--panel-2);border:1px solid #303035;border-radius:10px;color:var(--text);padding:12px 14px;font:inherit; }
  .toolbar input { flex:1; }
  .dark-select { cursor:pointer; }
  .muted-note,.setting-help,.finance-note { color:var(--muted);font-size:13px; }
  .table-wrap { width:100%;overflow-x:auto;border-radius:14px; }
  .table-wrap .stats { min-width:580px; }
  .table-wrap .stats td:last-child,.table-wrap .stats th:last-child { text-align:right; }
  .action-btn { background:var(--panel-2);border:1px solid #333339;color:var(--text);padding:7px 10px;border-radius:8px;cursor:pointer;font:inherit;font-size:13px; }
  .action-btn.danger { color:#ff8178;border-color:rgba(233,69,59,.35); }
  .empty-state { padding:24px;color:var(--muted);text-align:center; }
  .top-grid,.metric-grid { display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:12px;margin:0 0 18px; }
  .top-card,.metric,.settings-card { background:linear-gradient(145deg,rgba(233,69,59,.10),transparent 75%),var(--panel);border:1px solid var(--line);border-radius:14px;padding:18px; }
  .top-card span,.metric span { display:block;color:var(--muted);font-size:13px;margin-bottom:9px; }
  .top-card strong,.metric strong { display:block;font-size:24px;font-weight:800;color:var(--gold);overflow-wrap:anywhere; }
  .top-card small,.metric small { display:block;color:var(--muted);margin-top:7px; }
  .metric-grid { grid-template-columns:repeat(4,minmax(0,1fr)); }
  .finance-note { padding:14px 2px;line-height:1.6; }
  .count-pill { border:1px solid rgba(232,184,74,.3);background:rgba(232,184,74,.08);color:var(--gold);border-radius:99px;padding:8px 12px;font-size:13px;white-space:nowrap; }
  .add-row input { flex:1; }
  .small-primary { width:auto;padding:12px 18px;font-size:14px;box-shadow:none; }
  .settings-card { max-width:720px;display:grid;gap:10px; }
  .settings-card label:not(.file-label) { color:var(--text);font-weight:600;margin-top:8px; }
  .settings-card input,.settings-card select { width:100%; }
  .settings-actions { margin:12px 0 0; }
  .file-label { display:inline-flex;align-items:center;cursor:pointer; }
  @media(max-width:800px) { .metric-grid { grid-template-columns:repeat(2,minmax(0,1fr)); } .top-grid { grid-template-columns:1fr; } }
  @media(max-width:520px) { .metric-grid { grid-template-columns:1fr; }.stats-head { align-items:flex-start;flex-direction:column; }.settings-actions>* { width:100%;justify-content:center; } }
  
:root { color-scheme: dark; }

table.stats {
  background:
    linear-gradient(180deg, rgba(233,69,59,.10), transparent 120px),
    var(--panel) !important;
  border: 1px solid var(--line) !important;
  border-radius: 14px;
  color: var(--text) !important;
}
table.stats thead,
table.stats tbody,
table.stats tr {
  background: transparent !important;
}
table.stats th {
  background: transparent !important;
  color: var(--muted) !important;
  border: 0 !important;
  border-bottom: 1px solid var(--line) !important;
}
table.stats td {
  background: transparent !important;
  color: var(--text) !important;
  border: 0 !important;
  border-bottom: 1px solid var(--line) !important;
}
/* без «зебри» */
table.stats tbody tr:nth-child(even) td,
table.stats tbody tr:nth-child(odd) td {
  background: transparent !important;
}
table.stats tbody tr:hover td {
  background: rgba(255,255,255,.03) !important;
}
table.stats tr:last-child td { border-bottom: 0 !important; }

table.stats tr.paid td {
  background: rgba(76,195,90,.16) !important;
  color: var(--green) !important;
}

.pay .box {
  background: var(--panel-2) !important;
  border: 1.5px solid #46464b !important;
}
.pay input:checked + .box {
  background: var(--green) !important;
  border-color: var(--green) !important;
}

.table-wrap { background: transparent; }
/* ---------- Таблиці на всю ширину ---------- */
.stats-block,
.stats-block.wide {
  max-width: none !important;
  width: 100%;
}
table.stats {
  width: 100% !important;
  table-layout: auto;
}

/* Статистика: нік зліва, кнопка з контрактами прижата до правого краю */
.report th:first-child,
.report td:first-child {
  text-align: left;
  width: 100%;
}
.report th:last-child,
.report td:last-child {
  text-align: right;
  width: 210px;
  white-space: nowrap;
}
.report .count-btn {
  min-width: 160px;
}
/* ---------- Усі розділи на повну ширину ---------- */
.app { max-width: none; }

#calc, #stats, #report,
#history, #top, #finance, #roster, #settings {
  width: 100%;
}

.calc,
.stats-block,
.stats-block.wide,
.settings-card {
  max-width: none !important;
  width: 100%;
}

/* усі таблиці */
table.stats {
  width: 100% !important;
  table-layout: auto;
}
.table-wrap { width: 100%; }
.table-wrap .stats { min-width: 0; }

/* Статистика: нік зліва, кнопка контрактів по правому краю */
.report th:first-child,
.report td:first-child {
  text-align: left;
  width: 100%;
}
.report th:last-child,
.report td:last-child {
  text-align: right;
  width: 210px;
  white-space: nowrap;
}
.report .count-btn { min-width: 160px; }

/* Виплати: сума праворуч, чекбокс по центру фіксованої колонки */
.stats:not(.report) th:first-child,
.stats:not(.report) td:first-child { width: auto; }

/* Налаштування: поля на всю ширину картки */
.settings-card input,
.settings-card select { width: 100%; }

/* картки "Топ" і "Фінанси" розтягуються рівномірно */
.top-grid, .metric-grid { width: 100%; }
</style>
</head>
<body>
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <defs>
    <symbol id="i-stats" viewBox="0 0 24 24"><path d="M4 20V10h4v10zm6 0V4h4v16zm6 0v-7h4v7z"/></symbol>
    <symbol id="i-chart" viewBox="0 0 24 24"><path d="M3 3h2v16h16v2H3zm4 11 4-5 3 3 5-7 1.600 1.200-6.300 8.800L11 12.500 8.600 15.600z"/></symbol>
    <symbol id="i-truck" viewBox="0 0 24 24"><path d="M2 5h12v10H2zM14 8h4.500l3.500 4v3h-8z"/><circle cx="7" cy="17.500" r="2.200"/><circle cx="17" cy="17.500" r="2.200"/></symbol>
    <symbol id="v-car" viewBox="-50 0 110 24">
      <defs><linearGradient id="carBeam" x1="1" x2="0"><stop offset="0" stop-color="#fff3b0" stop-opacity=".6"/><stop offset="1" stop-color="#fff3b0" stop-opacity="0"/></linearGradient></defs>
      <polygon points="2,14 -50,8 -50,22 2,17" fill="url(#carBeam)"/>
      <path d="M2 18 L4 12 Q8 6 18 5 L38 5 Q46 6 50 11 L56 13 Q58 14 58 18 V20 H2Z" fill="currentColor"/>
      <path d="M12 12 Q16 7 22 7 L26 7 L26 12Z M29 7 L38 7 Q43 8 46 12 L29 12Z" fill="#0b1a24" opacity=".85"/>
      <circle cx="15" cy="20" r="4.200" fill="#0b0b0d"/><circle cx="46" cy="20" r="4.200" fill="#0b0b0d"/>
      <circle cx="15" cy="20" r="1.800" fill="#8b9097"/><circle cx="46" cy="20" r="1.800" fill="#8b9097"/>
      <circle cx="3" cy="15" r="2.200" fill="#fffbe0"/><rect x="56" y="14" width="3" height="3" rx="1" fill="#ff3b30"/>
    </symbol>
    <symbol id="i-calc" viewBox="0 0 24 24"><path d="M5 2h14a1 1 0 0 1 1 1v18a1 1 0 0 1-1 1H5a1 1 0 0 1-1-1V3a1 1 0 0 1 1-1zm1 2v4h12V4zm1 6v2h2v-2zm4 0v2h2v-2zm4 0v2h2v-2zm-8 4v2h2v-2zm4 0v2h2v-2zm4 0v6h2v-6zm-8 4v2h2v-2zm4 0v2h2v-2z"/></symbol>
  </defs>
</svg>

<div class="app">
  <aside class="sidebar">
    <div class="org-head">
      <div class="logo" aria-hidden="true">GOLD</div>
      <div class="org-name">GOLD</div>
    </div>
    <div class="menu-label">МЕНЮ</div>
    <nav>
      <button id="calcTab" aria-controls="calc"><svg><use href="#i-calc"/></svg>Розрахунок</button>
      <button id="statsTab" aria-controls="stats"><svg><use href="#i-stats"/></svg>Виплати</button>
      <button id="reportTab" aria-controls="report"><svg><use href="#i-chart"/></svg>Статистика</button>
      <button id="truckTab" aria-controls="truck"><svg><use href="#i-truck"/></svg>Далекобійник</button>
      <button class="extra-tab" data-tab="history" aria-controls="history"><svg viewBox="0 0 24 24"><path d="M12 8v5l3 2M3 12a9 9 0 1 0 2.6-6.4L3 8m0-5v5h5"/></svg>Історія операцій</button>
      <button class="extra-tab" data-tab="top" aria-controls="top"><svg viewBox="0 0 24 24"><path d="M3 19h18v2H3zm2-2V9h4v8zm6 0V4h4v13zm6 0v-5h4v5z"/></svg>Топ гравців</button>
      <button class="extra-tab" data-tab="finance" aria-controls="finance"><svg viewBox="0 0 24 24"><path d="M3 5h18v14H3zm0 4h18M7 15h4"/></svg>Фінанси команди</button>
      <button class="extra-tab" data-tab="roster" aria-controls="roster"><svg viewBox="0 0 24 24"><path d="M16 20v-1a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v1m7-9a4 4 0 1 0 0-8 4 4 0 0 0 0 8m9-3v6m-3-3h6"/></svg>Склад</button>
      <button class="extra-tab" data-tab="settings" aria-controls="settings"><svg viewBox="0 0 24 24"><path d="M12 8a4 4 0 1 0 0 8 4 4 0 0 0 0-8zm8 4a8 8 0 0 0-.1-1.3l2-1.5-2-3.5-2.4 1a8 8 0 0 0-2.2-1.3L15 3h-4l-.4 2.4a8 8 0 0 0-2.2 1.3l-2.4-1-2 3.5 2 1.5A8 8 0 0 0 6 12c0 .4 0 .9.1 1.3l-2 1.5 2 3.5 2.4-1a8 8 0 0 0 2.2 1.3L11 21h4l.4-2.4a8 8 0 0 0 2.2-1.3l2.4 1 2-3.5-2-1.5c.1-.4.1-.9.1-1.3z"/></svg>Налаштування</button>
    </nav>
  </aside>

  <main>
    <p class="empty" id="empty">Оберіть розділ у меню.</p>

    <section id="calc" hidden>
      <h1>Розрахунок</h1>
      <p class="sub">Вкажіть суму та оберіть учасників</p>

      <div class="calc">
        <div class="field">
          <label for="sum">Сума</label>
          <div class="money">
            <span>$</span>
            <input id="sum" type="number" min="0" step="any" inputmode="decimal" placeholder="0">
          </div>
        </div>

        <div class="field">
          <div class="label" id="ddLabel">Люди</div>
          <div class="dd" id="dd">
            <button class="dd-btn placeholder" id="ddBtn" type="button" aria-haspopup="true" aria-expanded="false" aria-labelledby="ddLabel ddBtn">
              <span id="ddText">Оберіть людей</span>
              <svg viewBox="0 0 24 24"><path d="M6 9l6 6 6-6"/></svg>
            </button>
            <div class="dd-list" id="ddList" hidden></div>
          </div>
        </div>

        <div class="result" aria-live="polite">
          <div class="label">Кожному</div>
          <p class="hint" id="hint">Введіть суму й оберіть хоча б одну людину.</p>
          <ul class="rows" id="rows"></ul>
        </div>

        <div class="actions">
          <button class="primary" id="calcBtn" type="button" disabled>Розрахувати</button>
        </div>
      </div>
    </section>

    <section id="stats" hidden>
      <div class="stats-block">
        <div class="stats-head">
          <h1>Виплати</h1>
          <div class="head-actions">
            <button class="ghost" id="refreshBtn" type="button">
              <svg viewBox="0 0 24 24"><path d="M20 12a8 8 0 1 1-2.400-5.700M20 4v5h-5"/></svg>Оновити
            </button>
          </div>
        </div>
        <table class="stats">
          <thead>
            <tr><th scope="col">Нік</th><th scope="col">Сума</th><th scope="col">Виплата</th></tr>
          </thead>
          <tbody id="statsBody"></tbody>
        </table>
      </div>
    </section>

    <section id="report" hidden>
      <div class="stats-block">
        <div class="stats-head">
          <h1>Статистика</h1>
          <button class="ghost" id="reportRefreshBtn" type="button">
            <svg viewBox="0 0 24 24"><path d="M20 12a8 8 0 1 1-2.400-5.700M20 4v5h-5"/></svg><span>Оновити</span>
          </button>
        </div>
        <p class="sub week-range" id="weekRange"></p>
        <table class="stats report">
          <thead>
            <tr>
              <th scope="col">Нікнейм</th>
              <th scope="col">Контракти за тиждень</th>
            </tr>
          </thead>
          <tbody id="reportBody"></tbody>
        </table>
      </div>
    </section>
    <section id="truck" hidden aria-label="Далекобійник">
      <button class="truck-back" id="truckBack" type="button">
        <svg viewBox="0 0 24 24"><path d="M15 5l-7 7 7 7"/></svg>Назад
      </button>
      <div class="truck-panel">
        <div class="hazard"></div>
        <div class="la" id="la" aria-hidden="true"></div>
        <div class="stars" aria-hidden="true"></div>
        <div class="truck-center">
          <div class="road-sign">Далекобійник</div>
          <div class="soon">
            <i class="beacon"></i>
            <h2>В розробці</h2>
            <i class="beacon b2"></i>
          </div>
          <p>Дорогу ще прокладаємо.</p>
        </div>
        <div class="road">
          <div class="traffic" aria-hidden="true">
            <svg class="car c1" viewBox="0 0 110 24" style="color:#c0392b"><use href="#v-car"/></svg>
            <svg class="car c2" viewBox="0 0 110 24" style="color:#2d6cdf"><use href="#v-car"/></svg>
            <svg class="car c3" viewBox="0 0 110 24" style="color:#d9d9d9"><use href="#v-car"/></svg>
          </div>
          <svg class="truck-svg" viewBox="-6 -22 576 170" aria-hidden="true">
            <defs>
              <linearGradient id="trailerG" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#454b55"/><stop offset=".45" stop-color="#2c3037"/><stop offset="1" stop-color="#1b1d22"/></linearGradient>
              <linearGradient id="cabG" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#ffd35a"/><stop offset=".4" stop-color="#ffb400"/><stop offset="1" stop-color="#a87300"/></linearGradient>
              <linearGradient id="chrome" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#f7f9fb"/><stop offset=".5" stop-color="#9ba1a8"/><stop offset="1" stop-color="#e3e6e9"/></linearGradient>
              <linearGradient id="glassG" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#2a6a86"/><stop offset="1" stop-color="#0a1c27"/></linearGradient>
              <linearGradient id="beam" x1="0" x2="1"><stop offset="0" stop-color="#fff3b0" stop-opacity=".6"/><stop offset=".85" stop-color="#fff3b0" stop-opacity="0"/></linearGradient>
              <radialGradient id="glowG"><stop offset="0" stop-color="#fff3b0" stop-opacity=".55"/><stop offset="1" stop-color="#fff3b0" stop-opacity="0"/></radialGradient>
              <radialGradient id="lampG"><stop offset="0" stop-color="#fff"/><stop offset=".6" stop-color="#fff3b0"/><stop offset="1" stop-color="#ffb400"/></radialGradient>
            </defs>

            <ellipse cx="280" cy="142" rx="280" ry="6" fill="#000" opacity=".55"/>
            <polygon points="532,76 800,24 800,138 532,90" fill="url(#beam)"/>
            <ellipse cx="640" cy="140" rx="190" ry="8" fill="url(#glowG)"/>

            <!-- рама, опора, сідельно-зчіпний пристрій -->
            <rect x="6" y="108" width="540" height="7" rx="2" fill="#0d0e10"/>
            <rect x="286" y="112" width="5" height="18" fill="#555a61"/><rect x="280" y="128" width="17" height="3" rx="1" fill="#3b3f45"/>
            <rect x="316" y="104" width="72" height="6" rx="2" fill="#17181b"/>

            <!-- причіп -->
            <rect x="6" y="14" width="324" height="92" rx="5" fill="url(#trailerG)" stroke="#14161a" stroke-width="1.500"/>
            <rect x="6" y="14" width="324" height="4" rx="2" fill="rgba(255,255,255,.14)"/>
            <line x1="30" y1="18" x2="30" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="48" y1="18" x2="48" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="66" y1="18" x2="66" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="84" y1="18" x2="84" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="102" y1="18" x2="102" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="120" y1="18" x2="120" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="138" y1="18" x2="138" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="156" y1="18" x2="156" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="174" y1="18" x2="174" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="192" y1="18" x2="192" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="210" y1="18" x2="210" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="228" y1="18" x2="228" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="246" y1="18" x2="246" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="264" y1="18" x2="264" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="282" y1="18" x2="282" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="300" y1="18" x2="300" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/><line x1="318" y1="18" x2="318" y2="102" stroke="rgba(255,255,255,.055)" stroke-width="1.5"/>
            <line x1="16" y1="18" x2="16" y2="102" stroke="rgba(0,0,0,.4)" stroke-width="1.500"/>
            <rect x="7" y="76" width="322" height="6" fill="#0057b7"/><rect x="7" y="82" width="322" height="6" fill="#ffd500"/>
            <path transform="translate(104 28) scale(1.700)" d="M3 8l4.500 4L12 5l4.500 7L21 8l-2 11H5z" fill="#ffb400"/>
            <text x="150" y="62" font-size="34" font-weight="900" letter-spacing="6" fill="#ffb400" font-family="Arial Black, Arial, sans-serif">GOLD</text>
            <circle cx="26" cy="20" r="2" fill="#ffb400" opacity=".85"/><circle cx="66" cy="20" r="2" fill="#ffb400" opacity=".85"/><circle cx="106" cy="20" r="2" fill="#ffb400" opacity=".85"/><circle cx="146" cy="20" r="2" fill="#ffb400" opacity=".85"/><circle cx="186" cy="20" r="2" fill="#ffb400" opacity=".85"/><circle cx="226" cy="20" r="2" fill="#ffb400" opacity=".85"/><circle cx="266" cy="20" r="2" fill="#ffb400" opacity=".85"/><circle cx="306" cy="20" r="2" fill="#ffb400" opacity=".85"/>
            <rect x="62" y="104" width="214" height="8" rx="2" fill="#16181c"/>
            <rect x="4" y="92" width="5" height="12" rx="1" fill="#e0281e"/><rect x="-2" y="108" width="34" height="6" rx="2" fill="url(#chrome)"/>
            <rect x="44" y="108" width="92" height="6" rx="3" fill="#15171a"/>
            <rect x="140" y="112" width="8" height="24" rx="1" fill="#0d0e10"/><rect x="142" y="116" width="4" height="4" fill="#ffb400"/>

            <!-- вертикальні вихлопні труби -->
            <rect x="332" y="-18" width="6" height="84" rx="3" fill="url(#chrome)"/><rect x="339" y="-14" width="5" height="80" rx="2.500" fill="url(#chrome)"/>
            <rect x="331" y="-19" width="8" height="5" rx="2" fill="#2b2d31"/><rect x="338" y="-15" width="7" height="5" rx="2" fill="#2b2d31"/>

            <!-- спальник і кабіна -->
            <rect x="346" y="22" width="56" height="84" rx="5" fill="url(#cabG)" stroke="#8a5f00" stroke-width="1"/>
            <path d="M392 106 V40 Q392 34 398 34 H420 Q428 34 432 42 L444 68 V106 Z" fill="url(#cabG)" stroke="#8a5f00" stroke-width="1"/>
            <rect x="392" y="28" width="40" height="6" rx="2" fill="#d89700" stroke="#8a5f00" stroke-width="1"/>
            <path d="M350 23 H398" stroke="rgba(255,255,255,.55)" stroke-width="1.500" fill="none"/>
            <rect x="356" y="38" width="30" height="20" rx="2" fill="url(#glassG)" stroke="#5a3f00" stroke-width="1.500"/>
            <polygon points="400,42 422,42 437,66 400,66" fill="url(#glassG)" stroke="#5a3f00" stroke-width="1.500"/>
            <polygon points="404,45 416,45 408,63 404,63" fill="rgba(255,255,255,.2)"/>
            <rect x="400" y="70" width="38" height="34" rx="3" fill="none" stroke="#8a5f00" stroke-width="1.200"/>
            <rect x="404" y="76" width="12" height="3.500" rx="1.500" fill="#1b1b1b"/>
            <rect x="346" y="88" width="98" height="5" fill="#111" opacity=".25"/>

            <!-- довгий капот -->
            <path d="M444 72 H512 Q522 72 525 80 V106 H444 Z" fill="url(#cabG)" stroke="#8a5f00" stroke-width="1"/>
            <path d="M446 73 H512" stroke="rgba(255,255,255,.6)" stroke-width="1.500" fill="none"/>
            <path d="M446 90 H524" stroke="#e8e8e8" stroke-width="1.500" fill="none" opacity=".75"/>
            <g stroke="rgba(0,0,0,.28)" stroke-width="1.500"><line x1="458" y1="78" x2="458" y2="88"/><line x1="466" y1="78" x2="466" y2="88"/><line x1="474" y1="78" x2="474" y2="88"/></g>
            <rect x="518" y="64" width="14" height="46" rx="3" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/>
            <line x1="522" y1="68" x2="522" y2="106" stroke="#555a61" stroke-width="1"/><line x1="525" y1="68" x2="525" y2="106" stroke="#555a61" stroke-width="1"/><line x1="528" y1="68" x2="528" y2="106" stroke="#555a61" stroke-width="1"/>
            <ellipse cx="528" cy="76" rx="3.500" ry="5" fill="url(#lampG)"/>
            <rect x="514" y="110" width="40" height="9" rx="3.500" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/>
            <circle cx="530" cy="102" r="2.800" fill="#fff3b0"/>
            <!-- дзеркало -->
            <line x1="440" y1="52" x2="448" y2="46" stroke="#222" stroke-width="2.500"/><rect x="446" y="34" width="8" height="30" rx="2" fill="#15171a" stroke="#8b9097" stroke-width="1"/>
            <!-- паливний бак -->
            <rect x="426" y="97" width="44" height="23" rx="10" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/>
            <line x1="438" y1="97" x2="438" y2="120" stroke="#555a61" stroke-width="1.500"/><line x1="458" y1="97" x2="458" y2="120" stroke="#555a61" stroke-width="1.500"/>
            <rect x="350" y="104" width="82" height="6" rx="3" fill="#a87300"/>

            <g class="wheel"><circle cx="70" cy="121" r="19" fill="#0d0e10"/><circle cx="70" cy="121" r="17" fill="none" stroke="#25272c" stroke-width="2" stroke-dasharray="3 2"/><circle cx="70" cy="121" r="12" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/><circle cx="70" cy="121" r="7.500" fill="#d4d8dc" stroke="#8b9097" stroke-width=".8"/><circle cx="79.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="74.8" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="65.2" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="60.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="65.2" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="74.8" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="70" cy="121" r="3.800" fill="#ffb400"/><rect x="68.5" y="105.5" width="3" height="4" rx="1" fill="#ffb400"/></g><g class="wheel"><circle cx="112" cy="121" r="19" fill="#0d0e10"/><circle cx="112" cy="121" r="17" fill="none" stroke="#25272c" stroke-width="2" stroke-dasharray="3 2"/><circle cx="112" cy="121" r="12" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/><circle cx="112" cy="121" r="7.500" fill="#d4d8dc" stroke="#8b9097" stroke-width=".8"/><circle cx="121.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="116.8" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="107.2" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="102.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="107.2" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="116.8" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="112" cy="121" r="3.800" fill="#ffb400"/><rect x="110.5" y="105.5" width="3" height="4" rx="1" fill="#ffb400"/></g><g class="wheel"><circle cx="366" cy="121" r="19" fill="#0d0e10"/><circle cx="366" cy="121" r="17" fill="none" stroke="#25272c" stroke-width="2" stroke-dasharray="3 2"/><circle cx="366" cy="121" r="12" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/><circle cx="366" cy="121" r="7.500" fill="#d4d8dc" stroke="#8b9097" stroke-width=".8"/><circle cx="375.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="370.8" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="361.2" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="356.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="361.2" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="370.8" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="366" cy="121" r="3.800" fill="#ffb400"/><rect x="364.5" y="105.5" width="3" height="4" rx="1" fill="#ffb400"/></g><g class="wheel"><circle cx="408" cy="121" r="19" fill="#0d0e10"/><circle cx="408" cy="121" r="17" fill="none" stroke="#25272c" stroke-width="2" stroke-dasharray="3 2"/><circle cx="408" cy="121" r="12" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/><circle cx="408" cy="121" r="7.500" fill="#d4d8dc" stroke="#8b9097" stroke-width=".8"/><circle cx="417.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="412.8" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="403.2" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="398.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="403.2" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="412.8" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="408" cy="121" r="3.800" fill="#ffb400"/><rect x="406.5" y="105.5" width="3" height="4" rx="1" fill="#ffb400"/></g><g class="wheel"><circle cx="486" cy="121" r="19" fill="#0d0e10"/><circle cx="486" cy="121" r="17" fill="none" stroke="#25272c" stroke-width="2" stroke-dasharray="3 2"/><circle cx="486" cy="121" r="12" fill="url(#chrome)" stroke="#6b7077" stroke-width="1"/><circle cx="486" cy="121" r="7.500" fill="#d4d8dc" stroke="#8b9097" stroke-width=".8"/><circle cx="495.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="490.8" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="481.2" cy="129.2" r="1.5" fill="#4a4e55"/><circle cx="476.5" cy="121.0" r="1.5" fill="#4a4e55"/><circle cx="481.2" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="490.8" cy="112.8" r="1.5" fill="#4a4e55"/><circle cx="486" cy="121" r="3.800" fill="#ffb400"/><rect x="484.5" y="105.5" width="3" height="4" rx="1" fill="#ffb400"/></g>
            <path d="M461 114 A25 25 0 0 1 511 114" fill="none" stroke="#ffb400" stroke-width="6" stroke-linecap="round"/>
            <path d="M461 114 A25 25 0 0 1 511 114" fill="none" stroke="rgba(255,255,255,.4)" stroke-width="1.200" transform="translate(0 -2)"/>
          
            <g class="smoke" aria-hidden="true">
              <circle cx="335" cy="-20" r="4" style="animation-delay:0s"/><circle cx="335" cy="-20" r="4" style="animation-delay:-.6s"/>
              <circle cx="335" cy="-20" r="4" style="animation-delay:-1.2s"/><circle cx="335" cy="-20" r="4" style="animation-delay:-1.8s"/>
              <circle cx="342" cy="-16" r="3.500" style="animation-delay:-.3s"/><circle cx="342" cy="-16" r="3.500" style="animation-delay:-.9s"/>
              <circle cx="342" cy="-16" r="3.500" style="animation-delay:-1.5s"/><circle cx="342" cy="-16" r="3.500" style="animation-delay:-2.1s"/>
            </g>
            <g class="dust" aria-hidden="true">
              <circle cx="60" cy="136" r="5" style="animation-delay:0s"/><circle cx="60" cy="136" r="5" style="animation-delay:-.45s"/><circle cx="60" cy="136" r="5" style="animation-delay:-.9s"/>
              <circle cx="400" cy="136" r="5" style="animation-delay:-.2s"/><circle cx="400" cy="136" r="5" style="animation-delay:-.65s"/><circle cx="400" cy="136" r="5" style="animation-delay:-1.1s"/>
            </g>
          </svg>
          <div class="lane"></div>
        </div>
        <div class="hazard"></div>
      </div>
    </section>

    <section id="history" class="extra-section" hidden>
      <div class="stats-block wide">
        <div class="stats-head"><div><h1>Історія операцій</h1><p class="sub">Усі нарахування команди в хронологічному порядку</p></div><button class="ghost" id="historyExport">Експорт CSV</button></div>
        <div class="toolbar"><input id="historySearch" type="search" placeholder="Пошук за гравцем…"><span id="historyCount" class="muted-note"></span></div>
        <div class="table-wrap"><table class="stats"><thead><tr><th>Дата й час</th><th>Гравець</th><th>Виплата</th><th>Дія</th></tr></thead><tbody id="historyBody"></tbody></table></div>
        <p class="empty-state" id="historyEmpty" hidden>Операцій поки немає.</p>
      </div>
    </section>

    <section id="top" class="extra-section" hidden>
      <div class="stats-block wide"><div class="stats-head"><div><h1>Топ гравців</h1><p class="sub">Рейтинг за кількістю виконаних контрактів</p></div><select id="topPeriod" class="dark-select"><option value="week">Цей тиждень</option><option value="month">Цей місяць</option><option value="all">За весь час</option></select></div>
        <div id="topCards" class="top-grid"></div>
        <div class="table-wrap"><table class="stats"><thead><tr><th>Місце</th><th>Гравець</th><th>Контракти</th><th>Зароблено</th></tr></thead><tbody id="topBody"></tbody></table></div>
      </div>
    </section>

    <section id="finance" class="extra-section" hidden>
      <div class="stats-block wide"><div class="stats-head"><div><h1>Фінанси команди</h1><p class="sub">Зведення за всіма записаними нарахуваннями</p></div><button class="ghost" id="financeRefresh">Оновити</button></div>
        <div class="metric-grid">
          <article class="metric"><span>Загальна сума контрактів</span><strong id="grossTotal">$ 0</strong><small>Оцінка до відрахування частки</small></article>
          <article class="metric"><span>Нараховано гравцям</span><strong id="paidTotal">$ 0</strong><small>Сума записаних виплат</small></article>
          <article class="metric"><span>Частка команди</span><strong id="teamCut">$ 0</strong><small id="teamCutHint">Відповідно до налаштувань</small></article>
          <article class="metric"><span>Кількість контрактів</span><strong id="contractTotal">0</strong><small>Кількість записів у журналі</small></article>
        </div>
        <div class="finance-note">Розрахунок враховує лише збережені виплати. Якщо суми округлювалися до копійок, загальна сума може трохи відрізнятися від початкової суми контрактів.</div>
      </div>
    </section>

    <section id="roster" class="extra-section" hidden>
      <div class="stats-block wide"><div class="stats-head"><div><h1>Управління складом</h1><p class="sub">Додавайте, перейменовуйте та прибирайте учасників</p></div><span class="count-pill" id="rosterCount">0 гравців</span></div>
        <form id="addPlayerForm" class="add-row"><input id="newPlayerName" type="text" maxlength="60" placeholder="Нікнейм або ID гравця" required><button class="primary small-primary" type="submit">Додати гравця</button></form>
        <div class="table-wrap"><table class="stats"><thead><tr><th>Учасник</th><th>Дії</th></tr></thead><tbody id="rosterBody"></tbody></table></div>
      </div>
    </section>

    <section id="settings" class="extra-section" hidden>
      <div class="stats-block wide"><div class="stats-head"><div><h1>Налаштування GOLD</h1><p class="sub">Основні параметри розрахунку та збереження</p></div></div>
        <div class="settings-card"><label for="sharePercent">Відрахування команди (%)</label><input id="sharePercent" type="number" min="0" max="99" step="1"><p class="setting-help">Наприклад, 30% означає, що гравцям розподіляється 70% суми контракту.</p>
          <label for="currencySelect">Валюта</label><select id="currencySelect" class="dark-select"><option value="$">Долар ($)</option><option value="₴">Гривня (₴)</option><option value="€">Євро (€)</option></select>
          <div class="settings-actions"><button class="primary small-primary" id="saveSettings" type="button">Зберегти налаштування</button><button class="ghost" id="backupData" type="button">Завантажити резервну копію</button><label class="ghost file-label">Відновити копію<input id="restoreData" type="file" accept="application/json" hidden></label></div>
          <p id="settingsStatus" class="setting-help" role="status">Налаштування зберігаються в цьому браузері.</p>
        </div>
      </div>
    </section>
  </main>
</div>

<div class="modal" id="modal" hidden>
  <div class="modal-card" role="dialog" aria-modal="true" aria-labelledby="mName">
    <button class="modal-close" id="mClose" type="button" aria-label="Закрити">
      <svg viewBox="0 0 24 24"><path d="M6 6l12 12M18 6L6 18"/></svg>
    </button>
    <div class="m-head">
      <div class="m-avatar" id="mAvatar"></div>
      <div class="m-title">
        <div class="m-sub">Статистика гравця</div>
        <h3 id="mName"></h3>
      </div>
    </div>
    <div class="m-body">
      <div class="donut" id="mDonut"></div>
      <ul class="legend" id="mLegend"></ul>
    </div>
    <div class="m-earn">
      <div>
        <div class="m-earn-label">Зароблено за весь час</div>
        <div class="m-earn-sum" id="mEarn">$ 0</div>
      </div>
      <div class="m-earn-share" id="mShare"></div>
    </div>
  </div>
</div>

<script>
  let SHARE_LEFT = 0.7; // частка суми, що розподіляється між гравцями
  let CURRENCY = '$';
  try { const cfg = JSON.parse(localStorage.getItem('gold-settings') || '{}'); if (Number.isFinite(cfg.sharePercent)) SHARE_LEFT = 1 - Math.min(99, Math.max(0, cfg.sharePercent)) / 100; if (['$','₴','€'].includes(cfg.currency)) CURRENCY = cfg.currency; } catch (e) {}

  const calcTab = document.getElementById('calcTab');
  const calc = document.getElementById('calc');
  const empty = document.getElementById('empty');
  const sum = document.getElementById('sum');
  const dd = document.getElementById('dd');
  const ddBtn = document.getElementById('ddBtn');
  const ddList = document.getElementById('ddList');
  const ddText = document.getElementById('ddText');
  const rows = document.getElementById('rows');
  const hint = document.getElementById('hint');
  const calcBtn = document.getElementById('calcBtn');
  const NAMES = [
    "AC. Damon_Smith | 50584",
    "AlexNych50097",
    "Oleksa Zhabka",
    "Roman Snow | 49747",
    "Semen Shpalokhod 4982...",
    "stepan zhulkevich/ 42251",
    "Tata Urban / 46314",
    "Vitalik Buyniy / 49798 (D...",
    "Сєргo",
    "4| Alex_GoodWin...",
    "Arno Crown #16907",
    "Connor_Cobra | 12489...",
    "Tom_Hardi / #23...",
    "Valera Jonson | 49749",
    "Vovan Prystupa 50527"
];
  try { const savedRoster = JSON.parse(localStorage.getItem('gold-roster') || 'null'); if (Array.isArray(savedRoster) && savedRoster.length) { NAMES.splice(0, NAMES.length, ...savedRoster.filter(x => typeof x === 'string' && x.trim()).slice(0, 200)); } } catch (e) {}
  NAMES.forEach(n => {
    const label = document.createElement('label');
    label.className = 'opt';
    label.innerHTML = '<input type="checkbox"><span class="box"><svg viewBox="0 0 24 24"><path d="M5 12l5 5 9-10"/></svg></span><span class="opt-name"></span>';
    label.querySelector('input').value = n;
    label.querySelector('.opt-name').textContent = n;
    ddList.appendChild(label);
  });
  let checks = ddList.querySelectorAll('input[type="checkbox"]');

  const statsTab = document.getElementById('statsTab');
  const reportTab = document.getElementById('reportTab');
  const statsSec = document.getElementById('stats');
  const reportSec = document.getElementById('report');
  const truckTab = document.getElementById('truckTab');
  const truckSec = document.getElementById('truck');
  const TABS = {
    calc:   { btn: calcTab,   sec: calc },
    stats:  { btn: statsTab,  sec: statsSec },
    report: { btn: reportTab, sec: reportSec }
  };
  let current = null;
  function showTab(tab) {
    // повторне натискання на відкритий розділ закриває його
    current = current === tab ? null : tab;
    document.querySelectorAll('.extra-section').forEach(sec => sec.hidden = true);
    document.querySelectorAll('.extra-tab').forEach(btn => btn.classList.remove('active'));
    empty.hidden = current !== null;
    Object.entries(TABS).forEach(([key, t]) => {
      t.sec.hidden = key !== current;
      t.btn.classList.toggle('active', key === current);
      t.btn.setAttribute('aria-expanded', key === current);
    });
    if (current === 'report') renderReport();
    if (current) playEnter(TABS[current].sec);
    if (current) TABS[current].sec.scrollIntoView({ behavior: 'smooth', block: 'start' });
  }
  calcTab.addEventListener('click', () => { currentExtra=null; showTab('calc'); if (current === 'calc') sum.focus(); });
  statsTab.addEventListener('click', () => { currentExtra=null; showTab('stats'); });
  reportTab.addEventListener('click', () => { currentExtra=null; showTab('report'); });
  const truckBack = document.getElementById('truckBack');
  function applyTruck(open) {
    if (truckSec.hidden === !open) return;
    truckSec.hidden = !open;
    document.body.style.overflow = open ? 'hidden' : '';
    if (open) { truckBack.focus(); window.scrollTo(0, 0); }
    else truckTab.focus();
  }
  truckTab.addEventListener('click', () => {
    applyTruck(true);
    try { location.hash = 'truck'; } catch (e) {}
  });
  function closeTruck() {
    applyTruck(false);
    try { if (location.hash === '#truck') history.back(); } catch (e) {}
  }
  truckBack.addEventListener('click', closeTruck);
  document.addEventListener('keydown', e => { if (e.key === 'Escape' && !truckSec.hidden && modal.hidden) closeTruck(); });
  window.addEventListener('hashchange', () => applyTruck(location.hash === '#truck'));
  if (location.hash === '#truck') applyTruck(true);

  function setOpen(open) {
    ddList.hidden = !open;
    dd.classList.toggle('open', open);
    ddBtn.setAttribute('aria-expanded', open);
  }
  ddBtn.addEventListener('click', () => setOpen(ddList.hidden));
  document.addEventListener('click', e => { if (!dd.contains(e.target)) setOpen(false); });
  document.addEventListener('keydown', e => { if (e.key === 'Escape') { setOpen(false); ddBtn.focus(); } });

  const fmt = n => CURRENCY + ' ' + Number(n || 0).toLocaleString('uk-UA', { minimumFractionDigits: 0, maximumFractionDigits: 2 });

  function update() {
    const names = [...checks].filter(c => c.checked).map(c => c.value);
    ddText.textContent = names.length ? names.join(', ') : 'Оберіть людей';
    ddBtn.classList.toggle('placeholder', !names.length);

    const total = parseFloat(sum.value);
    rows.innerHTML = '';

    calcBtn.disabled = !(total > 0) || !names.length;
    if (!(total > 0) || !names.length) {
      hint.hidden = false;
      return;
    }
    hint.hidden = true;

    const each = Math.floor(total * SHARE_LEFT / names.length * 100) / 100;
    names.forEach(n => {
      const li = document.createElement('li');
      li.innerHTML = '<span class="name"></span><span class="sum"></span>';
      li.children[0].textContent = n;
      li.children[1].textContent = fmt(each);
      rows.appendChild(li);
    });
  }

  sum.addEventListener('input', update);
  checks.forEach(c => c.addEventListener('change', update));

  /* ---------- Виплати ---------- */
  const statsBody = document.getElementById('statsBody');
  const refreshBtn = document.getElementById('refreshBtn');
  const reportBody = document.getElementById('reportBody');
  const totals = Object.fromEntries(NAMES.map(n => [n, 0]));
  const paid = Object.fromEntries(NAMES.map(n => [n, false]));

  function renderStats() {
    statsBody.innerHTML = '';
    NAMES.forEach(n => {
      const tr = document.createElement('tr');
      tr.classList.toggle('paid', paid[n]);
      tr.innerHTML =
        '<td></td><td></td>' +
        '<td><label class="pay"><input type="checkbox"><span class="box">' +
        '<svg viewBox="0 0 24 24"><path d="M5 12l5 5 9-10"/></svg></span></label></td>';
      tr.children[0].textContent = n;
      tr.children[1].textContent = fmt(totals[n]);
      const cb = tr.querySelector('input');
      cb.checked = paid[n];
      cb.setAttribute('aria-label', 'Виплачено: ' + n);
      cb.addEventListener('change', () => {
        paid[n] = cb.checked;
        if (typeof persistPaid === 'function') persistPaid();
        tr.classList.toggle('paid', cb.checked);
      });
      statsBody.appendChild(tr);
    });
  }

  calcBtn.addEventListener('click', () => {
    const names = [...checks].filter(c => c.checked).map(c => c.value);
    const total = parseFloat(sum.value);
    if (!(total > 0) || !names.length) return;

    const each = Math.floor(total * SHARE_LEFT / names.length * 100) / 100;
    const br = calcBtn.getBoundingClientRect();
    burst(br.left + br.width / 2, br.top + br.height / 2);
    const now = Date.now();
    names.forEach(n => {
      totals[n] = Math.round((totals[n] + each) * 100) / 100;
      log.push({ t: now, n, a: each }); // 1 виконаний контракт у статистику
    });
    renderStats();
    renderReport();
    saveLog();
    names.forEach(n => { const row = statsBody.children[NAMES.indexOf(n)]; if (row) row.classList.add('flash'); });

    sum.value = '';
    update();
    sum.focus();
  });

  refreshBtn.addEventListener('click', () => {
    recalculateTotals();
    renderStats();
  });

  /* ---------- Статистика (накопичення) ---------- */
  const STORE_KEY = 'gold-contracts-log';
  let log = []; // { t: час збереження (мс), n: нік, a: сума }

  async function loadLog() {
    try {
      if (window.storage) {
        const r = await window.storage.get(STORE_KEY);
        if (r && r.value) return JSON.parse(r.value);
      } else if (window.localStorage) {
        const v = localStorage.getItem(STORE_KEY);
        if (v) return JSON.parse(v);
      }
    } catch (e) { /* немає збережених даних */ }
    return [];
  }
  async function saveLog() {
    try {
      const v = JSON.stringify(log);
      if (window.storage) await window.storage.set(STORE_KEY, v);
      else if (window.localStorage) localStorage.setItem(STORE_KEY, v);
    } catch (e) { /* зберігання недоступне — дані лишаються до перезавантаження */ }
  }

  function periodStarts() {
    const d = new Date();
    const day = new Date(d.getFullYear(), d.getMonth(), d.getDate());
    const week = new Date(day); week.setDate(day.getDate() - ((day.getDay() + 6) % 7)); // понеділок
    const month = new Date(d.getFullYear(), d.getMonth(), 1);
    return { day: +day, week: +week, month: +month };
  }

  function playerStats(name) {
    const st = periodStarts();
    const count = (list, from) => list.filter(e => e.t >= from).length;
    const mine = log.filter(e => e.n === name);
    return {
      day:   { me: count(mine, st.day),   all: count(log, st.day) },
      week:  { me: count(mine, st.week),  all: count(log, st.week) },
      month: { me: count(mine, st.month), all: count(log, st.month) },
      total: mine.length,
      earned: Math.round(mine.reduce((a, e) => a + e.a, 0) * 100) / 100,
      teamEarned: Math.round(log.reduce((a, e) => a + e.a, 0) * 100) / 100
    };
  }

  // тиждень: з понеділка 00:00 до наступного понеділка 00:00 (пн–нд)
  function weekBounds() {
    const start = new Date(periodStarts().week);
    const end = new Date(start); end.setDate(end.getDate() + 7);
    return { start: +start, end: +end, last: new Date(+end - 1), first: start };
  }
  const ddmm = d => String(d.getDate()).padStart(2, '0') + '.' + String(d.getMonth() + 1).padStart(2, '0');

  function renderReport() {
    const wk = weekBounds();
    document.getElementById('weekRange').textContent =
      'Поточний тиждень: ' + ddmm(wk.first) + ' – ' + ddmm(wk.last) + ' (пн–нд), у порядку за кількістю контрактів';

    const counts = {};
    NAMES.forEach(n => { counts[n] = 0; });
    log.forEach(e => { if (e.t >= wk.start && e.t < wk.end && e.n in counts) counts[e.n]++; });
    // найбільше контрактів — вгорі; при рівності зберігається початковий порядок
    const order = NAMES.map((n, i) => ({ n, i, c: counts[n] })).sort((x, y) => y.c - x.c || x.i - y.i);

    // позиції рядків до перебудови (для плавного підйому вгору)
    const prev = new Map();
    [...reportBody.children].forEach(tr => prev.set(tr.dataset.name, tr.getBoundingClientRect().top));

    reportBody.innerHTML = '';
    order.forEach(({ n, c }) => {
      const tr = document.createElement('tr');
      tr.dataset.name = n;
      tr.innerHTML = '<td></td><td><button class="click-btn count-btn" type="button"><b></b><span></span></button></td>';
      tr.children[0].textContent = n;
      const btn = tr.querySelector('.click-btn');
      btn.querySelector('b').textContent = c;
      btn.querySelector('span').textContent = plural(c);
      btn.classList.toggle('zero', c === 0);
      btn.setAttribute('aria-label', n + ': ' + c + ' ' + plural(c) + ' за тиждень. Відкрити статистику');
      btn.addEventListener('click', () => openModal(n, btn));
      reportBody.appendChild(tr);
    });

    if (prev.size && !matchMedia('(prefers-reduced-motion: reduce)').matches) {
      [...reportBody.children].forEach(tr => {
        const old = prev.get(tr.dataset.name);
        const now = tr.getBoundingClientRect().top;
        if (old && now && old !== now && tr.animate) {
          tr.animate([{ transform: 'translateY(' + (old - now) + 'px)' }, { transform: 'none' }],
                     { duration: 520, easing: 'cubic-bezier(.2,.8,.2,1)' });
        }
      });
    }
  }

  /* ---------- Вікно зі статистикою гравця ---------- */
  const modal = document.getElementById('modal');
  const mClose = document.getElementById('mClose');
  let lastTrigger = null;
  let countRaf = 0;

  const SVG_NS = 'http://www.w3.org/2000/svg';
  const RINGS = [
    { key: 'day',   label: 'Сьогодні',   color: '#e9453b', r: 96 },
    { key: 'week',  label: 'Цей тиждень', color: '#e8b84a', r: 76 },
    { key: 'month', label: 'Цей місяць',  color: '#4cc35a', r: 56 }
  ];
  function plural(n) {
    const m10 = n % 10, m100 = n % 100;
    if (m10 === 1 && m100 !== 11) return 'контракт';
    if (m10 >= 2 && m10 <= 4 && (m100 < 12 || m100 > 14)) return 'контракти';
    return 'контрактів';
  }
  const pct = (me, all) => all > 0 ? Math.round(me / all * 100) : 0;

  function countUp(el, to, ms) {
    cancelAnimationFrame(countRaf);
    if (matchMedia('(prefers-reduced-motion: reduce)').matches || to === 0) { el.textContent = fmt(to); return; }
    const t0 = performance.now();
    const tick = now => {
      const k = Math.min(1, (now - t0) / ms);
      const eased = 1 - Math.pow(1 - k, 3);
      el.textContent = fmt(Math.round(to * eased * 100) / 100);
      if (k < 1) countRaf = requestAnimationFrame(tick);
    };
    countRaf = requestAnimationFrame(tick);
  }

  function openModal(name, trigger) {
    lastTrigger = trigger;
    const d = playerStats(name);

    document.getElementById('mName').textContent = name;
    const ch = (name.match(/[\p{L}\p{N}]/u) || ['?'])[0];
    document.getElementById('mAvatar').textContent = ch;

    // кільцева діаграма: кожне кільце — частка гравця від усіх контрактів за період
    const donut = document.getElementById('mDonut');
    donut.innerHTML = '';
    const svg = document.createElementNS(SVG_NS, 'svg');
    svg.setAttribute('viewBox', '0 0 220 220');
    svg.setAttribute('role', 'img');
    svg.setAttribute('aria-label', 'Частка гравця від усіх контрактів за день, тиждень і місяць');
    const vals = [];
    RINGS.forEach(r => {
      const C = 2 * Math.PI * r.r;
      const track = document.createElementNS(SVG_NS, 'circle');
      track.setAttribute('class', 'track');
      track.setAttribute('cx', 110); track.setAttribute('cy', 110); track.setAttribute('r', r.r);
      track.setAttribute('stroke-width', 14);
      const val = document.createElementNS(SVG_NS, 'circle');
      val.setAttribute('class', 'val');
      val.setAttribute('cx', 110); val.setAttribute('cy', 110); val.setAttribute('r', r.r);
      val.setAttribute('stroke-width', 14);
      val.setAttribute('stroke', r.color);
      val.setAttribute('stroke-dasharray', C);
      val.style.strokeDashoffset = C;
      svg.append(track, val);
      const share = d[r.key].all > 0 ? d[r.key].me / d[r.key].all : 0;
      vals.push({ val, C, share });
    });
    donut.appendChild(svg);
    const center = document.createElement('div');
    center.className = 'donut-center';
    center.innerHTML = '<b></b><span>загалом</span>';
    center.querySelector('b').textContent = d.total;
    donut.appendChild(center);

    // легенда
    const legend = document.getElementById('mLegend');
    legend.innerHTML = '';
    RINGS.forEach((r, i) => {
      const li = document.createElement('li');
      li.style.animationDelay = (0.12 + i * 0.1) + 's';
      li.innerHTML = '<span class="dot"></span><span class="lbl"></span><span class="num"><span class="n"></span><span class="pct"></span></span>';
      li.querySelector('.dot').style.background = r.color;
      li.querySelector('.dot').style.color = r.color;
      li.querySelector('.lbl').textContent = r.label;
      li.querySelector('.n').textContent = d[r.key].me + ' ' + plural(d[r.key].me);
      li.querySelector('.pct').textContent = d[r.key].all > 0
        ? pct(d[r.key].me, d[r.key].all) + '% від усіх'
        : 'ще немає даних';
      legend.appendChild(li);
    });

    // зароблено
    const share = document.getElementById('mShare');
    share.textContent = d.teamEarned > 0 ? pct(d.earned, d.teamEarned) + '% від усіх виплат' : '';
    const earnEl = document.getElementById('mEarn');
    earnEl.textContent = fmt(0);

    modal.hidden = false;
    document.body.style.overflow = 'hidden';
    mClose.focus();

    // запуск анімацій після появи
    requestAnimationFrame(() => requestAnimationFrame(() => {
      vals.forEach(v => { v.val.style.strokeDashoffset = v.C * (1 - v.share); });
      countUp(earnEl, d.earned, 900);
    }));
  }

  function closeModal() {
    if (modal.hidden) return;
    cancelAnimationFrame(countRaf);
    modal.hidden = true;
    document.body.style.overflow = '';
    if (lastTrigger) lastTrigger.focus();
  }
  mClose.addEventListener('click', closeModal);
  modal.addEventListener('click', e => { if (e.target === modal) closeModal(); });
  document.addEventListener('keydown', e => { if (e.key === 'Escape') closeModal(); });

  const reportRefreshBtn = document.getElementById('reportRefreshBtn');
  let resetTimer = null;
  function resetArm(armed) {
    reportRefreshBtn.classList.toggle('armed', armed);
    reportRefreshBtn.querySelector('span').textContent = armed ? 'Точно обнулити?' : 'Оновити';
  }
  reportRefreshBtn.addEventListener('click', () => {
    if (!reportRefreshBtn.classList.contains('armed')) {
      resetArm(true);
      clearTimeout(resetTimer);
      resetTimer = setTimeout(() => resetArm(false), 3000);
      return;
    }
    clearTimeout(resetTimer);
    resetArm(false);
    log = [];
    saveLog();
    recalculateTotals();
    renderStats();
    renderReport();
    renderHistory();
    renderTop();
    renderFinance();
  });

  loadLog().then(l => { log = Array.isArray(l) ? l : []; recalculateTotals(); renderReport(); renderStats(); renderHistory(); renderTop(); renderFinance(); });

  renderStats();
  renderReport();
  update();


  /* ---------- Анімації ---------- */
  const REDUCED = matchMedia('(prefers-reduced-motion: reduce)').matches;
  const crownUrl = c => "url(\"data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath fill='" + c + "' d='M3 8l4.500 4L12 5l4.500 7L21 8l-2 11H5zM5 21h14v-1H5z'/%3E%3C/svg%3E\")";
  const GOLD_C = crownUrl('%23e8b84a'), RED_C = crownUrl('%23e9453b');

  (function floatingCrowns() {
    if (REDUCED) return;
    const bg = document.createElement('div');
    bg.className = 'crowns-bg';
    for (let i = 0; i < 16; i++) {
      const sp = document.createElement('span');
      const st = sp.style;
      st.setProperty('--x', (Math.random() * 100).toFixed(1) + '%');
      st.setProperty('--s', (18 + Math.random() * 30).toFixed(0) + 'px');
      st.setProperty('--d', (20 + Math.random() * 20).toFixed(1) + 's');
      st.setProperty('--dl', (-Math.random() * 40).toFixed(1) + 's');
      st.setProperty('--o', (0.10 + Math.random() * 0.14).toFixed(2));
      st.setProperty('--sway', ((Math.random() - .5) * 120).toFixed(0) + 'px');
      st.setProperty('--c', Math.random() < .6 ? RED_C : GOLD_C);
      bg.appendChild(sp);
    }
    document.body.prepend(bg);
  })();

  function burst(x, y) {
    if (REDUCED || !document.body.animate) return;
    const shock = document.createElement('div');
    shock.className = 'shock';
    document.body.appendChild(shock);
    shock.animate([
      { transform: `translate(${x - 10}px, ${y - 10}px) scale(1)`, opacity: .9 },
      { transform: `translate(${x - 10}px, ${y - 10}px) scale(9)`, opacity: 0 }
    ], { duration: 700, easing: 'cubic-bezier(.2,.7,.3,1)' }).onfinish = () => shock.remove();

    for (let i = 0; i < 26; i++) {
      const el = document.createElement('div');
      const crown = i % 3 === 0;
      el.className = 'spark ' + (crown ? 'crown' : 'coin');
      if (crown) el.style.setProperty('--c', Math.random() < .5 ? RED_C : GOLD_C);
      document.body.appendChild(el);
      const ang = Math.random() * Math.PI * 2;
      const dist = 70 + Math.random() * 130;
      const dx = Math.cos(ang) * dist, dy = Math.sin(ang) * dist;
      const rot = (Math.random() - .5) * 720;
      el.animate([
        { transform: `translate(${x}px, ${y}px) rotate(0deg) scale(1)`, opacity: 1 },
        { transform: `translate(${x + dx}px, ${y + dy - 30}px) rotate(${rot / 2}deg) scale(1.1)`, opacity: 1, offset: .55 },
        { transform: `translate(${x + dx * 1.15}px, ${y + dy + 90}px) rotate(${rot}deg) scale(.4)`, opacity: 0 }
      ], { duration: 900 + Math.random() * 600, easing: 'cubic-bezier(.15,.7,.3,1)' }).onfinish = () => el.remove();
    }
  }

  const enterTimers = new Map();
  function playEnter(sec) {
    if (REDUCED) return;
    sec.classList.remove('enter');
    void sec.offsetWidth;
    sec.querySelectorAll('tbody tr').forEach((tr, i) => tr.style.setProperty('--i', i));
    sec.classList.add('enter');
    clearTimeout(enterTimers.get(sec));
    enterTimers.set(sec, setTimeout(() => sec.classList.remove('enter'), 1600));
  }

  /* ---------- Фон «Лос-Анджелес» ---------- */
  (function buildLA() {
    const la = document.getElementById('la');
    if (!la) return;
    const rng = seed => () => {
      seed |= 0; seed = seed + 0x6D2B79F5 | 0;
      let t = Math.imul(seed ^ seed >>> 15, 1 | seed);
      t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t;
      return ((t ^ t >>> 14) >>> 0) / 4294967296;
    };
    const url = (w, h, inner) => 'url("data:image/svg+xml,' + encodeURIComponent(
      '<svg xmlns="http://www.w3.org/2000/svg" width="' + w + '" height="' + h + '" viewBox="0 0 ' + w + ' ' + h + '">' + inner + '</svg>') + '")';
    const WIN = ['#ffd27a', '#ffb45a', '#ffe9b0', '#9fd4ff'];

    function city(W, H, seed, minH, maxH, landmarks, c1, c2, rim) {
      const r = rng(seed);
      const body = [], rims = [], wa = [], wb = [], beacons = [];
      const windows = (x, w, h) => {
        for (let yy = H - h + 8; yy < H - 6; yy += 9) {
          for (let xx = x + 4; xx < x + w - 5; xx += 7) {
            const q = r();
            if (q < .22) {
              const cell = '<rect x="' + xx + '" y="' + yy + '" width="2.500" height="3.500" fill="' + WIN[Math.floor(r() * WIN.length)] + '"/>';
              (q < .11 ? wa : wb).push(cell);
            }
          }
        }
      };
      const box = (x, w, h, noWin) => {
        body.push('<rect x="' + x + '" y="' + (H - h) + '" width="' + w + '" height="' + h + '"/>');
        rims.push('<rect x="' + x + '" y="' + (H - h) + '" width="' + w + '" height="1.500" fill="' + rim + '"/>');
        if (!noWin) windows(x, w, h);
      };
      const beacon = (x, y) => beacons.push('<circle cx="' + x + '" cy="' + y + '" r="2" fill="#ff3b30"/>');
      const fill = (x0, x1) => {
        let x = x0;
        while (x1 - x > 26) {
          let w = Math.min(24 + r() * 44, x1 - x);
          if (x1 - x - w < 22) w = x1 - x;
          const h = minH + r() * (maxH - minH);
          box(x, w, h);
          if (r() < .3) box(x + w * .2, w * .6, h + 8 + r() * 12, true);
          if (r() < .25) { body.push('<rect x="' + (x + w / 2 - 1) + '" y="' + (H - h - 14) + '" width="2" height="14"/>'); if (r() < .5) beacon(x + w / 2, H - h - 14); }
          x += w;
        }
      };
      let cursor = 0;
      landmarks.slice().sort((a, b) => a.x - b.x).forEach(lm => {
        fill(cursor, lm.x);
        lm.draw(lm.x, { box, body, beacon, H, rims });
        cursor = lm.x + lm.w;
      });
      fill(cursor, W);
      const defs = '<defs><linearGradient id="g" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="' + c1 + '"/><stop offset="1" stop-color="' + c2 + '"/></linearGradient></defs>';
      return {
        b:  url(W, H, defs + '<g fill="url(#g)">' + body.join('') + '</g>' + rims.join('')),
        wa: url(W, H, wa.join('') + beacons.join('')),
        wb: url(W, H, wb.join(''))
      };
    }

    const landmarks = [
      { x: 260, w: 58, draw(x, k) { k.box(x, 58, 188); k.box(x + 8, 42, 204, true); k.body.push('<rect x="' + (x + 16) + '" y="' + (k.H - 218) + '" width="26" height="218" rx="10"/>'); k.beacon(x + 29, k.H - 219); } },
      { x: 520, w: 92, draw(x, k) { [[0, 104], [32, 130], [64, 112]].forEach(([dx, h]) => { k.body.push('<rect x="' + (x + dx) + '" y="' + (k.H - h) + '" width="28" height="' + h + '" rx="11"/>'); }); k.beacon(x + 46, k.H - 131); } },
      { x: 760, w: 52, draw(x, k) { k.box(x, 52, 176); k.box(x + 6, 40, 186, true); k.body.push('<rect x="' + (x + 24.500) + '" y="' + (k.H - 226) + '" width="3" height="40"/>'); k.beacon(x + 26, k.H - 227); } }
    ];

    function hills(W, H) {
      let d = 'M0 ' + H;
      for (let x = 0; x <= W; x += 10) {
        const k = 2 * Math.PI * x / W;
        const y = H * (0.50 + 0.17 * Math.sin(k) + 0.09 * Math.sin(3 * k + 1) + 0.04 * Math.sin(7 * k));
        d += ' L' + x + ' ' + y.toFixed(1);
      }
      d += ' L' + W + ' ' + H + ' Z';
      return url(W, H, '<defs><linearGradient id="h" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#3a1f3f"/><stop offset="1" stop-color="#1b1030"/></linearGradient></defs><path d="' + d + '" fill="url(#h)"/>');
    }

    function palms(W, H, seed) {
      const r = rng(seed); let out = '';
      const xs = [70, 250, 430, 610, 760];
      xs.forEach(px => {
        const bx = px + (r() - .5) * 40, h = 105 + r() * 70, lean = (r() - .5) * 26;
        const tx = bx + lean, ty = H - h, cx = bx + lean * .2 - (r() * 8);
        const L = [], R = [];
        for (let i = 0; i <= 10; i++) {
          const t = i / 10, mt = 1 - t;
          const x = mt * mt * bx + 2 * mt * t * cx + t * t * tx, y = mt * mt * H + 2 * mt * t * (H - h * .5) + t * t * ty;
          const hw = 3.600 - 2.000 * t;
          L.push((x - hw).toFixed(1) + ',' + y.toFixed(1)); R.unshift((x + hw).toFixed(1) + ',' + y.toFixed(1));
        }
        out += '<polygon points="' + L.concat(R).join(' ') + '" fill="#040508"/>';
        const n = 11, len = 44 + r() * 14;
        for (let i = 0; i < n; i++) {
          const a = Math.PI + (i + .5) * Math.PI / n, dx = Math.cos(a), dy = Math.sin(a);
          const ex = tx + dx * len, ey = ty + dy * len * .45 + len * .32;
          out += '<path d="M' + tx + ' ' + ty + ' Q' + (tx + dx * len * .55) + ' ' + (ty + dy * len * .85 - 4) + ' ' + ex.toFixed(1) + ' ' + ey.toFixed(1) + '" stroke="#040508" stroke-width="3.200" stroke-linecap="round" fill="none"/>';
        }
        out += '<circle cx="' + tx + '" cy="' + (ty + 3) + '" r="4" fill="#040508"/>';
      });
      return url(W, H, out);
    }

    function dots(W, count, seed, color) {
      const r = rng(seed); let out = '';
      for (let i = 0; i < count; i++) {
        const x = r() * W, y = 1 + r() * 4, w = 3 + r() * 3;
        out += '<rect x="' + x.toFixed(1) + '" y="' + y.toFixed(1) + '" width="' + w.toFixed(1) + '" height="2" rx="1" fill="' + color + '"/>' +
               '<rect x="' + (x - 2).toFixed(1) + '" y="' + (y - 1).toFixed(1) + '" width="' + (w + 4).toFixed(1) + '" height="4" rx="2" fill="' + color + '" opacity=".25"/>';
      }
      return url(W, 8, out);
    }

    la.innerHTML = '<div class="la-sky"></div><div class="moon"></div><div class="sl sl1"></div><div class="sl sl2"></div>';
    const layer = (tile, h, t, bgs, extra, rev) => {
      const d = document.createElement('div');
      d.className = 'layer' + (rev ? ' rev' : '');
      d.style.cssText = '--tile:' + tile + 'px;--h:' + h + 'px;--t:' + t + 's;' + (extra || '');
      bgs.forEach(([cls, bg]) => { const i = document.createElement('i'); if (cls) i.className = cls; i.style.backgroundImage = bg; d.appendChild(i); });
      la.appendChild(d);
    };

    layer(1600, 150, 240, [['', hills(1600, 150)]]);
    const far = city(1000, 130, 7, 40, 118, [], '#3a2250', '#1d1233', 'rgba(255,150,90,.5)');
    layer(1000, 130, 150, [['', far.b], ['wa', far.wa], ['wb', far.wb]]);
    const near = city(1200, 230, 21, 70, 170, landmarks, '#14142a', '#07080f', 'rgba(255,140,70,.4)');
    layer(1200, 230, 85, [['', near.b], ['wa', near.wa], ['wb', near.wb]]);
    layer(600, 8, 7, [['', dots(600, 14, 3, '#fff6dd')]]);
    layer(520, 8, 10, [['', dots(520, 12, 5, '#ff4a3a')]], '', true);

    // придорожні ліхтарі та дорожній знак — рухаються разом із дорогою
    function roadside(W, H) {
      let o = '<defs><radialGradient id="lg"><stop offset="0" stop-color="#ffcf7a" stop-opacity=".95"/><stop offset=".35" stop-color="#ff9d3a" stop-opacity=".35"/><stop offset="1" stop-color="#ff9d3a" stop-opacity="0"/></radialGradient></defs>';
      [260, 900, 1540].forEach(x => {
        o += '<rect x="' + x + '" y="' + (H - 150) + '" width="5" height="150" fill="#06070b"/>' +
             '<path d="M' + (x + 2.500) + ' ' + (H - 148) + ' Q' + (x + 2.500) + ' ' + (H - 160) + ' ' + (x - 26) + ' ' + (H - 158) + '" stroke="#06070b" stroke-width="4" fill="none"/>' +
             '<circle cx="' + (x - 26) + '" cy="' + (H - 154) + '" r="46" fill="url(#lg)"/>' +
             '<ellipse cx="' + (x - 26) + '" cy="' + (H - 155) + '" rx="9" ry="3.200" fill="#fff3c8"/>';
      });
      const sx = 1180;
      o += '<rect x="' + (sx + 20) + '" y="' + (H - 120) + '" width="6" height="120" fill="#06070b"/><rect x="' + (sx + 154) + '" y="' + (H - 120) + '" width="6" height="120" fill="#06070b"/>' +
           '<rect x="' + sx + '" y="' + (H - 188) + '" width="180" height="76" rx="6" fill="#0c6b3b" stroke="#fff" stroke-width="3"/>' +
           '<text x="' + (sx + 90) + '" y="' + (H - 154) + '" text-anchor="middle" font-family="Arial, sans-serif" font-weight="700" font-size="21" fill="#fff" letter-spacing="1">LOS ANGELES</text>' +
           '<text x="' + (sx + 90) + '" y="' + (H - 126) + '" text-anchor="middle" font-family="Arial, sans-serif" font-weight="700" font-size="17" fill="#fff">5 mi</text>';
      return url(W, H, o);
    }
    layer(2130, 210, 10, [['', roadside(2130, 210)]]);
    layer(840, 190, 3.94, [['', palms(840, 190, 11)]]);

    la.insertAdjacentHTML('beforeend',
      '<div class="plane"><i></i></div><div class="plane p2"><i></i></div>' +
      '<div class="heli"><div class="beam"></div>' +
      '<svg viewBox="0 0 90 44" aria-hidden="true"><rect class="rotor" x="8" y="9" width="64" height="2.500" rx="1.200" fill="#0b0c12"/>' +
      '<rect x="38" y="11" width="3" height="8" fill="#0b0c12"/><ellipse cx="42" cy="26" rx="20" ry="9" fill="#0b0c12"/>' +
      '<path d="M54 20 Q66 22 63 31 Q57 34 52 32Z" fill="#3a5a72"/><path d="M4 22 L24 21 L24 28 L4 26Z" fill="#0b0c12"/><rect x="2" y="16" width="2.500" height="12" fill="#0b0c12"/>' +
      '<path d="M24 38 H62 M30 33 V38 M54 33 V38" stroke="#0b0c12" stroke-width="2" fill="none"/>' +
      '<circle class="nav" cx="26" cy="26" r="2" fill="#ff3b30"/><circle cx="50" cy="35" r="2.200" fill="#fffbe0"/></svg></div>');
  })();


  /* ---------- Додаткові модулі GOLD ---------- */
  const EXTRA_KEYS = ['history','top','finance','roster','settings'];
  let currentExtra = null;
  const extraSections = Object.fromEntries(EXTRA_KEYS.map(k => [k, document.getElementById(k)]));
  document.querySelectorAll('.extra-tab').forEach(btn => btn.addEventListener('click', () => {
    const key = btn.dataset.tab;
    currentExtra = currentExtra === key ? null : key;
    current = null;
    Object.values(TABS).forEach(t => { t.sec.hidden = true; t.btn.classList.remove('active'); t.btn.setAttribute('aria-expanded','false'); });
    EXTRA_KEYS.forEach(k => { extraSections[k].hidden = k !== currentExtra; });
    document.querySelectorAll('.extra-tab').forEach(b => b.classList.toggle('active', b.dataset.tab === currentExtra));
    empty.hidden = currentExtra !== null;
    if (currentExtra) {
      renderExtra(currentExtra);
      extraSections[currentExtra].scrollIntoView({behavior:'smooth',block:'start'});
    }
  }));

  const PAID_KEY = 'gold-paid-state';
  try { const savedPaid = JSON.parse(localStorage.getItem(PAID_KEY) || '{}'); Object.keys(savedPaid).forEach(k => { if (k in paid) paid[k] = !!savedPaid[k]; }); } catch(e) {}
  function persistPaid(){ try { localStorage.setItem(PAID_KEY, JSON.stringify(paid)); } catch(e) {} }
  function recalculateTotals(){
    NAMES.forEach(n => totals[n] = 0);
    log.forEach(e => { if (e && e.n in totals) totals[e.n] = Math.round((totals[e.n] + Number(e.a || 0))*100)/100; });
  }
  function saveRoster(){ try { localStorage.setItem('gold-roster', JSON.stringify(NAMES)); } catch(e) {} }
  function rebuildPeopleOptions(){
    const selected = [...ddList.querySelectorAll('input:checked')].map(x=>x.value);
    ddList.innerHTML = '';
    NAMES.forEach(n => {
      const label = document.createElement('label'); label.className='opt';
      label.innerHTML='<input type="checkbox"><span class="box"><svg viewBox="0 0 24 24"><path d="M5 12l5 5 9-10"/></svg></span><span class="opt-name"></span>';
      const input=label.querySelector('input'); input.value=n; input.checked=selected.includes(n);
      label.querySelector('.opt-name').textContent=n;
      input.addEventListener('change',update); ddList.appendChild(label);
    });
    checks=ddList.querySelectorAll('input[type="checkbox"]'); update();
  }
  // Existing payment toggles are persisted across refreshes.
  const originalRenderStats = renderStats;
  renderStats = function(){ originalRenderStats(); };
  function renderExtra(key){
    if(key==='history') renderHistory();
    if(key==='top') renderTop();
    if(key==='finance') renderFinance();
    if(key==='roster') renderRoster();
    if(key==='settings') loadSettingsForm();
  }
  function renderHistory(){
    const body=document.getElementById('historyBody'), q=document.getElementById('historySearch').value.trim().toLowerCase();
    body.innerHTML='';
    const items=log.map((e,i)=>({...e,_i:i})).filter(e=>(e.n||'').toLowerCase().includes(q)).sort((a,b)=>(b.t||0)-(a.t||0));
    document.getElementById('historyCount').textContent=`Записів: ${items.length}`;
    document.getElementById('historyEmpty').hidden=items.length>0;
    items.forEach(e=>{
      const tr=document.createElement('tr');
      const date=new Date(e.t||Date.now()).toLocaleString('uk-UA');
      const cells=[date,e.n||'—',fmt(e.a||0)];
      cells.forEach(v=>{const td=document.createElement('td');td.textContent=v;tr.appendChild(td);});
      const td=document.createElement('td');const b=document.createElement('button');b.className='action-btn danger';b.textContent='Скасувати';b.addEventListener('click',()=>{
        if(!confirm(`Скасувати нарахування ${fmt(e.a||0)} для ${e.n}?`)) return;
        log.splice(e._i,1); saveLog(); recalculateTotals(); renderStats(); renderReport(); renderHistory(); renderTop(); renderFinance();
      });td.appendChild(b);tr.appendChild(td);body.appendChild(tr);
    });
  }
  document.getElementById('historySearch').addEventListener('input',renderHistory);
  document.getElementById('historyExport').addEventListener('click',()=>{
    const rows=[['Дата','Гравець','Виплата'],...log.map(e=>[new Date(e.t).toLocaleString('uk-UA'),e.n,e.a])];
    downloadFile('gold-history.csv','\ufeff'+rows.map(r=>r.map(v=>'"'+String(v??'').replaceAll('"','""')+'"').join(';')).join('\n'),'text/csv;charset=utf-8');
  });
  function getPeriodLog(period){
    if(period==='all') return log;
    const now=new Date(), start=new Date(now.getFullYear(),now.getMonth(),now.getDate());
    if(period==='week') start.setDate(start.getDate()-((start.getDay()+6)%7)); else start.setDate(1);
    return log.filter(e=>e.t>=+start);
  }
  function renderTop(){
    const period=document.getElementById('topPeriod').value, entries=getPeriodLog(period), stats=NAMES.map(n=>({n,count:entries.filter(e=>e.n===n).length,earned:entries.filter(e=>e.n===n).reduce((s,e)=>s+Number(e.a||0),0)})).sort((a,b)=>b.count-a.count||b.earned-a.earned);
    const cards=document.getElementById('topCards'); cards.innerHTML='';
    const labels=['🥇 Перше місце','🥈 Друге місце','🥉 Третє місце'];
    stats.slice(0,3).forEach((p,i)=>{const card=document.createElement('article');card.className='top-card';const span=document.createElement('span');span.textContent=labels[i];const strong=document.createElement('strong');strong.textContent=p.n;const small=document.createElement('small');small.textContent=`${p.count} контрактів · ${fmt(p.earned)}`;card.append(span,strong,small);cards.appendChild(card);});
    const body=document.getElementById('topBody');body.innerHTML='';stats.forEach((p,i)=>{const tr=document.createElement('tr');[i+1,p.n,p.count,fmt(p.earned)].forEach(v=>{const td=document.createElement('td');td.textContent=v;tr.appendChild(td);});body.appendChild(tr);});
  }
  document.getElementById('topPeriod').addEventListener('change',renderTop);
  function renderFinance(){
    const payout=log.reduce((s,e)=>s+Number(e.a||0),0), gross=SHARE_LEFT>0?payout/SHARE_LEFT:payout, cut=gross-payout;
    document.getElementById('grossTotal').textContent=fmt(gross);
    document.getElementById('paidTotal').textContent=fmt(payout);
    document.getElementById('teamCut').textContent=fmt(cut);
    document.getElementById('contractTotal').textContent=log.length;
    document.getElementById('teamCutHint').textContent=`${Math.round((1-SHARE_LEFT)*100)}% від розрахункової суми`;
  }
  document.getElementById('financeRefresh').addEventListener('click',renderFinance);
  function renderRoster(){
    const body=document.getElementById('rosterBody');body.innerHTML='';
    document.getElementById('rosterCount').textContent=`${NAMES.length} гравців`;
    NAMES.forEach((name,index)=>{
      const tr=document.createElement('tr'), tdName=document.createElement('td'), tdAct=document.createElement('td');tdName.textContent=name;
      const edit=document.createElement('button');edit.className='action-btn';edit.textContent='Змінити';edit.addEventListener('click',()=>{
        const next=prompt('Новий нікнейм або ID:',name);if(next===null)return;const clean=next.trim();if(!clean||NAMES.some((n,i)=>i!==index&&n.toLowerCase()===clean.toLowerCase())){alert('Вкажіть унікальний непорожній нікнейм.');return;}
        NAMES[index]=clean; if(name in totals){totals[clean]=totals[name];delete totals[name];} if(name in paid){paid[clean]=paid[name];delete paid[name];}
        log.forEach(e=>{if(e.n===name)e.n=clean;});saveLog();saveRoster();persistPaid();rebuildPeopleOptions();renderStats();renderReport();renderRoster();
      });
      const del=document.createElement('button');del.className='action-btn danger';del.textContent='Видалити зі складу';del.style.marginLeft='7px';del.addEventListener('click',()=>{
        if(!confirm(`Прибрати ${name} зі складу? Історичні операції залишаться.`))return;
        NAMES.splice(index,1);delete totals[name];delete paid[name];saveRoster();persistPaid();rebuildPeopleOptions();renderStats();renderReport();renderRoster();
      });tdAct.append(edit,del);tr.append(tdName,tdAct);body.appendChild(tr);
    });
  }
  document.getElementById('addPlayerForm').addEventListener('submit',e=>{
    e.preventDefault();const input=document.getElementById('newPlayerName'),name=input.value.trim();if(!name)return;
    if(NAMES.some(n=>n.toLowerCase()===name.toLowerCase())){alert('Такий гравець уже є у складі.');return;}
    if(NAMES.length>=200){alert('До складу можна додати не більше 200 гравців.');return;}
    NAMES.push(name);totals[name]=0;paid[name]=false;saveRoster();persistPaid();input.value='';rebuildPeopleOptions();renderStats();renderReport();renderRoster();
  });
  function loadSettingsForm(){document.getElementById('sharePercent').value=Math.round((1-SHARE_LEFT)*100);document.getElementById('currencySelect').value=CURRENCY;}
  document.getElementById('saveSettings').addEventListener('click',()=>{
    const pct=Number(document.getElementById('sharePercent').value),currency=document.getElementById('currencySelect').value;
    if(!Number.isFinite(pct)||pct<0||pct>99){alert('Відрахування має бути від 0 до 99%.');return;}
    SHARE_LEFT=1-pct/100;CURRENCY=currency;
    try{localStorage.setItem('gold-settings',JSON.stringify({sharePercent:pct,currency}));}catch(e){}
    document.getElementById('settingsStatus').textContent='Налаштування збережено.';
    renderStats();renderHistory();renderTop();renderFinance();update();
  });
  function downloadFile(name,content,type){const blob=new Blob([content],{type}),url=URL.createObjectURL(blob),a=document.createElement('a');a.href=url;a.download=name;document.body.appendChild(a);a.click();a.remove();URL.revokeObjectURL(url);}
  document.getElementById('backupData').addEventListener('click',()=>{
    const data={version:1,createdAt:new Date().toISOString(),roster:NAMES,log,paid,settings:{sharePercent:Math.round((1-SHARE_LEFT)*100),currency:CURRENCY}};
    downloadFile('GOLD-backup.json',JSON.stringify(data,null,2),'application/json');
  });
  document.getElementById('restoreData').addEventListener('change',async e=>{
    const file=e.target.files[0];if(!file)return;
    try{
      const data=JSON.parse(await file.text());if(!data||!Array.isArray(data.roster)||!Array.isArray(data.log))throw new Error('invalid');
      if(!confirm('Відновити резервну копію? Поточні дані будуть замінені.'))return;
      NAMES.splice(0,NAMES.length,...data.roster.filter(x=>typeof x==='string'&&x.trim()).slice(0,200));
      log=data.log.filter(x=>x&&typeof x.n==='string'&&Number.isFinite(Number(x.a))&&Number.isFinite(Number(x.t)));
      Object.keys(paid).forEach(k=>delete paid[k]);Object.assign(paid,data.paid||{});
      if(data.settings){SHARE_LEFT=1-Math.min(99,Math.max(0,Number(data.settings.sharePercent)||0))/100;CURRENCY=['$','₴','€'].includes(data.settings.currency)?data.settings.currency:'$';localStorage.setItem('gold-settings',JSON.stringify(data.settings));}
      saveRoster();saveLog();persistPaid();recalculateTotals();rebuildPeopleOptions();renderStats();renderReport();renderExtra('roster');renderHistory();renderTop();renderFinance();
      document.getElementById('settingsStatus').textContent='Резервну копію відновлено.';
    }catch(err){alert('Не вдалося прочитати резервну копію GOLD.');}finally{e.target.value='';}
  });

  // Update related panels immediately after a new calculation.
  const calcButton = calcBtn;
  calcButton.addEventListener('click',()=>{setTimeout(()=>{recalculateTotals();persistPaid();renderHistory();renderTop();renderFinance();},0);});
  // Save payment checkboxes whenever the table is redrawn or changed.
  const statsBodyObserver = new MutationObserver(()=>{
    statsBody.querySelectorAll('input[type="checkbox"]').forEach(cb=>{if(!cb.dataset.persistBound){cb.dataset.persistBound='1';cb.addEventListener('change',persistPaid);}});
  });
  statsBodyObserver.observe(statsBody,{childList:true,subtree:true});
  recalculateTotals();
(function navWheel() {
  const navEl = document.querySelector('.sidebar nav');
  if (!navEl) return;
  navEl.addEventListener('wheel', e => {
    // якщо всі кнопки вміщуються, нічого не робимо
    if (navEl.scrollWidth <= navEl.clientWidth) return;
    // горизонтальні жести тачпада лишаємо як є
    if (Math.abs(e.deltaX) > Math.abs(e.deltaY)) return;

    const atStart = navEl.scrollLeft <= 0 && e.deltaY < 0;
    const atEnd = navEl.scrollLeft + navEl.clientWidth >= navEl.scrollWidth - 1 && e.deltaY > 0;
    if (atStart || atEnd) return; // на краях сторінка скролиться як звичайно

    e.preventDefault();
    navEl.scrollLeft += e.deltaY;
  }, { passive: false });
})();
  
</script>
</body>
</html>
