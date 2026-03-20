<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>amenitiz — career compass</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,300&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --blue:        #507FE6;
    --blue-dark:   #2D58B8;
    --blue-darker: #1A3A8A;
    --blue-light:  #7BA3EF;
    --blue-pale:   #D6E4FB;
    --blue-bg:     #EEF4FF;
    --gold:        #C9A96E;
    --gold-light:  #E8D5B0;
    --white:       #FFFFFF;
    --gray-50:     #F7F9FE;
    --gray-100:    #EBF0FA;
    --gray-200:    #CFDAF5;
    --gray-400:    #8FA6D4;
    --gray-600:    #4A6199;
    --gray-900:    #0D1F4A;
    --font:        'DM Sans', sans-serif;
    --radius:      16px;
    --radius-sm:   10px;
  }

  body {
    font-family: var(--font);
    background: var(--gray-50);
    color: var(--gray-900);
    min-height: 100vh;
    line-height: 1.6;
  }

  /* ── LOGIN GATE ── */
  #gate {
    position: fixed; inset: 0; z-index: 1000;
    background: var(--blue-darker);
    display: flex; align-items: center; justify-content: center;
    padding: 24px;
  }
  #gate.hidden { display: none; }

  .gate-bg {
    position: absolute; inset: 0; overflow: hidden; pointer-events: none;
  }
  .gate-blob {
    position: absolute; border-radius: 50%;
    background: rgba(255,255,255,0.04);
  }
  .gb1 { width: 400px; height: 320px; top: -80px; left: -60px; border-radius: 60% 40% 70% 30%; }
  .gb2 { width: 280px; height: 220px; bottom: -40px; right: -40px; border-radius: 40% 60% 30% 70%; }
  .gb3 { width: 160px; height: 160px; top: 40%; left: 60%; border-radius: 50%; background: rgba(201,169,110,0.06); }
  .gate-deco-tl {
    position: absolute; top: 32px; left: 32px; opacity: 0.25;
  }
  .gate-deco-br {
    position: absolute; bottom: 32px; right: 32px; opacity: 0.2;
  }

  .gate-card {
    position: relative; z-index: 1;
    background: rgba(255,255,255,0.06);
    border: 1px solid rgba(255,255,255,0.12);
    border-radius: 20px;
    padding: 40px 36px 36px;
    width: 100%; max-width: 400px;
    backdrop-filter: blur(12px);
    animation: gateIn .4s cubic-bezier(.22,1,.36,1) both;
  }
  @keyframes gateIn {
    from { opacity:0; transform: translateY(16px) scale(.98); }
    to   { opacity:1; transform: translateY(0) scale(1); }
  }

  .gate-logo {
    font-size: 18px; font-weight: 300; color: var(--white);
    letter-spacing: -.01em; margin-bottom: 4px;
  }
  .gate-title {
    font-size: 22px; font-weight: 500; color: var(--white);
    margin-bottom: 6px; line-height: 1.2;
  }
  .gate-sub {
    font-size: 13px; color: rgba(255,255,255,0.5);
    margin-bottom: 28px; line-height: 1.5;
  }

  .gate-field { margin-bottom: 14px; }
  .gate-label {
    display: block; font-size: 11px; font-weight: 500;
    text-transform: uppercase; letter-spacing: .07em;
    color: rgba(255,255,255,0.5); margin-bottom: 6px;
  }
  .gate-input {
    width: 100%; padding: 11px 14px;
    background: rgba(255,255,255,0.08);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 10px;
    font-family: var(--font); font-size: 14px;
    color: var(--white);
    outline: none; transition: border-color .15s, background .15s;
  }
  .gate-input::placeholder { color: rgba(255,255,255,0.3); }
  .gate-input:focus {
    border-color: rgba(255,255,255,0.4);
    background: rgba(255,255,255,0.12);
  }
  .gate-input.error { border-color: #F08080; }

  .gate-error {
    font-size: 12px; color: #F08080;
    margin-top: 8px; min-height: 18px;
    display: flex; align-items: center; gap: 5px;
  }
  .gate-error::before { content: ''; display: inline-block; width: 6px; height: 6px; border-radius: 50%; background: #F08080; flex-shrink: 0; }
  .gate-error:empty { display: none; }
  .gate-error:empty::before { display: none; }

  .gate-btn {
    width: 100%; margin-top: 20px;
    padding: 13px;
    background: var(--blue);
    border: none; border-radius: 10px;
    font-family: var(--font); font-size: 14px; font-weight: 500;
    color: var(--white); cursor: pointer;
    transition: background .15s, transform .1s;
    letter-spacing: .01em;
  }
  .gate-btn:hover { background: var(--blue-dark); }
  .gate-btn:active { transform: scale(.99); }

  .gate-hint {
    margin-top: 16px; text-align: center;
    font-size: 11.5px; color: rgba(255,255,255,0.3);
    line-height: 1.5;
  }

  /* ── APP (hidden until auth) ── */
  #app { display: none; }
  #app.visible { display: block; }

  /* ── Header ── */
  header {
    background: var(--blue);
    padding: 0;
    position: relative;
    overflow: hidden;
    min-height: 72px;
    display: flex;
    align-items: center;
  }
  .header-bg { position: absolute; inset: 0; pointer-events: none; }
  .blob { position: absolute; border-radius: 50%; background: rgba(255,255,255,0.08); }
  .blob-1 { width: 180px; height: 140px; top: -40px; left: 60px; border-radius: 60% 40% 70% 30%; }
  .blob-2 { width: 120px; height: 100px; top: 10px; right: 200px; border-radius: 40% 60% 30% 70%; }
  .blob-3 { width: 80px; height: 80px; bottom: -20px; right: 80px; border-radius: 50%; }
  .deco-svg { position: absolute; right: 24px; top: 50%; transform: translateY(-50%); opacity: 0.5; }
  .header-inner {
    position: relative; z-index: 1;
    padding: 16px 40px;
    display: flex; align-items: center; justify-content: space-between;
    width: 100%;
  }
  .logo { font-size: 20px; font-weight: 300; color: var(--white); letter-spacing: -.01em; }
  .header-right { display: flex; align-items: center; gap: 16px; }
  .header-tag { font-size: 12px; font-weight: 400; color: rgba(255,255,255,0.7); letter-spacing: .02em; }
  .logout-btn {
    font-size: 11.5px; font-weight: 400; color: rgba(255,255,255,0.5);
    background: none; border: 1px solid rgba(255,255,255,0.2);
    border-radius: 20px; padding: 4px 12px; cursor: pointer;
    font-family: var(--font); transition: all .15s; letter-spacing: .01em;
  }
  .logout-btn:hover { color: var(--white); border-color: rgba(255,255,255,0.5); }

  /* ── Container ── */
  .container { max-width: 1100px; margin: 0 auto; padding: 28px 24px 60px; }

  /* ── Role selector ── */
  .selector-wrap { margin-bottom: 24px; }
  .section-label {
    font-size: 11px; font-weight: 500; text-transform: uppercase;
    letter-spacing: .08em; color: var(--gray-400); margin-bottom: 10px;
  }
  .role-groups { display: flex; flex-direction: column; gap: 8px; }
  .role-group { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }
  .group-tag {
    font-size: 10px; font-weight: 500; text-transform: uppercase;
    letter-spacing: .06em; color: var(--gray-400); min-width: 56px;
  }
  .role-btn {
    padding: 6px 14px; border-radius: 20px; font-size: 12.5px;
    font-family: var(--font); cursor: pointer;
    border: 1.5px solid var(--gray-200); color: var(--gray-600);
    background: var(--white); transition: all .15s; font-weight: 400;
  }
  .role-btn:hover { border-color: var(--blue-light); color: var(--blue); background: var(--blue-bg); }
  .role-btn.active { border-color: var(--blue); background: var(--blue); color: var(--white); font-weight: 500; }

  /* ── Journey card ── */
  .journey-card {
    background: var(--blue); border-radius: var(--radius);
    padding: 20px 24px 18px; margin-bottom: 16px;
    position: relative; overflow: hidden;
  }
  .journey-card .blob { background: rgba(255,255,255,0.06); }
  .journey-card .jb1 { width: 160px; height: 120px; top: -30px; right: 40px; border-radius: 50% 40% 60% 50%; }
  .journey-card .jb2 { width: 80px; height: 80px; bottom: -20px; left: 20px; border-radius: 40% 60% 50% 60%; }
  .journey-deco { position: absolute; right: 20px; bottom: 10px; opacity: 0.3; }
  .journey-label {
    font-size: 11px; font-weight: 500; text-transform: uppercase;
    letter-spacing: .08em; color: rgba(255,255,255,0.6); margin-bottom: 14px;
    position: relative; z-index: 1;
  }
  .journey {
    display: flex; align-items: center; gap: 0;
    overflow-x: auto; padding-bottom: 2px; position: relative; z-index: 1;
  }
  .jstep { display: flex; flex-direction: column; align-items: center; flex-shrink: 0; }
  .jcircle {
    width: 42px; height: 42px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-size: 10px; font-weight: 500; flex-shrink: 0;
  }
  .j-past    { background: rgba(255,255,255,0.15); color: rgba(255,255,255,0.5); border: 1.5px solid rgba(255,255,255,0.2); }
  .j-current { background: var(--white); color: var(--blue); border: 2px solid var(--white); font-weight: 600; }
  .j-next    { background: rgba(255,255,255,0.25); color: var(--white); border: 2px solid rgba(255,255,255,0.5); }
  .j-future  { background: transparent; color: rgba(255,255,255,0.35); border: 1.5px dashed rgba(255,255,255,0.2); }
  .jlabel { font-size: 10px; color: rgba(255,255,255,0.5); text-align: center; margin-top: 6px; max-width: 80px; line-height: 1.3; }
  .jlabel.current { color: var(--white); font-weight: 500; }
  .jlabel.next    { color: rgba(255,255,255,0.75); }
  .jarrow { width: 24px; height: 2px; background: rgba(255,255,255,0.2); flex-shrink: 0; position: relative; margin-bottom: 20px; }
  .jarrow::after { content:''; position: absolute; right: -5px; top: -4px; border-left: 8px solid rgba(255,255,255,0.2); border-top: 5px solid transparent; border-bottom: 5px solid transparent; }
  .jarrow.active { background: rgba(255,255,255,0.5); }
  .jarrow.active::after { border-left-color: rgba(255,255,255,0.5); }

  /* ── Role summary ── */
  .role-summary {
    background: var(--white); border: 1px solid var(--gray-100);
    border-radius: var(--radius-sm); padding: 14px 16px;
    margin-bottom: 16px; font-size: 13.5px; color: var(--gray-600); line-height: 1.6;
    border-left: 3px solid var(--blue);
  }
  .role-summary strong { color: var(--blue-dark); font-weight: 500; display: block; margin-bottom: 3px; }

  /* ── Legend ── */
  .legend {
    display: flex; gap: 16px; flex-wrap: wrap; margin-bottom: 14px;
    padding-bottom: 14px; border-bottom: 1px solid var(--gray-100);
  }
  .leg { display: flex; align-items: center; gap: 6px; font-size: 11.5px; color: var(--gray-600); }
  .ldot { width: 10px; height: 10px; border-radius: 3px; flex-shrink: 0; }

  /* ── Comparison ── */
  .comparison { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .col-header { padding: 10px 14px; border-radius: var(--radius-sm) var(--radius-sm) 0 0; font-size: 11px; font-weight: 500; text-transform: uppercase; letter-spacing: .05em; }
  .col-now  { background: var(--blue); color: var(--white); }
  .col-next { background: var(--blue-pale); color: var(--blue-dark); }
  .col-body { border: 1px solid var(--gray-100); border-top: none; border-radius: 0 0 var(--radius-sm) var(--radius-sm); background: var(--white); }
  .comp-row { padding: 11px 14px; border-bottom: 1px solid var(--gray-100); }
  .comp-row:last-child { border-bottom: none; }
  .comp-name { font-size: 12.5px; font-weight: 500; color: var(--gray-900); margin-bottom: 4px; display: flex; align-items: center; gap: 6px; flex-wrap: wrap; }
  .comp-desc { font-size: 12px; color: var(--gray-600); line-height: 1.55; }
  .comp-na   { font-size: 12px; color: var(--gray-400); font-style: italic; }
  .badge { font-size: 10px; padding: 2px 7px; border-radius: 10px; font-weight: 400; white-space: nowrap; }
  .b-u   { background: var(--blue-bg); color: var(--blue-dark); }
  .b-r   { background: #EDF4FF; color: var(--blue-darker); border: 1px solid var(--blue-pale); }
  .b-s   { background: #FFF8ED; color: #7A4F00; }
  .b-new { background: var(--gold-light); color: #5C3A00; font-weight: 500; }

  @media (max-width: 700px) {
    .comparison { grid-template-columns: 1fr; }
    .header-inner { padding: 14px 20px; }
    .container { padding: 16px 16px 40px; }
    .gate-card { padding: 32px 24px 28px; }
  }
</style>
</head>
<body>

<div id="gate">
  <div class="gate-bg">
    <div class="gate-blob gb1"></div>
    <div class="gate-blob gb2"></div>
    <div class="gate-blob gb3"></div>
    <svg class="gate-deco-tl" width="48" height="48" viewBox="0 0 48 48" fill="none">
      <path d="M24 3 L26 22 L45 24 L26 26 L24 45 L22 26 L3 24 L22 22 Z" stroke="...
