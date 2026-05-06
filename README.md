
<!DOCTYPE html>

<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BTC DCA Tracker Pro</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=DM+Sans:opsz,wght@9..40,300;9..40,400;9..40,500;9..40,600&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0a0a0f;
  --bg2: #111118;
  --bg3: #1a1a24;
  --border: rgba(255,255,255,0.06);
  --border2: rgba(255,255,255,0.12);
  --text: #f0f0f8;
  --muted: #6b6b8a;
  --orange: #f7931a;
  --orange2: rgba(247,147,26,0.15);
  --green: #00d68f;
  --green2: rgba(0,214,143,0.12);
  --red: #ff4d6d;
  --red2: rgba(255,77,109,0.12);
  --blue: #4d9fff;
  --font-mono: 'Space Mono', monospace;
  --font-body: 'DM Sans', sans-serif;
}
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--font-body);
  font-size: 14px;
  line-height: 1.6;
  min-height: 100vh;
}

::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: var(–bg); }
::-webkit-scrollbar-thumb { background: var(–border2); border-radius: 2px; }

.topbar {
position: sticky; top: 0; z-index: 100;
display: flex; align-items: center; justify-content: space-between;
padding: 0 24px;
height: 56px;
background: rgba(10,10,15,0.92);
backdrop-filter: blur(12px);
border-bottom: 1px solid var(–border);
}
.topbar-brand { display: flex; align-items: center; gap: 10px; }
.brand-icon {
width: 32px; height: 32px; border-radius: 8px;
background: var(–orange);
display: flex; align-items: center; justify-content: center;
font-family: var(–font-mono); font-size: 14px; font-weight: 700; color: #000;
}
.brand-name { font-weight: 600; font-size: 15px; }
.brand-sub { font-size: 11px; color: var(–muted); margin-left: 6px; }
.topbar-right { display: flex; align-items: center; gap: 12px; }
.sync-badge {
display: flex; align-items: center; gap: 6px;
padding: 4px 10px; border-radius: 20px;
border: 1px solid var(–border2);
font-family: var(–font-mono); font-size: 11px; color: var(–muted);
}
.sync-dot { width: 6px; height: 6px; border-radius: 50%; background: var(–green); animation: pulse 2s infinite; }
@keyframes pulse { 0%,100%{opacity:1}50%{opacity:.3} }

.main { max-width: 1280px; margin: 0 auto; padding: 24px 24px 60px; }

.section { margin-bottom: 28px; }
.section-head {
display: flex; align-items: center; justify-content: space-between;
margin-bottom: 16px;
}
.section-title {
display: flex; align-items: center; gap: 8px;
font-size: 13px; font-weight: 600; letter-spacing: .06em; text-transform: uppercase; color: var(–muted);
}
.section-num {
width: 20px; height: 20px; border-radius: 4px;
background: var(–bg3); border: 1px solid var(–border2);
display: flex; align-items: center; justify-content: center;
font-family: var(–font-mono); font-size: 10px; color: var(–orange);
}

.overview-grid { display: grid; grid-template-columns: 300px 1fr; gap: 16px; }

.card {
background: var(–bg2);
border: 1px solid var(–border);
border-radius: 12px;
padding: 20px;
}
.card-sm { padding: 16px; }

.pnl-label { font-size: 10px; letter-spacing: .1em; text-transform: uppercase; color: var(–muted); margin-bottom: 6px; }
.pnl-big {
font-family: var(–font-mono); font-size: 28px; font-weight: 700;
letter-spacing: -.5px; line-height: 1;
}
.pnl-big.pos { color: var(–green); }
.pnl-big.neg { color: var(–red); }
.pnl-badge {
display: inline-flex; align-items: center; gap: 5px;
margin-top: 8px; padding: 3px 8px; border-radius: 20px;
font-family: var(–font-mono); font-size: 11px;
}
.pnl-badge.pos { background: var(–green2); color: var(–green); }
.pnl-badge.neg { background: var(–red2); color: var(–red); }
.pnl-divider { margin: 16px 0; border: none; border-top: 1px solid var(–border); }
.pnl-row { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
.pnl-item-label { font-size: 10px; color: var(–muted); letter-spacing: .06em; text-transform: uppercase; }
.pnl-item-val { font-family: var(–font-mono); font-size: 15px; font-weight: 700; margin-top: 3px; }
.pnl-item-sub { font-size: 11px; color: var(–muted); margin-top: 2px; }

.chart-tabs { display: flex; gap: 4px; margin-bottom: 14px; flex-wrap: wrap; }
.tab-btn {
padding: 5px 12px; border-radius: 6px; border: 1px solid var(–border);
background: transparent; color: var(–muted); font-size: 12px; font-family: var(–font-body);
cursor: pointer; transition: all .15s;
}
.tab-btn:hover { border-color: var(–border2); color: var(–text); }
.tab-btn.active { background: var(–orange); border-color: var(–orange); color: #000; font-weight: 600; }
.chart-wrap { position: relative; height: 200px; }

.metrics-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(170px, 1fr)); gap: 12px; }
.metric-card {
background: var(–bg2); border: 1px solid var(–border); border-radius: 10px;
padding: 14px 16px; position: relative; overflow: hidden;
transition: border-color .2s;
}
.metric-card:hover { border-color: var(–border2); }
.metric-card::before {
content: ‘’; position: absolute; left: 0; top: 0; bottom: 0;
width: 3px; border-radius: 3px 0 0 3px;
}
.mc-orange::before { background: var(–orange); }
.mc-green::before { background: var(–green); }
.mc-blue::before { background: var(–blue); }
.mc-red::before { background: var(–red); }
.mc-purple::before { background: #a855f7; }
.mc-teal::before { background: #2dd4bf; }
.metric-label { font-size: 10px; color: var(–muted); letter-spacing: .08em; text-transform: uppercase; }
.metric-val { font-family: var(–font-mono); font-size: 16px; font-weight: 700; margin-top: 4px; line-height: 1.2; }
.metric-val.pos { color: var(–green); }
.metric-val.neg { color: var(–red); }
.metric-sub { font-size: 11px; color: var(–muted); margin-top: 3px; }

.goal-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 12px; }
.goal-card { background: var(–bg2); border: 1px solid var(–border); border-radius: 10px; padding: 16px; }
.goal-label { font-size: 10px; color: var(–muted); letter-spacing: .08em; text-transform: uppercase; }
.goal-pct { font-family: var(–font-mono); font-size: 24px; font-weight: 700; margin-top: 4px; }
.goal-pct.orange { color: var(–orange); }
.goal-pct.blue { color: var(–blue); }
.goal-track { height: 4px; background: var(–bg3); border-radius: 2px; margin: 10px 0; overflow: hidden; }
.goal-fill { height: 100%; border-radius: 2px; transition: width 1s ease; }
.goal-fill.orange { background: var(–orange); }
.goal-fill.blue { background: var(–blue); }
.goal-meta { display: flex; justify-content: space-between; font-size: 11px; color: var(–muted); font-family: var(–font-mono); }

.action-buttons {
display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 10px;
margin-top: 16px;
}
.btn {
padding: 10px 16px; border-radius: 8px; border: none;
font-family: var(–font-body); font-size: 12px; font-weight: 600;
cursor: pointer; transition: all .15s; text-transform: uppercase;
letter-spacing: .05em;
}
.btn-primary {
background: var(–orange); color: #000;
}
.btn-primary:hover { opacity: 0.9; }
.btn-secondary {
background: transparent; border: 1px solid var(–orange); color: var(–orange);
}
.btn-secondary:hover { background: var(–orange2); }
.btn-success {
background: var(–green); color: #000;
}
.btn-success:hover { opacity: 0.9; }
.btn-danger {
background: var(–red); color: #fff;
}
.btn-danger:hover { opacity: 0.9; }
.btn-info {
background: var(–blue); color: #fff;
}
.btn-info:hover { opacity: 0.9; }

/* Modal */
.modal {
display: none; position: fixed; top: 0; left: 0;
width: 100%; height: 100%; background: rgba(0,0,0,0.8);
z-index: 1000; align-items: center; justify-content: center;
}
.modal.active { display: flex; }
.modal-content {
background: var(–bg2); border: 1px solid var(–border);
border-radius: 16px; padding: 24px; max-width: 400px; width: 90%;
max-height: 90vh; overflow-y: auto;
}
.modal-header {
font-size: 18px; font-weight: 700; margin-bottom: 16px;
color: var(–text);
}
.modal-close {
float: right; cursor: pointer; font-size: 24px; color: var(–muted);
background: none; border: none; padding: 0;
}
.form-group {
margin-bottom: 16px;
}
.form-label {
display: block; font-size: 12px; font-weight: 600;
margin-bottom: 6px; color: var(–muted); text-transform: uppercase;
}
.form-input {
width: 100%; padding: 10px 12px; background: var(–bg3);
border: 1px solid var(–border); border-radius: 8px;
color: var(–text); font-family: var(–font-body);
font-size: 13px; outline: none;
}
.form-input:focus { border-color: var(–orange); }
.modal-actions {
display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 20px;
}
.modal-actions button { padding: 10px 16px; border-radius: 8px; border: none; font-weight: 600; }

.table-controls { display: flex; align-items: center; gap: 12px; margin-bottom: 14px; }
.search-input {
flex: 1; padding: 8px 14px; background: var(–bg2); border: 1px solid var(–border2);
border-radius: 8px; color: var(–text); font-size: 13px; font-family: var(–font-body);
outline: none; transition: border-color .15s;
}
.search-input::placeholder { color: var(–muted); }
.search-input:focus { border-color: var(–orange); }
.rec-count { font-family: var(–font-mono); font-size: 12px; color: var(–muted); white-space: nowrap; }
.rows-sel {
padding: 7px 10px; background: var(–bg2); border: 1px solid var(–border2);
border-radius: 8px; color: var(–text); font-size: 12px; cursor: pointer;
}
.table-wrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; font-size: 13px; }
thead th {
padding: 10px 12px; text-align: left; font-size: 10px; font-weight: 600;
letter-spacing: .08em; text-transform: uppercase; color: var(–muted);
border-bottom: 1px solid var(–border);
cursor: pointer; user-select: none; white-space: nowrap;
}
thead th:hover { color: var(–text); }
tbody td { pa
