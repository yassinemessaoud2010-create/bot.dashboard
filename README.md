[probot-dashboard (1).html](https://github.com/user-attachments/files/26067058/probot-dashboard.1.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ProBot Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg-darkest: #0d0f13;
    --bg-dark: #16181d;
    --bg-panel: #1e2128;
    --bg-card: #252830;
    --bg-hover: #2d3039;
    --blurple: #5865F2;
    --blurple-hover: #4752c4;
    --blurple-glow: rgba(88,101,242,0.35);
    --green: #57F287;
    --green-dim: rgba(87,242,135,0.15);
    --red: #ED4245;
    --red-dim: rgba(237,66,69,0.15);
    --yellow: #FEE75C;
    --yellow-dim: rgba(254,231,92,0.15);
    --cyan: #00b4d8;
    --pink: #eb459e;
    --text-primary: #f2f3f5;
    --text-secondary: #b5bac1;
    --text-muted: #6d7180;
    --border: rgba(255,255,255,0.06);
    --border-bright: rgba(88,101,242,0.4);
    --sidebar-width: 260px;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Outfit', sans-serif;
    background: var(--bg-darkest);
    color: var(--text-primary);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ANIMATED BG */
  .bg-fx {
    position: fixed; inset: 0; pointer-events: none; z-index: 0;
    overflow: hidden;
  }
  .bg-fx::before {
    content: '';
    position: absolute;
    width: 700px; height: 700px;
    background: radial-gradient(circle, rgba(88,101,242,0.12) 0%, transparent 70%);
    top: -200px; left: -200px;
    animation: driftA 18s ease-in-out infinite alternate;
  }
  .bg-fx::after {
    content: '';
    position: absolute;
    width: 500px; height: 500px;
    background: radial-gradient(circle, rgba(235,69,158,0.08) 0%, transparent 70%);
    bottom: -150px; right: -100px;
    animation: driftB 22s ease-in-out infinite alternate;
  }
  .bg-orb3 {
    position: absolute;
    width: 400px; height: 400px;
    background: radial-gradient(circle, rgba(0,180,216,0.07) 0%, transparent 70%);
    top: 50%; right: 200px;
    animation: driftC 15s ease-in-out infinite alternate;
  }

  @keyframes driftA { 0%{transform:translate(0,0)} 100%{transform:translate(120px,80px)} }
  @keyframes driftB { 0%{transform:translate(0,0)} 100%{transform:translate(-80px,-60px)} }
  @keyframes driftC { 0%{transform:translate(0,0) scale(1)} 100%{transform:translate(-40px,60px) scale(1.2)} }

  /* PARTICLES */
  .particles { position: fixed; inset: 0; pointer-events: none; z-index: 0; }
  .particle {
    position: absolute;
    border-radius: 50%;
    background: var(--blurple);
    animation: floatUp linear infinite;
    opacity: 0;
  }

  @keyframes floatUp {
    0% { transform: translateY(100vh) scale(0); opacity: 0; }
    10% { opacity: 0.6; }
    90% { opacity: 0.2; }
    100% { transform: translateY(-100px) scale(1); opacity: 0; }
  }

  /* LAYOUT */
  .layout { display: flex; min-height: 100vh; position: relative; z-index: 1; }

  /* SIDEBAR */
  .sidebar {
    width: var(--sidebar-width);
    background: var(--bg-dark);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0; left: 0; bottom: 0;
    z-index: 100;
    overflow: hidden;
  }
  .sidebar::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0; height: 1px;
    background: linear-gradient(90deg, transparent, var(--blurple), transparent);
    animation: scanline 3s ease-in-out infinite;
  }
  @keyframes scanline { 0%,100%{opacity:0.3} 50%{opacity:1} }

  .sidebar-logo {
    padding: 24px 20px;
    display: flex; align-items: center; gap: 12px;
    border-bottom: 1px solid var(--border);
  }
  .logo-icon {
    width: 40px; height: 40px;
    background: linear-gradient(135deg, var(--blurple), var(--pink));
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px;
    box-shadow: 0 4px 20px var(--blurple-glow);
    animation: logoPulse 3s ease-in-out infinite;
    flex-shrink: 0;
  }
  @keyframes logoPulse {
    0%,100%{box-shadow:0 4px 20px var(--blurple-glow)}
    50%{box-shadow:0 4px 30px rgba(88,101,242,0.6), 0 0 60px rgba(235,69,158,0.2)}
  }
  .logo-text { font-size: 18px; font-weight: 800; letter-spacing: -0.5px; }
  .logo-text span { color: var(--blurple); }

  .server-info {
    margin: 16px;
    padding: 12px;
    background: var(--bg-card);
    border-radius: 12px;
    border: 1px solid var(--border);
    display: flex; align-items: center; gap: 10px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .server-info:hover { border-color: var(--border-bright); background: var(--bg-hover); }
  .server-avatar {
    width: 36px; height: 36px;
    background: linear-gradient(135deg, var(--blurple), var(--cyan));
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px; flex-shrink: 0;
  }
  .server-name { font-size: 13px; font-weight: 600; }
  .server-id { font-size: 11px; color: var(--text-muted); font-family: 'JetBrains Mono'; }
  .server-chevron { margin-left: auto; color: var(--text-muted); font-size: 12px; }

  .nav-section { padding: 8px 12px 4px; }
  .nav-label { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); padding: 0 8px; margin-bottom: 4px; }
  .nav-item {
    display: flex; align-items: center; gap: 10px;
    padding: 10px 12px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
    font-size: 14px;
    font-weight: 500;
    color: var(--text-secondary);
    position: relative;
    overflow: hidden;
  }
  .nav-item::before {
    content: '';
    position: absolute; left: 0; top: 50%;
    transform: translateY(-50%);
    width: 3px; height: 0;
    background: var(--blurple);
    border-radius: 0 2px 2px 0;
    transition: height 0.2s;
  }
  .nav-item:hover { background: var(--bg-hover); color: var(--text-primary); }
  .nav-item:hover::before { height: 50%; }
  .nav-item.active {
    background: rgba(88,101,242,0.15);
    color: #fff;
    border: 1px solid rgba(88,101,242,0.25);
  }
  .nav-item.active::before { height: 100%; }
  .nav-icon { font-size: 18px; width: 24px; text-align: center; flex-shrink: 0; }
  .nav-badge {
    margin-left: auto;
    background: var(--blurple);
    color: #fff;
    font-size: 10px;
    font-weight: 700;
    padding: 2px 7px;
    border-radius: 20px;
  }

  .sidebar-footer {
    margin-top: auto;
    padding: 16px;
    border-top: 1px solid var(--border);
  }
  .bot-status-pill {
    display: flex; align-items: center; gap: 8px;
    padding: 8px 12px;
    background: var(--bg-card);
    border-radius: 8px;
    border: 1px solid var(--green-dim);
  }
  .status-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
    animation: statusPulse 2s ease-in-out infinite;
  }
  @keyframes statusPulse { 0%,100%{box-shadow:0 0 8px var(--green)} 50%{box-shadow:0 0 16px var(--green), 0 0 30px rgba(87,242,135,0.3)} }
  .status-text { font-size: 12px; font-weight: 600; color: var(--green); }

  /* MAIN */
  .main { margin-left: var(--sidebar-width); flex: 1; padding: 32px; }

  /* PAGE HEADER */
  .page-header {
    margin-bottom: 32px;
    animation: fadeSlideIn 0.5s ease both;
  }
  @keyframes fadeSlideIn {
    from { opacity:0; transform:translateY(-16px); }
    to   { opacity:1; transform:translateY(0); }
  }
  .page-title { font-size: 28px; font-weight: 800; letter-spacing: -0.5px; }
  .page-title span { 
    background: linear-gradient(135deg, var(--blurple), var(--cyan), var(--pink));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .page-subtitle { color: var(--text-muted); font-size: 14px; margin-top: 4px; }

  /* STATS CARDS */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin-bottom: 28px;
    animation: fadeSlideIn 0.5s 0.1s ease both;
  }
  .stat-card {
    background: var(--bg-panel);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 20px;
    position: relative;
    overflow: hidden;
    cursor: default;
    transition: all 0.3s;
  }
  .stat-card::after {
    content: '';
    position: absolute; inset: 0;
    background: linear-gradient(135deg, rgba(88,101,242,0.04), transparent);
    opacity: 0;
    transition: opacity 0.3s;
  }
  .stat-card:hover { transform: translateY(-3px); border-color: var(--border-bright); box-shadow: 0 12px 40px rgba(88,101,242,0.15); }
  .stat-card:hover::after { opacity: 1; }
  .stat-card-bg {
    position: absolute; right: -10px; top: -10px;
    font-size: 70px; opacity: 0.04;
    pointer-events: none;
  }
  .stat-label { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.2px; color: var(--text-muted); margin-bottom: 8px; }
  .stat-value { font-size: 32px; font-weight: 800; letter-spacing: -1px; }
  .stat-value.green { color: var(--green); text-shadow: 0 0 30px rgba(87,242,135,0.4); }
  .stat-value.blurple { color: var(--blurple); text-shadow: 0 0 30px var(--blurple-glow); }
  .stat-value.cyan { color: var(--cyan); text-shadow: 0 0 30px rgba(0,180,216,0.3); }
  .stat-delta { font-size: 11px; color: var(--green); margin-top: 4px; display: flex; align-items: center; gap: 4px; }

  /* PANELS GRID */
  .panels-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }
  .panel-full { grid-column: 1 / -1; }

  /* PANEL */
  .panel {
    background: var(--bg-panel);
    border: 1px solid var(--border);
    border-radius: 18px;
    overflow: hidden;
    transition: border-color 0.3s;
    animation: fadeSlideIn 0.5s ease both;
  }
  .panel:hover { border-color: rgba(88,101,242,0.2); }
  .panel-header {
    padding: 20px 24px;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 12px;
    position: relative;
  }
  .panel-header::after {
    content: '';
    position: absolute; bottom: 0; left: 24px; right: 24px; height: 1px;
    background: linear-gradient(90deg, transparent, var(--blurple), transparent);
    opacity: 0;
    transition: opacity 0.3s;
  }
  .panel:hover .panel-header::after { opacity: 0.5; }
  .panel-icon {
    width: 38px; height: 38px;
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
    flex-shrink: 0;
  }
  .pi-blue { background: rgba(88,101,242,0.2); box-shadow: inset 0 0 0 1px rgba(88,101,242,0.3); }
  .pi-green { background: rgba(87,242,135,0.15); box-shadow: inset 0 0 0 1px rgba(87,242,135,0.25); }
  .pi-red { background: rgba(237,66,69,0.15); box-shadow: inset 0 0 0 1px rgba(237,66,69,0.25); }
  .pi-yellow { background: rgba(254,231,92,0.15); box-shadow: inset 0 0 0 1px rgba(254,231,92,0.25); }
  .pi-pink { background: rgba(235,69,158,0.15); box-shadow: inset 0 0 0 1px rgba(235,69,158,0.25); }
  .pi-cyan { background: rgba(0,180,216,0.15); box-shadow: inset 0 0 0 1px rgba(0,180,216,0.25); }
  .panel-title { font-size: 15px; font-weight: 700; }
  .panel-subtitle { font-size: 12px; color: var(--text-muted); margin-top: 1px; }
  .panel-body { padding: 24px; display: flex; flex-direction: column; gap: 20px; }

  /* TOGGLE */
  .field-row {
    display: flex; align-items: center; justify-content: space-between; gap: 12px;
  }
  .field-label { font-size: 13px; font-weight: 600; }
  .field-desc { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
  .toggle-wrap { position: relative; flex-shrink: 0; }
  .toggle-input { opacity: 0; width: 0; height: 0; position: absolute; }
  .toggle-track {
    display: block;
    width: 46px; height: 26px;
    background: var(--bg-hover);
    border: 1px solid rgba(255,255,255,0.1);
    border-radius: 13px;
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
  }
  .toggle-track::after {
    content: '';
    position: absolute;
    width: 18px; height: 18px;
    background: var(--text-muted);
    border-radius: 50%;
    top: 3px; left: 4px;
    transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
    box-shadow: 0 2px 6px rgba(0,0,0,0.3);
  }
  .toggle-input:checked + .toggle-track {
    background: var(--blurple);
    border-color: var(--blurple);
    box-shadow: 0 0 20px var(--blurple-glow);
  }
  .toggle-input:checked + .toggle-track::after {
    background: #fff;
    left: 24px;
  }

  /* INPUT */
  .field-input {
    width: 100%;
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 11px 14px;
    color: var(--text-primary);
    font-family: 'Outfit', sans-serif;
    font-size: 14px;
    transition: all 0.2s;
    outline: none;
  }
  .field-input::placeholder { color: var(--text-muted); }
  .field-input:focus {
    border-color: var(--blurple);
    box-shadow: 0 0 0 3px rgba(88,101,242,0.2);
    background: var(--bg-hover);
  }
  textarea.field-input { resize: vertical; min-height: 80px; line-height: 1.5; }

  /* SELECT */
  select.field-input {
    cursor: pointer;
    appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 12 12'%3E%3Cpath fill='%236d7180' d='M6 8L1 3h10z'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right 12px center;
    padding-right: 32px;
  }
  select.field-input option { background: var(--bg-panel); }

  /* TICKET SYSTEM */
  .ticket-builder {
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 20px;
    display: flex; flex-direction: column; gap: 16px;
  }
  .btn-color-picker { display: flex; gap: 8px; flex-wrap: wrap; }
  .color-btn {
    width: 32px; height: 32px;
    border-radius: 8px;
    border: 2px solid transparent;
    cursor: pointer;
    transition: all 0.2s;
    display: flex; align-items: center; justify-content: center;
    font-size: 10px; font-weight: 700;
  }
  .color-btn:hover { transform: scale(1.1); }
  .color-btn.selected { border-color: #fff; transform: scale(1.15); box-shadow: 0 0 12px currentColor; }
  .cb-blue { background: var(--blurple); color: #fff; }
  .cb-green { background: var(--green); color: #000; }
  .cb-red { background: var(--red); color: #fff; }
  .cb-gray { background: #4f545c; color: #fff; }

  /* LIVE PREVIEW */
  .preview-box {
    background: #313338;
    border-radius: 12px;
    padding: 16px;
    border: 1px solid rgba(255,255,255,0.06);
  }
  .preview-label { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: var(--text-muted); margin-bottom: 12px; display: flex; align-items: center; gap: 6px; }
  .preview-label::before { content: ''; width: 6px; height: 6px; border-radius: 50%; background: var(--green); box-shadow: 0 0 6px var(--green); }
  .embed-preview {
    background: #2b2d31;
    border-radius: 4px;
    border-left: 4px solid var(--blurple);
    padding: 12px 14px;
    font-size: 14px;
    position: relative;
    overflow: hidden;
  }
  .embed-preview::before {
    content: '';
    position: absolute; inset: 0;
    background: linear-gradient(135deg, rgba(88,101,242,0.05), transparent);
  }
  .embed-title { font-weight: 700; color: var(--blurple); margin-bottom: 4px; font-size: 15px; }
  .embed-desc { color: var(--text-secondary); font-size: 13px; line-height: 1.5; }
  .embed-footer { font-size: 11px; color: var(--text-muted); margin-top: 8px; display: flex; align-items: center; gap: 6px; }

  /* REACTION ROLES */
  .reaction-role-item {
    display: flex; align-items: center; gap: 12px;
    padding: 12px 14px;
    background: var(--bg-card);
    border: 1px solid var(--border);
    border-radius: 10px;
    transition: all 0.2s;
  }
  .reaction-role-item:hover { border-color: rgba(88,101,242,0.3); background: var(--bg-hover); }
  .rr-emoji { font-size: 22px; width: 36px; text-align: center; }
  .rr-role { font-size: 13px; font-weight: 600; }
  .rr-desc { font-size: 11px; color: var(--text-muted); }
  .rr-remove {
    margin-left: auto;
    background: var(--red-dim);
    border: 1px solid rgba(237,66,69,0.3);
    color: var(--red);
    border-radius: 6px;
    width: 28px; height: 28px;
    display: flex; align-items: center; justify-content: center;
    cursor: pointer;
    font-size: 14px;
    transition: all 0.2s;
  }
  .rr-remove:hover { background: rgba(237,66,69,0.25); }
  .add-rr-btn {
    display: flex; align-items: center; justify-content: center; gap: 8px;
    padding: 11px;
    background: transparent;
    border: 1px dashed rgba(88,101,242,0.35);
    border-radius: 10px;
    color: var(--blurple);
    font-size: 13px; font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
    font-family: 'Outfit';
  }
  .add-rr-btn:hover { background: rgba(88,101,242,0.1); border-color: var(--blurple); }

  /* SAVE BUTTON */
  .save-bar {
    position: sticky;
    bottom: 0;
    margin-top: 32px;
    padding: 20px 0;
    display: flex; align-items: center; justify-content: flex-end; gap: 12px;
    background: linear-gradient(to top, var(--bg-darkest) 80%, transparent);
    z-index: 10;
  }
  .btn-discard {
    padding: 12px 24px;
    background: transparent;
    border: 1px solid var(--border);
    border-radius: 10px;
    color: var(--text-secondary);
    font-family: 'Outfit'; font-size: 14px; font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
  }
  .btn-discard:hover { background: var(--bg-hover); color: var(--text-primary); }
  .btn-save {
    padding: 12px 32px;
    background: var(--blurple);
    border: none;
    border-radius: 10px;
    color: #fff;
    font-family: 'Outfit'; font-size: 14px; font-weight: 700;
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
    letter-spacing: 0.3px;
  }
  .btn-save::before {
    content: '';
    position: absolute; inset: 0;
    background: linear-gradient(135deg, rgba(255,255,255,0.15), transparent);
    opacity: 0; transition: opacity 0.2s;
  }
  .btn-save::after {
    content: '';
    position: absolute; top: 50%; left: 50%;
    width: 0; height: 0;
    background: rgba(255,255,255,0.2);
    border-radius: 50%;
    transform: translate(-50%, -50%);
    transition: width 0.5s, height 0.5s, opacity 0.5s;
  }
  .btn-save:hover { background: var(--blurple-hover); box-shadow: 0 8px 30px var(--blurple-glow); transform: translateY(-1px); }
  .btn-save:hover::before { opacity: 1; }
  .btn-save:active::after { width: 300px; height: 300px; opacity: 0; }

  /* TOAST */
  .toast {
    position: fixed;
    bottom: 32px; right: 32px;
    background: var(--bg-panel);
    border: 1px solid var(--green);
    border-radius: 12px;
    padding: 14px 20px;
    display: flex; align-items: center; gap: 10px;
    font-size: 14px; font-weight: 600;
    box-shadow: 0 8px 32px rgba(0,0,0,0.4), 0 0 20px rgba(87,242,135,0.15);
    transform: translateY(100px); opacity: 0;
    transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
    z-index: 9999;
  }
  .toast.show { transform: translateY(0); opacity: 1; }
  .toast-icon { font-size: 18px; }
  .toast-msg { color: var(--green); }

  /* PAGE SECTIONS (show/hide) */
  .page-section { display: none; }
  .page-section.active { display: block; }

  /* DIVIDER */
  .divider { height: 1px; background: var(--border); margin: 4px 0; }

  /* FIELD LABEL STANDALONE */
  .label-block { display: flex; flex-direction: column; gap: 6px; }
  .label-text { font-size: 12px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.8px; color: var(--text-muted); }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg-dark); }
  ::-webkit-scrollbar-thumb { background: var(--bg-hover); border-radius: 3px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--text-muted); }

  /* ROLES CHIP */
  .role-chip {
    display: inline-flex; align-items: center; gap: 6px;
    background: rgba(88,101,242,0.15);
    border: 1px solid rgba(88,101,242,0.3);
    border-radius: 6px;
    padding: 4px 10px;
    font-size: 12px; font-weight: 600; color: #c9cdfb;
  }
  .role-chip::before { content: '@'; color: var(--blurple); }

  /* STAGGER ANIMATIONS */
  .panel:nth-child(1) { animation-delay: 0.05s; }
  .panel:nth-child(2) { animation-delay: 0.1s; }
  .panel:nth-child(3) { animation-delay: 0.15s; }
  .panel:nth-child(4) { animation-delay: 0.2s; }
  .panel:nth-child(5) { animation-delay: 0.25s; }
</style>
</head>
<body>

<!-- BG FX -->
<div class="bg-fx"><div class="bg-orb3"></div></div>
<div class="particles" id="particles"></div>

<div class="layout">
  <!-- SIDEBAR -->
  <aside class="sidebar">
    <div class="sidebar-logo">
      <div class="logo-icon">🤖</div>
      <div>
        <div class="logo-text">Pro<span>Bot</span></div>
        <div style="font-size:10px;color:var(--text-muted);font-family:'JetBrains Mono'">v2.4.1</div>
      </div>
    </div>

    <div class="server-info">
      <div class="server-avatar">🏰</div>
      <div>
        <div class="server-name">My Discord Server</div>
        <div class="server-id">ID: 123456789</div>
      </div>
      <div class="server-chevron">▾</div>
    </div>

    <nav style="flex:1;overflow-y:auto;padding-bottom:8px">
      <div class="nav-section">
        <div class="nav-label">Overview</div>
        <div class="nav-item active" onclick="showPage('dashboard',this)">
          <span class="nav-icon">🏠</span> Dashboard
        </div>
        <div class="nav-item" onclick="showPage('general',this)">
          <span class="nav-icon">⚙️</span> General Settings
        </div>
      </div>

      <div class="nav-section">
        <div class="nav-label">Features</div>
        <div class="nav-item" onclick="showPage('welcome',this)">
          <span class="nav-icon">👋</span> Welcome & Leave
        </div>
        <div class="nav-item" onclick="showPage('moderation',this)">
          <span class="nav-icon">🛡️</span> Moderation
          <span class="nav-badge">3</span>
        </div>
        <div class="nav-item" onclick="showPage('tickets',this)">
          <span class="nav-icon">🎫</span> Ticket System
        </div>
        <div class="nav-item" onclick="showPage('roles',this)">
          <span class="nav-icon">🎭</span> Reaction Roles
        </div>
        <div class="nav-item" onclick="showPage('logging',this)">
          <span class="nav-icon">📋</span> Logging
        </div>
      </div>
    </nav>

    <div class="sidebar-footer">
      <div class="bot-status-pill">
        <div class="status-dot"></div>
        <div class="status-text">Bot Online</div>
        <div style="margin-left:auto;font-size:11px;color:var(--text-muted)">24ms</div>
      </div>
    </div>
  </aside>

  <!-- MAIN CONTENT -->
  <main class="main">

    <!-- DASHBOARD PAGE -->
    <div class="page-section active" id="page-dashboard">
      <div class="page-header">
        <div class="page-title">Welcome back, <span>Admin</span> 👋</div>
        <div class="page-subtitle">Here's what's happening with your server today</div>
      </div>

      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-card-bg">🖥️</div>
          <div class="stat-label">Total Servers</div>
          <div class="stat-value blurple" id="cnt-servers">0</div>
          <div class="stat-delta">↑ +12 this week</div>
        </div>
        <div class="stat-card">
          <div class="stat-card-bg">👥</div>
          <div class="stat-label">Total Users</div>
          <div class="stat-value cyan" id="cnt-users">0</div>
          <div class="stat-delta" style="color:var(--cyan)">↑ +340 today</div>
        </div>
        <div class="stat-card">
          <div class="stat-card-bg">✅</div>
          <div class="stat-label">Bot Status</div>
          <div class="stat-value green">Online</div>
          <div class="stat-delta">Uptime: 99.9%</div>
        </div>
      </div>

      <div class="panels-grid">
        <div class="panel" style="animation-delay:0.1s">
          <div class="panel-header">
            <div class="panel-icon pi-blue">📊</div>
            <div><div class="panel-title">Quick Overview</div><div class="panel-subtitle">Server activity summary</div></div>
          </div>
          <div class="panel-body">
            <div style="display:flex;flex-direction:column;gap:14px">
              <div style="display:flex;align-items:center;justify-content:space-between">
                <span style="font-size:13px;color:var(--text-secondary)">Messages today</span>
                <span style="font-weight:700;color:var(--blurple)">1,248</span>
              </div>
              <div style="height:6px;background:var(--bg-hover);border-radius:3px;overflow:hidden">
                <div style="height:100%;width:72%;background:linear-gradient(90deg,var(--blurple),var(--cyan));border-radius:3px;animation:growBar 1.2s 0.3s ease both;transform-origin:left" id="bar1"></div>
              </div>
              <div style="display:flex;align-items:center;justify-content:space-between">
                <span style="font-size:13px;color:var(--text-secondary)">New members</span>
                <span style="font-weight:700;color:var(--green)">34</span>
              </div>
              <div style="height:6px;background:var(--bg-hover);border-radius:3px;overflow:hidden">
                <div style="height:100%;width:34%;background:linear-gradient(90deg,var(--green),var(--cyan));border-radius:3px;animation:growBar 1.2s 0.5s ease both" id="bar2"></div>
              </div>
              <div style="display:flex;align-items:center;justify-content:space-between">
                <span style="font-size:13px;color:var(--text-secondary)">Tickets opened</span>
                <span style="font-weight:700;color:var(--yellow)">7</span>
              </div>
              <div style="height:6px;background:var(--bg-hover);border-radius:3px;overflow:hidden">
                <div style="height:100%;width:15%;background:linear-gradient(90deg,var(--yellow),var(--pink));border-radius:3px;animation:growBar 1.2s 0.7s ease both" id="bar3"></div>
              </div>
            </div>
          </div>
        </div>

        <div class="panel" style="animation-delay:0.15s">
          <div class="panel-header">
            <div class="panel-icon pi-green">🔔</div>
            <div><div class="panel-title">Recent Events</div><div class="panel-subtitle">Last 5 server events</div></div>
          </div>
          <div class="panel-body" style="gap:10px">
            <div class="reaction-role-item">
              <span style="font-size:18px">👋</span>
              <div><div class="rr-role">JohnDoe joined</div><div class="rr-desc">2 minutes ago</div></div>
              <div style="margin-left:auto" class="role-chip" style="font-size:10px">New Member</div>
            </div>
            <div class="reaction-role-item">
              <span style="font-size:18px">🎫</span>
              <div><div class="rr-role">Ticket #42 opened</div><div class="rr-desc">15 minutes ago</div></div>
            </div>
            <div class="reaction-role-item">
              <span style="font-size:18px">🛡️</span>
              <div><div class="rr-role">Spam detected & blocked</div><div class="rr-desc">1 hour ago</div></div>
              <div style="margin-left:auto;background:var(--red-dim);border:1px solid rgba(237,66,69,0.3);color:var(--red);border-radius:5px;padding:2px 8px;font-size:11px;font-weight:600">Blocked</div>
            </div>
            <div class="reaction-role-item">
              <span style="font-size:18px">⚙️</span>
              <div><div class="rr-role">Settings updated</div><div class="rr-desc">3 hours ago</div></div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- WELCOME PAGE -->
    <div class="page-section" id="page-welcome">
      <div class="page-header">
        <div class="page-title"><span>Welcome</span> & Leave System</div>
        <div class="page-subtitle">Configure join/leave messages for your server</div>
      </div>

      <div class="panels-grid">
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-green">👋</div>
            <div><div class="panel-title">Welcome System</div><div class="panel-subtitle">Greet new members</div></div>
          </div>
          <div class="panel-body">
            <div class="field-row">
              <div>
                <div class="field-label">Enable Welcome Messages</div>
                <div class="field-desc">Send a message when someone joins</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-welcome" checked onchange="updatePreview()">
                <label class="toggle-track" for="t-welcome"></label>
              </div>
            </div>
            <div class="divider"></div>
            <div class="label-block">
              <div class="label-text">Welcome Channel</div>
              <select class="field-input">
                <option>👋 #welcome</option>
                <option>💬 #general</option>
                <option>📢 #announcements</option>
              </select>
            </div>
            <div class="label-block">
              <div class="label-text">Welcome Message</div>
              <textarea class="field-input" id="welcome-msg" placeholder="Welcome {user} to {server}! 🎉" oninput="updatePreview()">Welcome {user} to {server}! 🎉 You are member #{count}.</textarea>
            </div>
            <div class="label-block">
              <div class="label-text">Banner Image URL</div>
              <input type="url" class="field-input" placeholder="https://i.imgur.com/...">
            </div>
          </div>
        </div>

        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-yellow">👁️</div>
            <div><div class="panel-title">Live Preview</div><div class="panel-subtitle">See how it'll look in Discord</div></div>
          </div>
          <div class="panel-body">
            <div class="preview-box">
              <div class="preview-label">Discord Preview</div>
              <div class="embed-preview" id="embed-preview" style="border-left-color:var(--green)">
                <div class="embed-title">👋 New Member!</div>
                <div class="embed-desc" id="preview-text">Welcome <strong style="color:var(--blurple)">@NewUser</strong> to <strong>My Discord Server</strong>! 🎉 You are member #1,234.</div>
                <div class="embed-footer">
                  <span>🤖</span> ProBot • Today at 12:00 PM
                </div>
              </div>
            </div>
            <div class="field-row">
              <div>
                <div class="field-label">Enable Leave Messages</div>
                <div class="field-desc">Send a message when someone leaves</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-leave">
                <label class="toggle-track" for="t-leave"></label>
              </div>
            </div>
            <div class="label-block">
              <div class="label-text">Leave Channel</div>
              <select class="field-input">
                <option>👋 #welcome</option>
                <option>📋 #logs</option>
              </select>
            </div>
          </div>
        </div>
      </div>

      <div class="save-bar">
        <button class="btn-discard" onclick="showToast('Changes discarded','#ED4245')">Discard</button>
        <button class="btn-save" onclick="showToast('✅ Changes saved successfully!','#57F287')">💾 Save Changes</button>
      </div>
    </div>

    <!-- MODERATION PAGE -->
    <div class="page-section" id="page-moderation">
      <div class="page-header">
        <div class="page-title"><span>Moderation</span> & Security</div>
        <div class="page-subtitle">Protect your server from spam and bad actors</div>
      </div>
      <div class="panels-grid">
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-red">🛡️</div>
            <div><div class="panel-title">Anti Bad-Things</div><div class="panel-subtitle">Auto-moderation toggles</div></div>
          </div>
          <div class="panel-body">
            <div class="field-row">
              <div>
                <div class="field-label">🔗 Anti-Link</div>
                <div class="field-desc">Block unauthorized links in chat</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-antilink" checked>
                <label class="toggle-track" for="t-antilink"></label>
              </div>
            </div>
            <div class="divider"></div>
            <div class="field-row">
              <div>
                <div class="field-label">📨 Anti-Spam</div>
                <div class="field-desc">Limit message flood attacks</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-antispam" checked>
                <label class="toggle-track" for="t-antispam"></label>
              </div>
            </div>
            <div class="divider"></div>
            <div class="field-row">
              <div>
                <div class="field-label">🤖 Anti-Bot</div>
                <div class="field-desc">Prevent unauthorized bots from joining</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-antibot">
                <label class="toggle-track" for="t-antibot"></label>
              </div>
            </div>
          </div>
        </div>
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-blue">🎖️</div>
            <div><div class="panel-title">Auto-Role</div><div class="panel-subtitle">Assign roles on join</div></div>
          </div>
          <div class="panel-body">
            <div class="label-block">
              <div class="label-text">Default Member Role</div>
              <select class="field-input">
                <option>🟢 Member</option>
                <option>🔵 Verified</option>
                <option>⚪ Unverified</option>
                <option>🟡 New Member</option>
              </select>
            </div>
            <div style="padding:12px;background:var(--green-dim);border:1px solid rgba(87,242,135,0.2);border-radius:10px;font-size:12px;color:var(--text-secondary);line-height:1.6">
              💡 This role will be automatically assigned to everyone who joins your server. Make sure the bot's role is above this role in the hierarchy.
            </div>
            <div class="label-block">
              <div class="label-text">Bot Role (ignore)</div>
              <select class="field-input">
                <option>🤖 ProBot</option>
                <option>🛡️ MEE6</option>
              </select>
            </div>
          </div>
        </div>
      </div>
      <div class="save-bar">
        <button class="btn-discard">Discard</button>
        <button class="btn-save" onclick="showToast('✅ Moderation settings saved!','#57F287')">💾 Save Changes</button>
      </div>
    </div>

    <!-- TICKETS PAGE -->
    <div class="page-section" id="page-tickets">
      <div class="page-header">
        <div class="page-title"><span>Ticket</span> System</div>
        <div class="page-subtitle">Build your support ticket button</div>
      </div>
      <div class="panels-grid">
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-cyan">🎫</div>
            <div><div class="panel-title">Button Creator</div><div class="panel-subtitle">Customize the ticket open button</div></div>
          </div>
          <div class="panel-body">
            <div class="ticket-builder">
              <div class="label-block">
                <div class="label-text">Button Text</div>
                <input type="text" class="field-input" id="ticket-btn-text" value="🎫 Open a Ticket" oninput="updateTicketPreview()">
              </div>
              <div class="label-block">
                <div class="label-text">Button Color</div>
                <div class="btn-color-picker">
                  <div class="color-btn cb-blue selected" onclick="selectColor(this,'var(--blurple)')">Blurple</div>
                  <div class="color-btn cb-green" onclick="selectColor(this,'var(--green)')">Green</div>
                  <div class="color-btn cb-red" onclick="selectColor(this,'var(--red)')">Red</div>
                  <div class="color-btn cb-gray" onclick="selectColor(this,'#4f545c')">Gray</div>
                </div>
              </div>
              <div class="label-block">
                <div class="label-text">Ticket Category</div>
                <select class="field-input">
                  <option>📂 Support</option>
                  <option>📂 Appeals</option>
                  <option>📂 Reports</option>
                  <option>📂 Applications</option>
                </select>
              </div>
            </div>
          </div>
        </div>
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-yellow">👁️</div>
            <div><div class="panel-title">Ticket Preview</div><div class="panel-subtitle">Live Discord preview</div></div>
          </div>
          <div class="panel-body">
            <div class="preview-box">
              <div class="preview-label">Discord Preview</div>
              <div style="background:#2b2d31;border-radius:8px;padding:14px">
                <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px">
                  <div style="width:32px;height:32px;border-radius:50%;background:linear-gradient(135deg,var(--blurple),var(--pink));display:flex;align-items:center;justify-content:center;font-size:14px">🤖</div>
                  <div style="font-size:13px;font-weight:700">ProBot <span style="background:#5865F2;color:#fff;font-size:9px;padding:1px 5px;border-radius:3px;font-weight:700">BOT</span></div>
                </div>
                <div style="font-size:13px;color:var(--text-secondary);margin-bottom:12px">Need help? Click the button below to open a support ticket!</div>
                <div id="ticket-btn-preview" style="display:inline-flex;align-items:center;gap:6px;padding:9px 16px;background:var(--blurple);border-radius:4px;font-size:13px;font-weight:600;cursor:pointer;transition:all 0.2s" onmouseover="this.style.opacity='.85'" onmouseout="this.style.opacity='1'">
                  🎫 Open a Ticket
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="save-bar">
        <button class="btn-discard">Discard</button>
        <button class="btn-save" onclick="showToast('✅ Ticket system updated!','#57F287')">💾 Save Changes</button>
      </div>
    </div>

    <!-- REACTION ROLES PAGE -->
    <div class="page-section" id="page-roles">
      <div class="page-header">
        <div class="page-title"><span>Reaction</span> Roles</div>
        <div class="page-subtitle">Let users self-assign roles with emoji reactions</div>
      </div>
      <div class="panels-grid">
        <div class="panel panel-full">
          <div class="panel-header">
            <div class="panel-icon pi-pink">🎭</div>
            <div><div class="panel-title">Role Assignments</div><div class="panel-subtitle">4 reaction roles configured</div></div>
          </div>
          <div class="panel-body">
            <div id="rr-list" style="display:flex;flex-direction:column;gap:8px">
              <div class="reaction-role-item">
                <span class="rr-emoji">🎮</span>
                <div><div class="rr-role">Gamer</div><div class="rr-desc">For gaming enthusiasts</div></div>
                <div style="margin-left:auto;display:flex;align-items:center;gap:8px">
                  <span class="role-chip">Gamer</span>
                  <div class="rr-remove" onclick="removeRR(this)">✕</div>
                </div>
              </div>
              <div class="reaction-role-item">
                <span class="rr-emoji">🎵</span>
                <div><div class="rr-role">Music Lover</div><div class="rr-desc">For music fans</div></div>
                <div style="margin-left:auto;display:flex;align-items:center;gap:8px">
                  <span class="role-chip">Music</span>
                  <div class="rr-remove" onclick="removeRR(this)">✕</div>
                </div>
              </div>
              <div class="reaction-role-item">
                <span class="rr-emoji">💻</span>
                <div><div class="rr-role">Developer</div><div class="rr-desc">Coding & tech role</div></div>
                <div style="margin-left:auto;display:flex;align-items:center;gap:8px">
                  <span class="role-chip">Dev</span>
                  <div class="rr-remove" onclick="removeRR(this)">✕</div>
                </div>
              </div>
              <div class="reaction-role-item">
                <span class="rr-emoji">🎨</span>
                <div><div class="rr-role">Artist</div><div class="rr-desc">Creative & design role</div></div>
                <div style="margin-left:auto;display:flex;align-items:center;gap:8px">
                  <span class="role-chip">Artist</span>
                  <div class="rr-remove" onclick="removeRR(this)">✕</div>
                </div>
              </div>
            </div>
            <button class="add-rr-btn" onclick="addRR()">+ Add Reaction Role</button>
          </div>
        </div>
      </div>
      <div class="save-bar">
        <button class="btn-discard">Discard</button>
        <button class="btn-save" onclick="showToast('✅ Reaction roles saved!','#57F287')">💾 Save Changes</button>
      </div>
    </div>

    <!-- LOGGING PAGE -->
    <div class="page-section" id="page-logging">
      <div class="page-header">
        <div class="page-title"><span>Logging</span> System</div>
        <div class="page-subtitle">Track deleted messages and server events</div>
      </div>
      <div class="panels-grid">
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-blue">📋</div>
            <div><div class="panel-title">Message Logs</div><div class="panel-subtitle">Log deleted messages</div></div>
          </div>
          <div class="panel-body">
            <div class="field-row">
              <div>
                <div class="field-label">Enable Message Logging</div>
                <div class="field-desc">Log edited and deleted messages</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-logging" checked>
                <label class="toggle-track" for="t-logging"></label>
              </div>
            </div>
            <div class="label-block">
              <div class="label-text">Log Channel</div>
              <select class="field-input">
                <option>📋 #logs</option>
                <option>🔍 #mod-logs</option>
                <option>📢 #admin-logs</option>
              </select>
            </div>
            <div class="field-row">
              <div>
                <div class="field-label">Log Edited Messages</div>
                <div class="field-desc">Show before & after edits</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-log-edit" checked>
                <label class="toggle-track" for="t-log-edit"></label>
              </div>
            </div>
            <div class="field-row">
              <div>
                <div class="field-label">Log Voice Activity</div>
                <div class="field-desc">Track voice channel joins/leaves</div>
              </div>
              <div class="toggle-wrap">
                <input type="checkbox" class="toggle-input" id="t-log-voice">
                <label class="toggle-track" for="t-log-voice"></label>
              </div>
            </div>
          </div>
        </div>
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-yellow">👁️</div>
            <div><div class="panel-title">Log Preview</div><div class="panel-subtitle">Example log entry</div></div>
          </div>
          <div class="panel-body">
            <div class="preview-box">
              <div class="preview-label">Example Log Entry</div>
              <div class="embed-preview" style="border-left-color:var(--red)">
                <div class="embed-title" style="color:var(--red)">🗑️ Message Deleted</div>
                <div class="embed-desc">
                  <div style="margin-bottom:6px"><span style="color:var(--text-muted)">Author:</span> <strong>@JohnDoe</strong></div>
                  <div style="margin-bottom:6px"><span style="color:var(--text-muted)">Channel:</span> #general</div>
                  <div><span style="color:var(--text-muted)">Content:</span> Hello world this is a test message</div>
                </div>
                <div class="embed-footer"><span>📋</span> ProBot Logs • Today at 12:34 PM</div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="save-bar">
        <button class="btn-discard">Discard</button>
        <button class="btn-save" onclick="showToast('✅ Logging settings saved!','#57F287')">💾 Save Changes</button>
      </div>
    </div>

    <!-- GENERAL PAGE -->
    <div class="page-section" id="page-general">
      <div class="page-header">
        <div class="page-title"><span>General</span> Settings</div>
        <div class="page-subtitle">Core bot configuration</div>
      </div>
      <div class="panels-grid">
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-blue">⚙️</div>
            <div><div class="panel-title">Basic Config</div><div class="panel-subtitle">Prefix, language, timezone</div></div>
          </div>
          <div class="panel-body">
            <div class="label-block">
              <div class="label-text">Command Prefix</div>
              <input type="text" class="field-input" value="!" style="font-family:'JetBrains Mono';font-size:16px;font-weight:700;max-width:80px">
            </div>
            <div class="label-block">
              <div class="label-text">Bot Language</div>
              <select class="field-input">
                <option>🇺🇸 English</option>
                <option>🇫🇷 Français</option>
                <option>🇦🇪 العربية</option>
                <option>🇩🇪 Deutsch</option>
              </select>
            </div>
            <div class="label-block">
              <div class="label-text">Timezone</div>
              <select class="field-input">
                <option>UTC+0</option>
                <option>UTC+1 (CET)</option>
                <option>UTC-5 (EST)</option>
                <option>UTC+3 (AST)</option>
              </select>
            </div>
          </div>
        </div>
        <div class="panel">
          <div class="panel-header">
            <div class="panel-icon pi-pink">🎨</div>
            <div><div class="panel-title">Embed Color</div><div class="panel-subtitle">Default embed accent color</div></div>
          </div>
          <div class="panel-body">
            <div class="label-block">
              <div class="label-text">Accent Color</div>
              <div style="display:flex;gap:10px;align-items:center">
                <input type="color" value="#5865F2" style="width:48px;height:48px;border:none;border-radius:10px;cursor:pointer;background:none">
                <input type="text" class="field-input" value="#5865F2" style="font-family:'JetBrains Mono';max-width:120px">
              </div>
            </div>
            <div style="padding:14px;background:var(--bg-card);border-radius:12px;border-left:4px solid var(--blurple)">
              <div style="font-weight:700;color:var(--blurple);margin-bottom:4px;font-size:14px">Color Preview</div>
              <div style="font-size:13px;color:var(--text-secondary)">This color will be used for all bot embeds in your server.</div>
            </div>
          </div>
        </div>
      </div>
      <div class="save-bar">
        <button class="btn-discard">Discard</button>
        <button class="btn-save" onclick="showToast('✅ General settings saved!','#57F287')">💾 Save Changes</button>
      </div>
    </div>

  </main>
</div>

<!-- TOAST -->
<div class="toast" id="toast">
  <span class="toast-icon">✅</span>
  <span class="toast-msg" id="toast-msg">Changes saved!</span>
</div>

<style>
  @keyframes growBar { from{width:0} }
</style>

<script>
  // PARTICLES
  const particlesEl = document.getElementById('particles');
  const colors = ['#5865F2','#eb459e','#00b4d8','#57F287','#FEE75C'];
  for(let i=0;i<25;i++){
    const p = document.createElement('div');
    p.className = 'particle';
    const size = Math.random()*4+2;
    p.style.cssText = `
      width:${size}px;height:${size}px;
      left:${Math.random()*100}%;
      animation-duration:${Math.random()*20+15}s;
      animation-delay:${Math.random()*20}s;
      background:${colors[Math.floor(Math.random()*colors.length)]};
    `;
    particlesEl.appendChild(p);
  }

  // COUNT ANIMATION
  function animateCount(id, target, duration=1800){
    const el = document.getElementById(id);
    if(!el) return;
    let start = 0;
    const step = target/60;
    const interval = duration/60;
    const timer = setInterval(()=>{
      start += step;
      if(start >= target){ el.textContent = target.toLocaleString(); clearInterval(timer); return; }
      el.textContent = Math.floor(start).toLocaleString();
    }, interval);
  }
  setTimeout(()=>{
    animateCount('cnt-servers', 1247);
    animateCount('cnt-users', 89432);
  }, 300);

  // PAGE NAVIGATION
  function showPage(page, navItem){
    document.querySelectorAll('.page-section').forEach(s=>s.classList.remove('active'));
    document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
    document.getElementById('page-'+page).classList.add('active');
    if(navItem) navItem.classList.add('active');
    window.scrollTo({top:0,behavior:'smooth'});
  }

  // PREVIEW UPDATE
  function updatePreview(){
    const msg = document.getElementById('welcome-msg').value;
    const formatted = msg.replace('{user}','<strong style="color:var(--blurple)">@NewUser</strong>')
                         .replace('{server}','<strong>My Discord Server</strong>')
                         .replace('{count}','<strong>#1,234</strong>');
    document.getElementById('preview-text').innerHTML = formatted;
  }

  // TICKET PREVIEW
  function updateTicketPreview(){
    const text = document.getElementById('ticket-btn-text').value || 'Open a Ticket';
    document.getElementById('ticket-btn-preview').textContent = text;
  }
  let selectedBtnColor = 'var(--blurple)';
  function selectColor(el, color){
    document.querySelectorAll('.color-btn').forEach(b=>b.classList.remove('selected'));
    el.classList.add('selected');
    selectedBtnColor = color;
    const preview = document.getElementById('ticket-btn-preview');
    if(preview) preview.style.background = color;
  }

  // REACTION ROLES
  function removeRR(btn){
    const item = btn.closest('.reaction-role-item');
    item.style.transform = 'translateX(20px)';
    item.style.opacity = '0';
    item.style.transition = 'all 0.3s';
    setTimeout(()=>item.remove(), 300);
  }
  const rrEmojis = ['⭐','🔥','💎','🌊','🦁','🐉','⚡','🍀'];
  const rrRoles = ['Star','Fire','Diamond','Water','Lion','Dragon','Thunder','Luck'];
  function addRR(){
    const list = document.getElementById('rr-list');
    const idx = Math.floor(Math.random()*rrEmojis.length);
    const item = document.createElement('div');
    item.className = 'reaction-role-item';
    item.style.cssText = 'transform:translateX(-20px);opacity:0;transition:all 0.3s';
    item.innerHTML = `
      <span class="rr-emoji">${rrEmojis[idx]}</span>
      <div><div class="rr-role">${rrRoles[idx]}</div><div class="rr-desc">New reaction role</div></div>
      <div style="margin-left:auto;display:flex;align-items:center;gap:8px">
        <span class="role-chip">${rrRoles[idx]}</span>
        <div class="rr-remove" onclick="removeRR(this)">✕</div>
      </div>
    `;
    list.appendChild(item);
    requestAnimationFrame(()=>requestAnimationFrame(()=>{
      item.style.transform = 'translateX(0)';
      item.style.opacity = '1';
    }));
  }

  // TOAST
  let toastTimer;
  function showToast(msg, color='#57F287'){
    clearTimeout(toastTimer);
    const toast = document.getElementById('toast');
    const toastMsg = document.getElementById('toast-msg');
    const toastIcon = toast.querySelector('.toast-icon');
    toast.style.borderColor = color;
    toastMsg.style.color = color;
    toastMsg.textContent = msg;
    toastIcon.textContent = color === '#57F287' ? '✅' : '❌';
    toast.classList.add('show');
    toastTimer = setTimeout(()=>toast.classList.remove('show'), 3000);
  }
</script>
</body>
</html>
