<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>数学問題データベース</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+JP:wght@400;600&family=DM+Mono:wght@400;500&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0e0f11;
  --bg2: #151619;
  --bg3: #1c1d21;
  --border: rgba(255,255,255,0.07);
  --border2: rgba(255,255,255,0.13);
  --text: #e8e6e0;
  --text2: #9a9890;
  --text3: #5c5a56;
  --accent: #d4a843;
  --accent2: #b8913a;
  --green: #4caf7d;
  --red: #e05c5c;
  --blue: #5b9cf6;
  --purple: #9b7fe8;
  --radius: 10px;
  --font-body: 'Outfit', sans-serif;
  --font-serif: 'Noto Serif JP', serif;
  --font-mono: 'DM Mono', monospace;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body { background: var(--bg); color: var(--text); font-family: var(--font-body); font-size: 14px; line-height: 1.6; min-height: 100vh; }

/* NAV */
.nav { display: flex; align-items: center; justify-content: space-between; padding: 14px 24px; border-bottom: 1px solid var(--border); background: var(--bg); position: sticky; top: 0; z-index: 100; }
.nav-logo { font-family: var(--font-serif); font-size: 16px; color: var(--accent); letter-spacing: 0.04em; }
.nav-tabs { display: flex; gap: 4px; }
.nav-tab { padding: 6px 14px; border-radius: 6px; border: none; background: transparent; color: var(--text2); font-family: var(--font-body); font-size: 13px; cursor: pointer; transition: all 0.15s; }
.nav-tab:hover { color: var(--text); background: var(--bg3); }
.nav-tab.active { background: var(--bg3); color: var(--accent); border: 1px solid var(--border2); }
.nav-right { display: flex; align-items: center; gap: 10px; }
.user-badge { padding: 4px 12px; border-radius: 20px; background: var(--bg3); border: 1px solid var(--border2); font-size: 12px; color: var(--text2); cursor: pointer; transition: all 0.15s; }
.user-badge:hover { border-color: var(--accent); color: var(--accent); }

/* LAYOUT */
.main { max-width: 980px; margin: 0 auto; padding: 28px 24px; }
.page { display: none; }
.page.active { display: block; }

/* CARDS */
.card { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; margin-bottom: 14px; }
.card-title { font-size: 13px; font-weight: 500; color: var(--text2); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 14px; }

/* STAT ROW */
.stat-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 24px; }
.stat-card { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px; }
.stat-num { font-size: 28px; font-weight: 600; color: var(--text); font-family: var(--font-mono); }
.stat-label { font-size: 12px; color: var(--text3); margin-top: 4px; }
.stat-card.accent .stat-num { color: var(--accent); }
.stat-card.green .stat-num { color: var(--green); }
.stat-card.blue .stat-num { color: var(--blue); }

/* PROBLEM LIST */
.problem-list { display: flex; flex-direction: column; gap: 10px; }
.problem-row { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px 18px; cursor: pointer; transition: all 0.15s; display: flex; align-items: center; gap: 14px; }
.problem-row:hover { border-color: var(--border2); background: var(--bg3); }
.problem-row.selected { border-color: var(--accent); }
.prob-id { font-family: var(--font-mono); font-size: 11px; color: var(--text3); min-width: 42px; }
.prob-main { flex: 1; }
.prob-title { font-size: 14px; color: var(--text); margin-bottom: 4px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 380px; }
.prob-meta { display: flex; gap: 8px; align-items: center; }
.tag { padding: 2px 8px; border-radius: 4px; font-size: 11px; font-weight: 500; }
.tag-field { background: rgba(91,156,246,0.12); color: var(--blue); }
.tag-level { background: rgba(212,168,67,0.12); color: var(--accent); }
.tag-hard { background: rgba(224,92,92,0.12); color: var(--red); }
.tag-std  { background: rgba(155,127,232,0.12); color: var(--purple); }
.tag-easy { background: rgba(76,175,125,0.12); color: var(--green); }
.prob-scores { display: flex; flex-direction: column; align-items: flex-end; gap: 4px; min-width: 120px; }
.score-bar-wrap { display: flex; align-items: center; gap: 6px; font-size: 11px; color: var(--text3); }
.score-bar { width: 60px; height: 4px; background: var(--bg3); border-radius: 2px; overflow: hidden; }
.score-fill { height: 100%; border-radius: 2px; transition: width 0.4s; }
.score-fill.diff { background: var(--red); }
.score-fill.time { background: var(--blue); }
.vote-count { font-size: 11px; color: var(--text3); }

/* DETAIL PANEL */
.detail-panel { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--radius); padding: 24px; }
.detail-header { display: flex; align-items: flex-start; justify-content: space-between; margin-bottom: 20px; gap: 16px; }
.detail-tags { display: flex; gap: 6px; flex-wrap: wrap; margin-bottom: 12px; }
.detail-title { font-family: var(--font-serif); font-size: 17px; color: var(--text); line-height: 1.6; }
.detail-body { background: var(--bg3); border-radius: 8px; padding: 16px; font-family: var(--font-serif); font-size: 14px; line-height: 1.9; color: var(--text); margin-bottom: 16px; }
.detail-answer { background: rgba(76,175,125,0.06); border: 1px solid rgba(76,175,125,0.2); border-radius: 8px; padding: 14px; font-family: var(--font-mono); font-size: 13px; color: var(--green); margin-bottom: 16px; }
.section-label { font-size: 12px; font-weight: 500; color: var(--text3); text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 10px; }

/* RATING WIDGET */
.rating-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 16px; }
.rating-box { background: var(--bg3); border: 1px solid var(--border); border-radius: 8px; padding: 14px; }
.rating-title { font-size: 12px; color: var(--text2); margin-bottom: 10px; }
.stars { display: flex; gap: 4px; }
.star { width: 28px; height: 28px; border-radius: 6px; border: 1px solid var(--border2); background: var(--bg2); color: var(--text3); font-size: 14px; cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all 0.12s; }
.star:hover, .star.on { background: rgba(212,168,67,0.15); border-color: var(--accent); color: var(--accent); }
.time-input-wrap { display: flex; align-items: center; gap: 8px; margin-top: 6px; }
.time-input { background: var(--bg2); border: 1px solid var(--border2); border-radius: 6px; color: var(--text); font-family: var(--font-mono); font-size: 14px; padding: 6px 10px; width: 80px; text-align: center; }
.time-input:focus { outline: none; border-color: var(--accent); }
.time-unit { font-size: 12px; color: var(--text3); }

/* AGGREGATE DISPLAY */
.agg-row { display: flex; gap: 10px; align-items: center; margin-bottom: 8px; }
.agg-label { font-size: 12px; color: var(--text2); min-width: 80px; }
.agg-bar { flex: 1; height: 6px; background: var(--bg3); border-radius: 3px; overflow: hidden; }
.agg-fill { height: 100%; border-radius: 3px; }
.agg-val { font-family: var(--font-mono); font-size: 12px; color: var(--text2); min-width: 36px; text-align: right; }
.confidence-badge { display: inline-flex; align-items: center; gap: 6px; padding: 4px 10px; border-radius: 20px; font-size: 11px; margin-bottom: 12px; }
.confidence-badge.high { background: rgba(76,175,125,0.1); color: var(--green); border: 1px solid rgba(76,175,125,0.2); }
.confidence-badge.mid  { background: rgba(212,168,67,0.1); color: var(--accent); border: 1px solid rgba(212,168,67,0.2); }
.confidence-badge.low  { background: rgba(155,127,232,0.1); color: var(--purple); border: 1px solid rgba(155,127,232,0.2); }
.dot { width: 6px; height: 6px; border-radius: 50%; background: currentColor; }

/* AI FEEDBACK BLOCK */
.ai-block { background: rgba(212,168,67,0.05); border: 1px solid rgba(212,168,67,0.15); border-radius: 8px; padding: 14px; font-size: 13px; color: var(--text2); line-height: 1.7; }
.ai-label { font-size: 11px; font-weight: 500; color: var(--accent); text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 8px; display: flex; align-items: center; gap: 6px; }
.ai-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); animation: pulse 2s infinite; }
@keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.4} }

/* FORM */
.form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 14px; }
.form-field { display: flex; flex-direction: column; gap: 6px; }
.form-field.full { grid-column: 1/-1; }
.form-label { font-size: 12px; color: var(--text2); font-weight: 500; }
.form-input, .form-select, .form-textarea { background: var(--bg3); border: 1px solid var(--border2); border-radius: 6px; color: var(--text); font-family: var(--font-body); font-size: 14px; padding: 8px 12px; width: 100%; transition: border-color 0.15s; }
.form-input:focus, .form-select:focus, .form-textarea:focus { outline: none; border-color: var(--accent); }
.form-textarea { resize: vertical; min-height: 90px; font-family: var(--font-serif); }
.form-select option { background: var(--bg3); }

/* BUTTONS */
.btn { padding: 9px 18px; border-radius: 7px; border: none; font-family: var(--font-body); font-size: 13px; font-weight: 500; cursor: pointer; transition: all 0.15s; }
.btn-primary { background: var(--accent); color: #0e0f11; }
.btn-primary:hover { background: var(--accent2); }
.btn-secondary { background: var(--bg3); color: var(--text2); border: 1px solid var(--border2); }
.btn-secondary:hover { color: var(--text); border-color: var(--border2); background: #222327; }
.btn-ghost { background: transparent; color: var(--text3); border: 1px solid var(--border); }
.btn-ghost:hover { color: var(--text2); border-color: var(--border2); }
.btn-danger { background: rgba(224,92,92,0.1); color: var(--red); border: 1px solid rgba(224,92,92,0.2); }
.btn-danger:hover { background: rgba(224,92,92,0.18); }
.btn-row { display: flex; gap: 8px; align-items: center; }

/* FEEDBACK HISTORY */
.feedback-list { display: flex; flex-direction: column; gap: 8px; max-height: 240px; overflow-y: auto; }
.feedback-item { background: var(--bg3); border-radius: 6px; padding: 10px 12px; display: flex; justify-content: space-between; align-items: center; font-size: 12px; }
.feedback-user { color: var(--text2); font-weight: 500; }
.feedback-vals { display: flex; gap: 10px; color: var(--text3); }
.feedback-val { display: flex; align-items: center; gap: 4px; }

/* REGISTER PAGE */
.reg-card { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--radius); padding: 24px; }
.reg-title { font-family: var(--font-serif); font-size: 18px; color: var(--text); margin-bottom: 20px; }

/* USER SELECTOR */
.user-modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 200; align-items: center; justify-content: center; }
.user-modal.open { display: flex; }
.user-modal-box { background: var(--bg2); border: 1px solid var(--border2); border-radius: var(--radius); padding: 24px; width: 320px; }
.user-modal-title { font-size: 15px; font-weight: 500; color: var(--text); margin-bottom: 16px; }
.user-list { display: flex; flex-direction: column; gap: 8px; }
.user-opt { padding: 10px 14px; border-radius: 7px; border: 1px solid var(--border); background: var(--bg3); color: var(--text2); cursor: pointer; transition: all 0.15s; font-size: 13px; }
.user-opt:hover { border-color: var(--accent); color: var(--accent); }
.user-opt.active { border-color: var(--accent); color: var(--accent); background: rgba(212,168,67,0.08); }

/* TOAST */
.toast { position: fixed; bottom: 24px; right: 24px; background: var(--bg3); border: 1px solid var(--border2); border-radius: 8px; padding: 12px 18px; font-size: 13px; color: var(--text); z-index: 300; opacity: 0; transform: translateY(10px); transition: all 0.25s; pointer-events: none; }
.toast.show { opacity: 1; transform: translateY(0); }
.toast.success { border-color: rgba(76,175,125,0.4); color: var(--green); }

/* ANALYTICS */
.analytics-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 14px; }
.dist-chart { display: flex; flex-direction: column; gap: 6px; }
.dist-row { display: flex; align-items: center; gap: 8px; font-size: 12px; }
.dist-label { min-width: 50px; color: var(--text2); }
.dist-bar-wrap { flex: 1; height: 16px; background: var(--bg3); border-radius: 4px; overflow: hidden; }
.dist-bar { height: 100%; border-radius: 4px; transition: width 0.5s; }
.dist-count { min-width: 28px; text-align: right; color: var(--text3); font-family: var(--font-mono); font-size: 11px; }
.empty-state { text-align: center; padding: 40px 20px; color: var(--text3); font-size: 13px; }
.empty-icon { font-size: 32px; margin-bottom: 10px; opacity: 0.4; }

/* SCROLLBAR */
::-webkit-scrollbar { width: 4px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 2px; }

/* GENERATE PAGE */
.sublevel-btn {
  padding: 8px 6px; text-align: center; border-radius: var(--radius);
  border: 1px solid var(--border2); font-size: 13px; cursor: pointer;
  background: var(--bg3); color: var(--text2); transition: all 0.15s;
  font-family: var(--font-body);
}
.sublevel-btn.easy.on  { background: rgba(76,175,125,0.12); color: var(--green); border-color: rgba(76,175,125,0.3); }
.sublevel-btn.std.on   { background: rgba(212,168,67,0.12); color: var(--accent); border-color: rgba(212,168,67,0.3); }
.sublevel-btn.hard.on  { background: rgba(224,92,92,0.12); color: var(--red); border-color: rgba(224,92,92,0.3); }
.chip {
  padding: 5px 11px; border-radius: 20px; border: 1px solid var(--border2);
  font-size: 12px; color: var(--text3); cursor: pointer;
  background: var(--bg3); transition: all 0.15s; user-select: none;
}
.chip.on { background: rgba(91,156,246,0.12); color: var(--blue); border-color: rgba(91,156,246,0.3); }
.gen-output { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--radius); padding: 22px; }
.gen-output h3 { font-family: var(--font-serif); font-size: 16px; color: var(--text); margin-bottom:4px; }
.gen-output .q-block { border-bottom: 1px solid var(--border); padding: 16px 0; }
.gen-output .q-block:last-child { border-bottom: none; }
.gen-output .q-num { font-size:11px;font-weight:500;color:var(--accent);text-transform:uppercase;letter-spacing:.06em;margin-bottom:6px; }
.gen-output .q-body { font-family: var(--font-serif); font-size:14px;color:var(--text);line-height:1.8;margin-bottom:8px; }
.gen-output .q-answer { background:rgba(76,175,125,0.06);border:1px solid rgba(76,175,125,0.18);border-radius:6px;padding:10px 12px;font-size:13px;color:var(--green);font-family:var(--font-mono); }
.gen-loading { display:flex;flex-direction:column;align-items:center;justify-content:center;padding:60px 20px;gap:14px; }
.gen-spinner { width:28px;height:28px;border:2px solid var(--border2);border-top-color:var(--accent);border-radius:50%;animation:spin .7s linear infinite; }
@keyframes spin { to { transform:rotate(360deg); } }
.gen-loading-text { font-size:13px;color:var(--text3); }

/* WEAKNESS PAGE */
.wk-user-btn { padding:9px 14px;border-radius:7px;border:1px solid var(--border);background:var(--bg3);color:var(--text2);cursor:pointer;transition:all .15s;font-size:13px;text-align:left;font-family:var(--font-body); }
.wk-user-btn:hover { border-color:var(--border2);color:var(--text); }
.wk-user-btn.active { border-color:var(--accent);color:var(--accent);background:rgba(212,168,67,0.08); }
.wk-field-row { display:flex;align-items:center;gap:10px;padding:10px 0;border-bottom:1px solid var(--border); }
.wk-field-row:last-child { border-bottom:none; }
.wk-field-name { font-size:13px;color:var(--text);min-width:110px; }
.wk-bar-wrap { flex:1;height:8px;background:var(--bg3);border-radius:4px;overflow:hidden; }
.wk-bar { height:100%;border-radius:4px;transition:width .5s; }
.wk-stat { font-size:12px;font-family:var(--font-mono);min-width:54px;text-align:right; }
.wk-badge { display:inline-flex;align-items:center;gap:5px;padding:3px 10px;border-radius:20px;font-size:11px;font-weight:500; }
.wk-badge.weak   { background:rgba(224,92,92,0.12);color:var(--red);border:1px solid rgba(224,92,92,0.25); }
.wk-badge.mid    { background:rgba(212,168,67,0.12);color:var(--accent);border:1px solid rgba(212,168,67,0.25); }
.wk-badge.strong { background:rgba(76,175,125,0.12);color:var(--green);border:1px solid rgba(76,175,125,0.25); }
.answer-hist-row { display:flex;align-items:center;gap:8px;padding:6px 0;border-bottom:1px solid var(--border);font-size:12px; }
.answer-hist-row:last-child { border-bottom:none; }
.ans-result { width:20px;height:20px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:500;flex-shrink:0; }
.ans-result.ok  { background:rgba(76,175,125,0.15);color:var(--green); }
.ans-result.ng  { background:rgba(224,92,92,0.15);color:var(--red); }

</style>
</head>
<body>

<!-- NAV -->
<nav class="nav">
  <div class="nav-logo">数学問題 DB</div>
  <div class="nav-tabs">
    <button class="nav-tab active" onclick="showPage('dashboard')">ダッシュボード</button>
    <button class="nav-tab" onclick="showPage('problems')">問題一覧</button>
    <button class="nav-tab" onclick="showPage('generate')">試験生成</button>
    <button class="nav-tab" onclick="showPage('weakness')">弱点分析</button>
    <button class="nav-tab" onclick="showPage('register')">問題登録</button>
    <button class="nav-tab" onclick="showPage('analytics')">分析</button>
  </div>
  <div class="nav-right">
    <span id="userBadge" class="user-badge" onclick="openUserModal()">👤 ゲスト</span>
  </div>
</nav>

<!-- USER MODAL -->
<div class="user-modal" id="userModal">
  <div class="user-modal-box">
    <div class="user-modal-title">ユーザーを選択</div>
    <div class="user-list" id="userList"></div>
    <div style="margin-top:12px; display:flex; gap:8px;">
      <input class="form-input" id="newUserInput" placeholder="新しいユーザー名" style="flex:1;">
      <button class="btn btn-primary" onclick="addUser()">追加</button>
    </div>
    <div style="margin-top:10px; text-align:right;">
      <button class="btn btn-ghost" onclick="closeUserModal()">閉じる</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<div class="main">

  <!-- ===== DASHBOARD ===== -->
  <div class="page active" id="page-dashboard">
    <div class="stat-row" id="statRow"></div>
    <div style="display:grid; grid-template-columns:1fr 1fr; gap:14px;">
      <div>
        <div class="card-title">最近登録された問題</div>
        <div id="recentProblems" class="problem-list"></div>
      </div>
      <div>
        <div class="card-title">評価待ち問題（データ不足）</div>
        <div id="needsRating" class="problem-list"></div>
      </div>
    </div>
  </div>

  <!-- ===== PROBLEMS ===== -->
  <div class="page" id="page-problems">
    <div style="display:flex; gap:10px; margin-bottom:16px; align-items:center;">
      <input class="form-input" id="searchInput" placeholder="問題を検索..." style="flex:1;" oninput="renderProblems()">
      <select class="form-select" id="filterField" style="width:140px;" onchange="renderProblems()">
        <option value="">すべての分野</option>
        <option>代数・方程式</option><option>関数・グラフ</option><option>図形・幾何</option>
        <option>確率・統計</option><option>微分・積分</option><option>数列・級数</option>
        <option>ベクトル・行列</option><option>三角関数</option>
      </select>
      <select class="form-select" id="filterLevel" style="width:120px;" onchange="renderProblems()">
        <option value="">すべての難易度</option>
        <option>小学生</option><option>中学生</option><option>高校生</option><option>大学受験</option><option>大学生</option>
      </select>
    </div>
    <div style="display:grid; grid-template-columns:1fr 380px; gap:16px;">
      <div id="problemList" class="problem-list"></div>
      <div id="detailPanel"><div class="empty-state"><div class="empty-icon">📋</div>問題を選択してください</div></div>
    </div>
  </div>

  <!-- ===== REGISTER ===== -->
  <div class="page" id="page-register">
    <div class="reg-card">
      <div class="reg-title">新しい問題を登録</div>
      <div class="form-grid">
        <div class="form-field">
          <label class="form-label">学校段階</label>
          <select class="form-select" id="reg-level">
            <option>小学生</option><option>中学生</option><option>高校生</option>
            <option selected>大学受験</option><option>大学生</option>
          </select>
        </div>
        <div class="form-field">
          <label class="form-label">サブレベル</label>
          <select class="form-select" id="reg-sublevel">
            <option>基礎</option><option>標準</option><option selected>難問</option>
          </select>
        </div>
        <div class="form-field">
          <label class="form-label">分野</label>
          <select class="form-select" id="reg-field">
            <option>代数・方程式</option><option>関数・グラフ</option><option>図形・幾何</option>
            <option>確率・統計</option><option>微分・積分</option><option>数列・級数</option>
            <option>ベクトル・行列</option><option>三角関数</option>
          </select>
        </div>
        <div class="form-field">
          <label class="form-label">問題形式</label>
          <select class="form-select" id="reg-type">
            <option>記述式</option><option>選択式</option><option>計算ドリル</option>
          </select>
        </div>
        <div class="form-field full">
          <label class="form-label">問題文</label>
          <textarea class="form-textarea" id="reg-body" placeholder="問題文を入力してください..." rows="4"></textarea>
        </div>
        <div class="form-field full">
          <label class="form-label">解答・解説</label>
          <textarea class="form-textarea" id="reg-answer" placeholder="解答と解説を入力してください..." rows="3"></textarea>
        </div>
        <div class="form-field">
          <label class="form-label">登録者の推定難易度（1〜5）</label>
          <div class="stars" id="reg-stars"></div>
        </div>
        <div class="form-field">
          <label class="form-label">登録者の推定所要時間（分）</label>
          <div class="time-input-wrap">
            <input class="time-input" id="reg-time" type="number" min="1" max="120" value="10">
            <span class="time-unit">分</span>
          </div>
        </div>
        <div class="form-field full">
          <label class="form-label">出典（任意）</label>
          <input class="form-input" id="reg-source" placeholder="例：東京大学 2023年 前期">
        </div>
      </div>
      <div class="btn-row">
        <button class="btn btn-primary" onclick="registerProblem()">問題を登録する</button>
        <button class="btn btn-secondary" onclick="clearForm()">クリア</button>
      </div>
    </div>
  </div>

  <!-- ===== GENERATE ===== -->
  <div class="page" id="page-generate">
    <div style="display:grid;grid-template-columns:340px 1fr;gap:16px;align-items:start;">

      <!-- LEFT: 設定パネル -->
      <div>
        <div class="reg-card" style="margin-bottom:14px;">
          <div class="reg-title" style="font-size:15px;margin-bottom:16px;">試験生成設定</div>
          <div class="form-field" style="margin-bottom:12px;">
            <label class="form-label">学校段階</label>
            <select class="form-select" id="gen-level">
              <option>小学生</option><option>中学生</option><option selected>高校生</option>
              <option>大学受験</option><option>大学生</option>
            </select>
          </div>
          <div class="form-field" style="margin-bottom:12px;">
            <label class="form-label">難易度レベル</label>
            <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:6px;" id="gen-sublevel-group">
              <button class="sublevel-btn easy" onclick="selectGenSub(this,'基礎')">基礎</button>
              <button class="sublevel-btn std on" onclick="selectGenSub(this,'標準')">標準</button>
              <button class="sublevel-btn hard" onclick="selectGenSub(this,'難問')">難問</button>
            </div>
          </div>
          <div class="form-field" style="margin-bottom:12px;">
            <label class="form-label">分野（複数選択可）</label>
            <div style="display:flex;flex-wrap:wrap;gap:6px;" id="gen-fields">
              <span class="chip on" data-v="代数・方程式">代数・方程式</span>
              <span class="chip on" data-v="関数・グラフ">関数・グラフ</span>
              <span class="chip" data-v="図形・幾何">図形・幾何</span>
              <span class="chip" data-v="確率・統計">確率・統計</span>
              <span class="chip" data-v="微分・積分">微分・積分</span>
              <span class="chip" data-v="数列・級数">数列・級数</span>
              <span class="chip" data-v="ベクトル・行列">ベクトル・行列</span>
              <span class="chip" data-v="三角関数">三角関数</span>
            </div>
          </div>
          <div class="form-field" style="margin-bottom:12px;">
            <label class="form-label">問題形式</label>
            <select class="form-select" id="gen-type">
              <option>記述式</option><option>選択式（4択）</option><option>混合</option><option>計算ドリル</option>
            </select>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;">
            <div class="form-field">
              <label class="form-label">制限時間（分）</label>
              <input class="form-input" id="gen-time" type="number" min="5" max="180" value="30">
            </div>
            <div class="form-field">
              <label class="form-label">問題数</label>
              <input class="form-input" id="gen-count" type="number" min="1" max="20" value="5">
            </div>
          </div>
          <!-- DB参照オプション -->
          <div style="background:var(--bg3);border-radius:8px;padding:12px;margin-bottom:14px;">
            <div style="font-size:12px;font-weight:500;color:var(--accent);margin-bottom:8px;display:flex;align-items:center;gap:6px;">
              <div style="width:6px;height:6px;border-radius:50%;background:var(--accent);animation:pulse 2s infinite;"></div>
              DBフィードバックをAIに反映
            </div>
            <div id="gen-db-summary" style="font-size:12px;color:var(--text3);line-height:1.7;"></div>
          </div>
          <button class="btn btn-primary" style="width:100%;padding:11px;" onclick="generateExam()">試験問題を生成する ↗</button>
        </div>
      </div>

      <!-- RIGHT: 結果エリア -->
      <div id="gen-result">
        <div class="empty-state" style="padding:60px 20px;">
          <div class="empty-icon">✏️</div>
          <div style="margin-top:8px;">左のパネルで設定して「試験問題を生成する」を押してください</div>
          <div style="margin-top:6px;font-size:12px;">DBに蓄積された評価データをAIへのフィードバックとして活用します</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ===== ANALYTICS ===== -->
  <div class="page" id="page-analytics">
    <div class="analytics-grid">
      <div class="card">
        <div class="card-title">分野別問題数</div>
        <div id="chartField" class="dist-chart"></div>
      </div>
      <div class="card">
        <div class="card-title">難易度分布（ユーザー評価平均）</div>
        <div id="chartDiff" class="dist-chart"></div>
      </div>
      <div class="card">
        <div class="card-title">評価数 TOP 問題</div>
        <div id="topRated" class="dist-chart"></div>
      </div>
      <div class="card">
        <div class="card-title">登録者 vs ユーザー難易度乖離</div>
        <div id="chartGap" class="dist-chart"></div>
      </div>
    </div>
  </div>

  <!-- ===== WEAKNESS ===== -->
  <div class="page" id="page-weakness">
    <div style="display:grid;grid-template-columns:300px 1fr;gap:16px;align-items:start;">
      <!-- LEFT -->
      <div>
        <div class="reg-card" style="margin-bottom:14px;">
          <div style="font-size:13px;font-weight:500;color:var(--text2);margin-bottom:12px;">分析対象ユーザー</div>
          <div id="wk-user-list" style="display:flex;flex-direction:column;gap:6px;margin-bottom:14px;"></div>
          <button class="btn btn-primary" style="width:100%;padding:10px;" onclick="generateReviewTest()">弱点復習テストを生成 ↗</button>
        </div>
        <div class="reg-card">
          <div style="font-size:13px;font-weight:500;color:var(--text2);margin-bottom:12px;">解答を記録する</div>
          <div class="form-field" style="margin-bottom:10px;">
            <label class="form-label">問題ID</label>
            <input class="form-input" id="rec-prob-id" placeholder="例: P001">
          </div>
          <div class="form-field" style="margin-bottom:10px;">
            <label class="form-label">結果</label>
            <div style="display:flex;gap:8px;">
              <button class="btn" id="rec-correct-btn" style="flex:1;background:rgba(76,175,125,0.12);color:var(--green);border:1px solid rgba(76,175,125,0.3);" onclick="setRecResult(true)">正解 ○</button>
              <button class="btn" id="rec-wrong-btn"   style="flex:1;background:var(--bg3);color:var(--text3);border:1px solid var(--border2);" onclick="setRecResult(false)">不正解 ✕</button>
            </div>
          </div>
          <div class="form-field" style="margin-bottom:10px;">
            <label class="form-label">実際の所要時間（分）</label>
            <input class="form-input" id="rec-time" type="number" min="1" max="180" placeholder="分">
          </div>
          <button class="btn btn-secondary" style="width:100%;" onclick="recordAnswer()">記録する</button>
        </div>
      </div>
      <!-- RIGHT -->
      <div id="wk-panel">
        <div class="empty-state" style="padding:60px 20px;">
          <div class="empty-icon">📊</div>
          <div style="margin-top:8px;">左でユーザーを選択してください</div>
          <div style="font-size:12px;margin-top:4px;">解答履歴から弱点分野を自動検出します</div>
        </div>
      </div>
    </div>
  </div>


</div>

<script>
const STORAGE_KEY = 'mathdb_v3';
let state = {
  problems: [], ratings: [], answers: [],
  users: ['田中','鈴木','山田','ゲスト'],
  currentUser: 'ゲスト', regStars: 3
};

function save() { try { localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); } catch(e){} }
function load() {
  try {
    const d = localStorage.getItem(STORAGE_KEY);
    if (d) Object.assign(state, JSON.parse(d));
  } catch(e){}
  if (!state.problems) state.problems = [];
  if (!state.ratings)  state.ratings  = [];
  if (!state.answers)  state.answers  = [];
  if (!state.users)    state.users    = ['田中','鈴木','山田','ゲスト'];
  if (!state.currentUser) state.currentUser = 'ゲスト';
}

/* ── SEED ── */
const SEED_PROBLEMS = [
  {id:'P001',level:'高校生',sublevel:'難問',field:'微分・積分',type:'記述式',
   body:'定積分 ∫₀^π x·sin(x) dx を求めよ。',answer:'部分積分より π',
   source:'東京大学 2019',regDiff:4,regTime:8,createdBy:'田中',createdAt:Date.now()-864e5*5},
  {id:'P002',level:'大学受験',sublevel:'標準',field:'数列・級数',type:'記述式',
   body:'a₁=1, aₙ₊₁ = aₙ/(1+n·aₙ) で定まる数列の一般項を求めよ。',answer:'aₙ = 2/(n²−n+2)',
   source:'京都大学 2021',regDiff:4,regTime:12,createdBy:'鈴木',createdAt:Date.now()-864e5*3},
  {id:'P003',level:'大学受験',sublevel:'難問',field:'ベクトル・行列',type:'記述式',
   body:'行列 A=[[3,1],[-2,0]] について Aⁿ を求めよ。',answer:'対角化より導出',
   source:'一橋大学 2022',regDiff:5,regTime:15,createdBy:'山田',createdAt:Date.now()-864e5*1},
  {id:'P004',level:'高校生',sublevel:'基礎',field:'三角関数',type:'選択式',
   body:'sin(π/6) の値はどれか。 A: 1/2  B: √2/2  C: √3/2  D: 1',answer:'A: 1/2',
   source:'',regDiff:1,regTime:2,createdBy:'田中',createdAt:Date.now()-864e5*7},
  {id:'P005',level:'大学受験',sublevel:'難問',field:'代数・方程式',type:'記述式',
   body:'p,q を整数とする。方程式 x³+px+q=0 が有理数解をもつなら、それは整数であることを示せ。',
   answer:'有理根定理による証明',source:'東京大学 2020',regDiff:5,regTime:20,
   createdBy:'鈴木',createdAt:Date.now()-864e5*2}
];
const SEED_RATINGS = [
  {probId:'P001',user:'鈴木',diff:4,time:9, at:Date.now()-864e5*4},
  {probId:'P001',user:'山田',diff:3,time:11,at:Date.now()-864e5*3},
  {probId:'P001',user:'田中',diff:5,time:7, at:Date.now()-864e5*2},
  {probId:'P002',user:'田中',diff:3,time:13,at:Date.now()-864e5*2},
  {probId:'P002',user:'山田',diff:4,time:14,at:Date.now()-864e5*1},
  {probId:'P003',user:'田中',diff:5,time:18,at:Date.now()-864e5*1},
  {probId:'P004',user:'鈴木',diff:1,time:2, at:Date.now()-864e5*6},
  {probId:'P004',user:'山田',diff:2,time:3, at:Date.now()-864e5*5},
  {probId:'P005',user:'田中',diff:5,time:22,at:Date.now()-864e5*1}
];

function initSeed() {
  if (!state.problems.length) { state.problems = SEED_PROBLEMS; state.ratings = SEED_RATINGS; save(); }
}

/* ── HELPERS ── */
let selectedProbId = null;

function uid() { return 'P' + String(Date.now()).slice(-6); }

function showPage(p) {
  document.querySelectorAll('.page').forEach(function(el){ el.classList.remove('active'); });
  document.querySelectorAll('.nav-tab').forEach(function(el){ el.classList.remove('active'); });
  var pg = document.getElementById('page-' + p);
  if (pg) pg.classList.add('active');
  var labels = {dashboard:'ダッシュ',problems:'問題一覧',generate:'試験生成',weakness:'弱点分析',register:'問題登録',analytics:'分析'};
  var key = labels[p] || '__';
  document.querySelectorAll('.nav-tab').forEach(function(el){
    if (el.textContent.indexOf(key) >= 0) el.classList.add('active');
  });
  if (p === 'dashboard') renderDashboard();
  if (p === 'problems')  renderProblems();
  if (p === 'analytics') renderAnalytics();
  if (p === 'generate')  renderGenDBSummary();
  if (p === 'weakness')  renderWeakness();
}

function toast(msg, type) {
  var t = document.getElementById('toast');
  t.textContent = msg;
  t.className = 'toast show ' + (type || '');
  setTimeout(function(){ t.className = 'toast'; }, 2400);
}

/* ── AGGREGATE ── */
function getAgg(probId) {
  var rs = state.ratings.filter(function(r){ return r.probId === probId; });
  if (!rs.length) return null;
  var prob = state.problems.find(function(p){ return p.id === probId; });
  var n = rs.length;
  var w = Math.min(n / 5, 1);
  var avgDiff = rs.reduce(function(s,r){ return s + r.diff; }, 0) / n;
  var avgTime = rs.reduce(function(s,r){ return s + r.time; }, 0) / n;
  var bd = w * avgDiff + (1 - w) * (prob ? prob.regDiff : 3);
  var bt = w * avgTime + (1 - w) * (prob ? prob.regTime : 10);
  var conf = n >= 5 ? 'high' : n >= 3 ? 'mid' : 'low';
  var confLabel = {high:'高信頼度',mid:'中程度',low:'データ不足'}[conf];
  return {n:n, avgDiff:avgDiff, avgTime:avgTime, blendedDiff:bd, blendedTime:bt, confidence:conf, confLabel:confLabel};
}

function getAIFeedback(prob, agg) {
  if (!agg) return '評価データが不足しています。解答後に難易度と所要時間を評価してください。';
  var diff = agg.blendedDiff - prob.regDiff;
  var fb = [];
  if (Math.abs(diff) < 0.5) {
    fb.push('登録者の難易度設定（' + prob.regDiff + '/5）とユーザー評価（' + agg.blendedDiff.toFixed(1) + '/5）はほぼ一致しています。');
  } else if (diff > 0.5) {
    fb.push('ユーザー評価（' + agg.blendedDiff.toFixed(1) + '/5）が登録者設定より' + Math.abs(diff).toFixed(1) + '高く、実際にはより難しい問題です。');
  } else {
    fb.push('ユーザー評価（' + agg.blendedDiff.toFixed(1) + '/5）が登録者設定より' + Math.abs(diff).toFixed(1) + '低く、取り組みやすい問題です。');
  }
  if (Math.abs(agg.blendedTime - prob.regTime) > 3) {
    var dir = agg.blendedTime > prob.regTime ? '長く' : '短く';
    fb.push('所要時間の平均（' + agg.blendedTime.toFixed(0) + '分）は登録者想定（' + prob.regTime + '分）より' + dir + 'かかっています。');
  }
  if (agg.n >= 3) fb.push(agg.n + '件の評価をもとに統計処理済み。信頼度: ' + agg.confLabel);
  return fb.join(' ');
}

/* ── DASHBOARD ── */
function renderDashboard() {
  var ps = state.problems, rs = state.ratings;
  var ratedIds = rs.reduce(function(acc,r){ if (acc.indexOf(r.probId)<0) acc.push(r.probId); return acc; }, []);
  document.getElementById('statRow').innerHTML =
    '<div class="stat-card accent"><div class="stat-num">' + ps.length + '</div><div class="stat-label">登録問題数</div></div>' +
    '<div class="stat-card green"><div class="stat-num">'  + rs.length + '</div><div class="stat-label">累積評価数</div></div>' +
    '<div class="stat-card blue"><div class="stat-num">'   + ratedIds.length + '</div><div class="stat-label">評価済み問題</div></div>' +
    '<div class="stat-card"><div class="stat-num">'        + (ps.length - ratedIds.length) + '</div><div class="stat-label">未評価問題</div></div>';
  var recent = ps.slice().sort(function(a,b){ return b.createdAt - a.createdAt; }).slice(0,4);
  document.getElementById('recentProblems').innerHTML = recent.map(probRowHTML).join('');
  var needs = ps.filter(function(p){ return rs.filter(function(r){ return r.probId===p.id; }).length < 3; }).slice(0,4);
  document.getElementById('needsRating').innerHTML = needs.length
    ? needs.map(probRowHTML).join('')
    : '<div class="empty-state">すべての問題が十分に評価されています</div>';
}

function probRowHTML(prob) {
  var agg = getAgg(prob.id);
  var bd = agg ? agg.blendedDiff : prob.regDiff;
  var bt = agg ? agg.blendedTime : prob.regTime;
  var n  = agg ? agg.n : 0;
  var sc = {難問:'hard',標準:'std',基礎:'easy'}[prob.sublevel] || 'std';
  var sel = selectedProbId === prob.id ? ' selected' : '';
  return '<div class="problem-row' + sel + '" onclick="selectProblem(\'' + prob.id + '\')">' +
    '<span class="prob-id">' + prob.id + '</span>' +
    '<div class="prob-main">' +
      '<div class="prob-title">' + prob.body.slice(0,60) + (prob.body.length>60?'…':'') + '</div>' +
      '<div class="prob-meta">' +
        '<span class="tag tag-field">'  + prob.field    + '</span>' +
        '<span class="tag tag-level">'  + prob.level    + '</span>' +
        '<span class="tag tag-' + sc + '">' + prob.sublevel + '</span>' +
      '</div>' +
    '</div>' +
    '<div class="prob-scores">' +
      '<div class="score-bar-wrap"><span>難易度</span><div class="score-bar"><div class="score-fill diff" style="width:' + (bd/5*100) + '%"></div></div><span>' + bd.toFixed(1) + '</span></div>' +
      '<div class="score-bar-wrap"><span>時間</span><div class="score-bar"><div class="score-fill time" style="width:' + Math.min(bt/30*100,100) + '%"></div></div><span>' + bt.toFixed(0) + '分</span></div>' +
      '<span class="vote-count">' + n + '件の評価</span>' +
    '</div>' +
  '</div>';
}

/* ── PROBLEMS ── */
function renderProblems() {
  var q     = (document.getElementById('searchInput') ? document.getElementById('searchInput').value : '').toLowerCase();
  var field = document.getElementById('filterField') ? document.getElementById('filterField').value : '';
  var level = document.getElementById('filterLevel') ? document.getElementById('filterLevel').value : '';
  var list  = state.problems.filter(function(p){
    if (field && p.field !== field) return false;
    if (level && p.level !== level) return false;
    if (q && p.body.toLowerCase().indexOf(q) < 0 && p.field.toLowerCase().indexOf(q) < 0) return false;
    return true;
  });
  document.getElementById('problemList').innerHTML = list.length
    ? list.map(probRowHTML).join('')
    : '<div class="empty-state"><div class="empty-icon">🔍</div>条件に合う問題が見つかりません</div>';
  if (selectedProbId) renderDetail(selectedProbId);
}

/* ── DETAIL ── */
var ratingState = {diff:0, time:0};

function selectProblem(id) {
  selectedProbId = id;
  ratingState = {diff:0, time:0};
  document.querySelectorAll('.problem-row').forEach(function(el){ el.classList.remove('selected'); });
  document.querySelectorAll('.problem-row').forEach(function(el){
    var pid = el.querySelector('.prob-id');
    if (pid && pid.textContent === id) el.classList.add('selected');
  });
  renderDetail(id);
  if (!document.getElementById('page-problems').classList.contains('active')) showPage('problems');
}

function renderDetail(id) {
  var prob = state.problems.find(function(p){ return p.id === id; });
  if (!prob) return;
  var agg = getAgg(id);
  var aiFb = getAIFeedback(prob, agg);
  var myRating = state.ratings.find(function(r){ return r.probId===id && r.user===state.currentUser; });
  var sc = {難問:'hard',標準:'std',基礎:'easy'}[prob.sublevel] || 'std';
  var confClass = agg ? agg.confidence : 'low';
  var confLabel = agg ? agg.confLabel  : 'データ不足';

  var histHtml = state.ratings.filter(function(r){ return r.probId===id; }).slice(-8).reverse().map(function(r){
    return '<div class="feedback-item"><span class="feedback-user">' + r.user + '</span>' +
      '<div class="feedback-vals">' +
        '<span class="feedback-val">難易度 ' + '★'.repeat(r.diff) + '</span>' +
        '<span class="feedback-val">⏱ ' + r.time + '分</span>' +
      '</div></div>';
  }).join('');

  var starHtml = '';
  for (var i=0;i<5;i++) {
    starHtml += '<div class="star' + (ratingState.diff>i?' on':'') + '" onclick="setRatingDiff(' + (i+1) + ')" id="dstar' + i + '">★</div>';
  }

  var aggHtml = '';
  if (agg) {
    aggHtml = '<div class="section-label">ユーザー評価集計</div>' +
      '<div class="confidence-badge ' + confClass + '"><div class="dot"></div>' + confLabel + '（' + agg.n + '件）</div>' +
      '<div class="agg-row"><span class="agg-label">難易度</span><div class="agg-bar"><div class="agg-fill" style="width:' + (agg.blendedDiff/5*100) + '%;background:var(--red)"></div></div><span class="agg-val">' + agg.blendedDiff.toFixed(2) + '/5</span></div>' +
      '<div class="agg-row"><span class="agg-label">所要時間</span><div class="agg-bar"><div class="agg-fill" style="width:' + Math.min(agg.blendedTime/30*100,100) + '%;background:var(--blue)"></div></div><span class="agg-val">' + agg.blendedTime.toFixed(0) + '分</span></div>' +
      '<div class="agg-row"><span class="agg-label">登録者想定</span><div class="agg-bar"><div class="agg-fill" style="width:' + (prob.regDiff/5*100) + '%;background:var(--text3)"></div></div><span class="agg-val">' + prob.regDiff + '/5</span></div>' +
      '<div style="margin-top:14px;margin-bottom:6px;" class="section-label">評価履歴</div>' +
      '<div class="feedback-list">' + (histHtml || '<div style="color:var(--text3);font-size:12px;padding:8px;">まだ評価がありません</div>') + '</div>';
  } else {
    aggHtml = '<div style="color:var(--text3);font-size:13px;margin-bottom:14px;">まだ評価データがありません</div>';
  }

  var ratingHtml = '';
  if (myRating) {
    ratingHtml = '<div style="background:rgba(76,175,125,0.06);border:1px solid rgba(76,175,125,0.2);border-radius:8px;padding:12px;font-size:12px;color:var(--green);">' +
      'あなたの評価：難易度 ' + '★'.repeat(myRating.diff) + ' / 所要時間 ' + myRating.time + '分 ' +
      '<button class="btn btn-ghost" style="margin-left:10px;font-size:11px;padding:3px 8px;" onclick="deleteMyRating(\'' + id + '\')">取り消し</button></div>';
  } else {
    ratingHtml = '<div class="section-label">あなたの評価を送る（' + state.currentUser + '）</div>' +
      '<div class="rating-grid">' +
        '<div class="rating-box"><div class="rating-title">難易度（1〜5）</div><div class="stars">' + starHtml + '</div></div>' +
        '<div class="rating-box"><div class="rating-title">所要時間</div>' +
          '<div class="time-input-wrap"><input class="time-input" id="ratingTime" type="number" min="1" max="180" placeholder="分"><span class="time-unit">分</span></div>' +
        '</div>' +
      '</div>' +
      '<div class="btn-row">' +
        '<button class="btn btn-primary" onclick="submitRating(\'' + id + '\')">評価を送信</button>' +
        '<span style="font-size:12px;color:var(--text3);">ユーザー: ' + state.currentUser + '</span>' +
      '</div>';
  }

  var sourceTag = prob.source ? '<span class="tag" style="background:rgba(255,255,255,0.05);color:var(--text3)">' + prob.source + '</span>' : '';
  document.getElementById('detailPanel').innerHTML =
    '<div class="detail-panel">' +
      '<div class="detail-tags">' +
        '<span class="tag tag-field">' + prob.field    + '</span>' +
        '<span class="tag tag-level">' + prob.level    + '</span>' +
        '<span class="tag tag-' + sc + '">' + prob.sublevel + '</span>' +
        '<span class="tag tag-field">' + prob.type     + '</span>' +
        sourceTag +
      '</div>' +
      '<div class="detail-body">' + prob.body + '</div>' +
      '<details style="margin-bottom:16px;"><summary style="font-size:12px;color:var(--text3);cursor:pointer;padding:6px 0;">解答・解説を表示</summary>' +
        '<div class="detail-answer" style="margin-top:8px;">' + prob.answer + '</div></details>' +
      aggHtml +
      '<div class="ai-block" style="margin:16px 0;"><div class="ai-label"><div class="ai-dot"></div>AI フィードバック</div>' + aiFb + '</div>' +
      ratingHtml +
    '</div>';
}

function setRatingDiff(n) {
  ratingState.diff = n;
  for (var i=0;i<5;i++) {
    var el = document.getElementById('dstar'+i);
    if (el) el.className = 'star' + (i < n ? ' on' : '');
  }
}

function submitRating(probId) {
  if (!ratingState.diff) { toast('難易度を選択してください'); return; }
  var timeEl = document.getElementById('ratingTime');
  var time = parseInt(timeEl ? timeEl.value : '0');
  if (!time || time < 1) { toast('所要時間を入力してください'); return; }
  state.ratings.push({probId:probId, user:state.currentUser, diff:ratingState.diff, time:time, at:Date.now()});
  save();
  toast('評価を送信しました！', 'success');
  ratingState = {diff:0, time:0};
  renderDetail(probId);
  renderProblems();
}

function deleteMyRating(probId) {
  state.ratings = state.ratings.filter(function(r){ return !(r.probId===probId && r.user===state.currentUser); });
  save();
  renderDetail(probId);
  renderProblems();
  toast('評価を取り消しました');
}

/* ── REGISTER ── */
function initRegStars() {
  var wrap = document.getElementById('reg-stars');
  if (!wrap) return;
  var h = '';
  for (var i=0;i<5;i++) h += '<div class="star' + (state.regStars>i?' on':'') + '" onclick="setRegStar(' + (i+1) + ')">★</div>';
  wrap.innerHTML = h;
}
function setRegStar(n) { state.regStars = n; initRegStars(); }

function registerProblem() {
  var body   = document.getElementById('reg-body').value.trim();
  var answer = document.getElementById('reg-answer').value.trim();
  if (!body)   { toast('問題文を入力してください'); return; }
  if (!answer) { toast('解答・解説を入力してください'); return; }
  var time = parseInt(document.getElementById('reg-time').value) || 10;
  state.problems.push({
    id: uid(),
    level:    document.getElementById('reg-level').value,
    sublevel: document.getElementById('reg-sublevel').value,
    field:    document.getElementById('reg-field').value,
    type:     document.getElementById('reg-type').value,
    body:body, answer:answer,
    source:   document.getElementById('reg-source').value.trim(),
    regDiff:  state.regStars, regTime:time,
    createdBy:state.currentUser, createdAt:Date.now()
  });
  save();
  toast('問題を登録しました！', 'success');
  clearForm();
}
function clearForm() {
  ['reg-body','reg-answer','reg-source'].forEach(function(id){
    var el = document.getElementById(id); if(el) el.value='';
  });
  document.getElementById('reg-time').value = 10;
  state.regStars = 3; initRegStars();
}

/* ── ANALYTICS ── */
function renderAnalytics() {
  var ps = state.problems, rs = state.ratings;
  var colors = ['var(--blue)','var(--purple)','var(--green)','var(--accent)','var(--red)'];
  var fields = {};
  ps.forEach(function(p){ fields[p.field] = (fields[p.field]||0)+1; });
  var maxF = Math.max.apply(null, Object.values(fields).concat([1]));
  document.getElementById('chartField').innerHTML = Object.entries(fields)
    .sort(function(a,b){ return b[1]-a[1]; })
    .map(function(e,i){
      return '<div class="dist-row"><span class="dist-label">' + e[0].slice(0,6) + '</span>' +
        '<div class="dist-bar-wrap"><div class="dist-bar" style="width:' + (e[1]/maxF*100) + '%;background:' + colors[i%5] + '"></div></div>' +
        '<span class="dist-count">' + e[1] + '</span></div>';
    }).join('') || '<div class="empty-state">データなし</div>';

  var db = {1:0,2:0,3:0,4:0,5:0};
  rs.forEach(function(r){ db[r.diff]=(db[r.diff]||0)+1; });
  var maxD = Math.max.apply(null, Object.values(db).concat([1]));
  document.getElementById('chartDiff').innerHTML = Object.entries(db).map(function(e){
    return '<div class="dist-row"><span class="dist-label">' + '★'.repeat(+e[0]) + '</span>' +
      '<div class="dist-bar-wrap"><div class="dist-bar" style="width:' + (e[1]/maxD*100) + '%;background:var(--red)"></div></div>' +
      '<span class="dist-count">' + e[1] + '</span></div>';
  }).join('');

  var rc = {};
  rs.forEach(function(r){ rc[r.probId]=(rc[r.probId]||0)+1; });
  var top = Object.entries(rc).sort(function(a,b){ return b[1]-a[1]; }).slice(0,5);
  var maxR = top.length ? Math.max.apply(null,top.map(function(e){ return e[1]; })) : 1;
  document.getElementById('topRated').innerHTML = top.map(function(e){
    return '<div class="dist-row"><span class="dist-label" style="min-width:40px;">' + e[0] + '</span>' +
      '<div class="dist-bar-wrap"><div class="dist-bar" style="width:' + (e[1]/maxR*100) + '%;background:var(--green)"></div></div>' +
      '<span class="dist-count">' + e[1] + '</span></div>';
  }).join('') || '<div class="empty-state">データなし</div>';

  var gaps = ps.map(function(p){
    var a = getAgg(p.id); if(!a) return null;
    return {id:p.id, gap:a.avgDiff - p.regDiff};
  }).filter(Boolean).sort(function(a,b){ return Math.abs(b.gap)-Math.abs(a.gap); }).slice(0,5);
  var maxG = gaps.length ? Math.max.apply(null,gaps.map(function(g){ return Math.abs(g.gap); })) : 0.1;
  document.getElementById('chartGap').innerHTML = gaps.length ? gaps.map(function(g){
    var col = g.gap > 0 ? 'var(--red)' : 'var(--blue)';
    var sign = g.gap > 0 ? '+' : '';
    return '<div class="dist-row"><span class="dist-label">' + g.id + '</span>' +
      '<div class="dist-bar-wrap"><div class="dist-bar" style="width:' + (Math.abs(g.gap)/maxG*100) + '%;background:' + col + '"></div></div>' +
      '<span class="dist-count" style="color:' + col + '">' + sign + g.gap.toFixed(1) + '</span></div>';
  }).join('') : '<div class="empty-state">評価データが不足しています</div>';
}

/* ── USER MODAL ── */
function openUserModal()  { renderUserList(); document.getElementById('userModal').classList.add('open'); }
function closeUserModal() { document.getElementById('userModal').classList.remove('open'); }
function renderUserList() {
  document.getElementById('userList').innerHTML = state.users.map(function(u){
    return '<div class="user-opt' + (u===state.currentUser?' active':'') + '" onclick="switchUser(\'' + u + '\')">' + u + '</div>';
  }).join('');
}
function switchUser(u) {
  state.currentUser = u;
  document.getElementById('userBadge').textContent = '👤 ' + u;
  ratingState = {diff:0, time:0};
  save(); renderUserList();
  if (selectedProbId) renderDetail(selectedProbId);
  toast(u + 'に切り替えました', 'success');
}
function addUser() {
  var name = document.getElementById('newUserInput').value.trim();
  if (!name) return;
  if (state.users.indexOf(name) >= 0) { toast('そのユーザーはすでに存在します'); return; }
  state.users.push(name);
  state.currentUser = name;
  document.getElementById('userBadge').textContent = '👤 ' + name;
  document.getElementById('newUserInput').value = '';
  save(); renderUserList();
  toast(name + 'を追加しました', 'success');
}

/* ── GENERATE PAGE ── */
var genSubLevel = '標準';

function selectGenSub(btn, val) {
  genSubLevel = val;
  document.querySelectorAll('#gen-sublevel-group .sublevel-btn').forEach(function(b){ b.classList.remove('on'); });
  btn.classList.add('on');
  renderGenDBSummary();
}

function getGenFields() {
  return Array.from(document.querySelectorAll('#gen-fields .chip.on')).map(function(c){ return c.dataset.v; });
}

function renderGenDBSummary() {
  var el = document.getElementById('gen-db-summary');
  if (!el) return;
  var fields = getGenFields();
  var level  = document.getElementById('gen-level') ? document.getElementById('gen-level').value : '';
  var matched = state.problems.filter(function(p){
    return (!fields.length || fields.indexOf(p.field)>=0) &&
           (!level || p.level===level) &&
           (!genSubLevel || p.sublevel===genSubLevel);
  });
  var rated = matched.filter(function(p){ return state.ratings.some(function(r){ return r.probId===p.id; }); });
  if (!matched.length) { el.textContent = '選択条件に合う問題がDBに存在しません。AIが新規生成します。'; return; }
  var aggs = rated.map(function(p){ return getAgg(p.id); }).filter(Boolean);
  var avgD = aggs.length ? (aggs.reduce(function(s,a){ return s+a.blendedDiff; },0)/aggs.length).toFixed(2) : null;
  var avgT = aggs.length ? (aggs.reduce(function(s,a){ return s+a.blendedTime; },0)/aggs.length).toFixed(0) : null;
  el.innerHTML = 'DB内の類似問題: <strong style="color:var(--text)">' + matched.length + '件</strong>（うち評価済み ' + rated.length + '件）<br>' +
    (avgD ? 'ユーザー評価平均難易度: <strong style="color:var(--red)">' + avgD + '/5</strong>　平均所要時間: <strong style="color:var(--blue)">' + avgT + '分</strong><br>' : '') +
    '→ この情報をAIへのフィードバックとして問題生成に反映します';
}

function buildFeedbackContext() {
  var fields  = getGenFields();
  var level   = document.getElementById('gen-level') ? document.getElementById('gen-level').value : '';
  var matched = state.problems.filter(function(p){
    return (!fields.length || fields.indexOf(p.field)>=0) && (!level||p.level===level) && (!genSubLevel||p.sublevel===genSubLevel);
  });
  var aggs = matched.map(function(p){ return {p:p,agg:getAgg(p.id)}; }).filter(function(x){ return x.agg; });
  if (!aggs.length) return '';
  var avgD = (aggs.reduce(function(s,x){ return s+x.agg.blendedDiff; },0)/aggs.length).toFixed(2);
  var avgT = (aggs.reduce(function(s,x){ return s+x.agg.blendedTime; },0)/aggs.length).toFixed(0);
  var gaps = aggs.map(function(x){ return {id:x.p.id,gap:(x.agg.avgDiff-x.p.regDiff).toFixed(1),n:x.agg.n}; })
                 .filter(function(g){ return Math.abs(g.gap) >= 0.5; });
  var ctx = '【DBフィードバック情報】\n類似問題のユーザー評価平均: 難易度 ' + avgD + '/5、所要時間 ' + avgT + '分/問\n';
  if (gaps.length) ctx += '難易度乖離が大きい問題: ' + gaps.map(function(g){ return g.id+'(ズレ'+g.gap+',評価'+g.n+'件)'; }).join('、') + '\n→ 乖離傾向を踏まえ、実際の体感難易度に合わせた問題を生成してください。\n';
  return ctx;
}

var lastGeneratedExam = null;

async function generateExam() {
  var fields = getGenFields();
  if (!fields.length) { toast('分野を1つ以上選択してください'); return; }
  var level   = document.getElementById('gen-level').value;
  var type    = document.getElementById('gen-type').value;
  var minutes = parseInt(document.getElementById('gen-time').value) || 30;
  var count   = parseInt(document.getElementById('gen-count').value) || 5;
  var fbCtx   = buildFeedbackContext();
  var sublevelDesc = {基礎:'典型問題・基本操作',標準:'入試頻出・応用問題',難問:'難関校・高度な思考力問題'}[genSubLevel] || '';
  var prompt = '数学の試験問題作成の専門家として、以下の条件で' + count + '問作成してください。\n\n' +
    '学校段階: ' + level + '\n難易度: ' + genSubLevel + '（' + sublevelDesc + '）\n分野: ' + fields.join('、') + '\n形式: ' + type + '\n制限時間: ' + minutes + '分\n問題数: ' + count + '問\n\n' +
    fbCtx +
    '\n以下のJSON形式のみで返答してください（マークダウン記号不要）:\n{"title":"タイトル","questions":[{"no":1,"field":"分野","type":"形式","body":"問題文","choices":["A:..."],"answer":"解答","explanation":"解説"}]}';

  document.getElementById('gen-result').innerHTML = '<div class="gen-loading"><div class="gen-spinner"></div><div class="gen-loading-text">AIが問題を生成中...</div><div style="font-size:11px;color:var(--text3);">DBフィードバックを反映しています</div></div>';
  try {
    var res = await fetch('https://api.anthropic.com/v1/messages', {
      method:'POST', headers:{'Content-Type':'application/json'},
      body: JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:4000,messages:[{role:'user',content:prompt}]})
    });
    var data = await res.json();
    var raw = (data.content||[]).map(function(b){ return b.text||''; }).join('');
    var clean = raw.replace(/```json|```/g,'').trim();
    var exam;
    try { exam = JSON.parse(clean); } catch(e) {
      document.getElementById('gen-result').innerHTML = '<div class="gen-output"><pre style="white-space:pre-wrap;font-size:13px;color:var(--text2);">' + raw + '</pre></div>';
      return;
    }
    renderGenResult(exam, fields, level, minutes, fbCtx);
  } catch(e) {
    document.getElementById('gen-result').innerHTML = '<div class="empty-state" style="padding:40px;"><div style="color:var(--red);">生成に失敗しました: ' + e.message + '</div></div>';
  }
}

function renderGenResult(exam, fields, level, minutes, fbCtx) {
  var qs = exam.questions || [];
  lastGeneratedExam = exam;
  var qHtml = qs.map(function(q,i){
    var choicesHtml = '';
    if (q.choices && q.choices.length) {
      choicesHtml = '<div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;margin:10px 0;">';
      q.choices.forEach(function(c){ choicesHtml += '<div style="padding:7px 10px;border-radius:6px;border:1px solid var(--border);background:var(--bg3);font-size:13px;color:var(--text2);">' + c + '</div>'; });
      choicesHtml += '</div>';
    }
    return '<div class="q-block">' +
      '<div class="q-num">問' + (q.no||i+1) + '　<span style="color:var(--text3);font-weight:400;">' + (q.field||'') + '</span></div>' +
      '<div class="q-body">' + q.body + '</div>' + choicesHtml +
      '<details style="margin-top:8px;"><summary style="font-size:12px;color:var(--text3);cursor:pointer;">解答・解説を表示</summary>' +
        '<div class="q-answer" style="margin-top:8px;">' +
          '<div style="margin-bottom:4px;font-size:11px;color:var(--green);">解答: ' + q.answer + '</div>' +
          '<div style="color:var(--text2);font-family:var(--font-body);font-size:13px;">' + (q.explanation||'') + '</div>' +
        '</div></details>' +
    '</div>';
  }).join('');

  var hasFb = fbCtx.length > 0;
  var fbBadge = hasFb ? '<span style="font-size:11px;padding:3px 10px;border-radius:20px;background:rgba(212,168,67,0.1);color:var(--accent);border:1px solid rgba(212,168,67,0.2);">DBフィードバック反映済</span>' : '';
  document.getElementById('gen-result').innerHTML =
    '<div class="gen-output">' +
      '<div style="display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:16px;gap:12px;flex-wrap:wrap;">' +
        '<div><h3>' + (exam.title||'数学試験') + '</h3><div style="font-size:12px;color:var(--text3);margin-top:4px;">' + level + ' / ' + genSubLevel + ' / ' + minutes + '分 / ' + qs.length + '問</div></div>' +
        '<div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap;">' + fbBadge +
          '<button class="btn btn-secondary" style="font-size:12px;padding:6px 12px;" onclick="saveGeneratedToClipboard()">コピー</button>' +
          '<button class="btn btn-primary" style="font-size:12px;padding:6px 12px;" onclick="generateExam()">再生成</button>' +
        '</div>' +
      '</div>' +
      (hasFb ? '<div class="ai-block" style="margin-bottom:16px;font-size:12px;">' + fbCtx.replace(/\n/g,'<br>') + '</div>' : '') +
      qHtml +
    '</div>';
}

function saveGeneratedToClipboard() {
  if (!lastGeneratedExam) return;
  var text = (lastGeneratedExam.questions||[]).map(function(q,i){
    return '問' + (i+1) + '. ' + q.body + '\n解答: ' + q.answer + '\n解説: ' + (q.explanation||'');
  }).join('\n\n');
  navigator.clipboard.writeText(text).then(function(){ toast('コピーしました','success'); });
}

/* ── WEAKNESS PAGE ── */
var wkUser  = null;
var recResult = null;

function renderWeakness() {
  if (!wkUser) wkUser = state.currentUser;
  var ul = document.getElementById('wk-user-list');
  if (ul) ul.innerHTML = state.users.map(function(u){
    return '<button class="wk-user-btn' + (u===wkUser?' active':'') + '" onclick="selectWkUser(\'' + u + '\')">' + u + '</button>';
  }).join('');
  renderWkPanel();
}

function selectWkUser(u) { wkUser = u; renderWeakness(); }

function computeWeakness(user) {
  var ans = state.answers.filter(function(a){ return a.user === user; });
  var fields = ['代数・方程式','関数・グラフ','図形・幾何','確率・統計','微分・積分','数列・級数','ベクトル・行列','三角関数'];
  return fields.map(function(field){
    var fa = ans.filter(function(a){
      var p = state.problems.find(function(x){ return x.id===a.probId; });
      return p && p.field === field;
    });
    if (!fa.length) return {field:field,total:0,correct:0,rate:null,avgTime:null,label:'データなし',score:0.5};
    var correct = fa.filter(function(a){ return a.correct; }).length;
    var rate = correct / fa.length;
    var avgTime = fa.reduce(function(s,a){ return s+(a.time||0); },0) / fa.length;
    var expTime = fa.reduce(function(s,a){
      var p = state.problems.find(function(x){ return x.id===a.probId; });
      return s + (p ? (getAgg(p.id) ? getAgg(p.id).blendedTime : p.regTime) : 10);
    },0) / fa.length;
    var penalty = avgTime > expTime * 1.5 ? 0.1 : 0;
    var score = Math.max(0, rate - penalty);
    var label = score < 0.4 ? '弱点' : score < 0.7 ? '要注意' : '得意';
    return {field:field,total:fa.length,correct:correct,rate:Math.round(rate*100),score:score,avgTime:Math.round(avgTime),expTime:Math.round(expTime),label:label};
  });
}

function renderWkPanel() {
  var panel = document.getElementById('wk-panel');
  if (!panel) return;
  var ans = state.answers.filter(function(a){ return a.user===wkUser; });
  if (!ans.length) {
    panel.innerHTML = '<div class="gen-output">' +
      '<div style="font-size:15px;font-weight:500;color:var(--text);margin-bottom:6px;">' + wkUser + ' の解答履歴</div>' +
      '<div style="font-size:12px;color:var(--text3);margin-bottom:16px;">まだ解答が記録されていません。</div>' +
      '<div style="text-align:center;padding:10px 0;">' +
        '<button class="btn btn-ghost" onclick="seedAnswers()">サンプル解答データを追加</button>' +
        '<div style="font-size:11px;color:var(--text3);margin-top:6px;">デモ用：ランダムな解答履歴を自動生成します</div>' +
      '</div></div>';
    return;
  }
  var wk = computeWeakness(wkUser);
  var weak = wk.filter(function(f){ return f.total>0 && f.label==='弱点'; });
  var mid  = wk.filter(function(f){ return f.total>0 && f.label==='要注意'; });
  var totalOk = ans.filter(function(a){ return a.correct; }).length;
  var overallRate = Math.round(totalOk/ans.length*100);

  var fieldRows = wk.filter(function(f){ return f.total>0; }).sort(function(a,b){ return a.score-b.score; }).map(function(f){
    var col = f.score<0.4?'var(--red)':f.score<0.7?'var(--accent)':'var(--green)';
    var bc  = f.label==='弱点'?'weak':f.label==='要注意'?'mid':'strong';
    return '<div class="wk-field-row">' +
      '<span class="wk-field-name">' + f.field + '</span>' +
      '<div class="wk-bar-wrap"><div class="wk-bar" style="width:' + f.rate + '%;background:' + col + '"></div></div>' +
      '<span class="wk-stat" style="color:' + col + '">' + f.rate + '%</span>' +
      '<span style="font-size:11px;color:var(--text3);min-width:36px;text-align:right;">' + f.correct + '/' + f.total + '</span>' +
      '<span class="wk-badge ' + bc + '">' + f.label + '</span>' +
    '</div>';
  }).join('');

  var recentHtml = ans.slice().sort(function(a,b){ return b.at-a.at; }).slice(0,8).map(function(a){
    var p = state.problems.find(function(x){ return x.id===a.probId; });
    return '<div class="answer-hist-row">' +
      '<div class="ans-result ' + (a.correct?'ok':'ng') + '">' + (a.correct?'○':'✕') + '</div>' +
      '<span style="color:var(--text3);font-family:var(--font-mono);min-width:46px;">' + a.probId + '</span>' +
      '<span style="flex:1;color:var(--text2);overflow:hidden;text-overflow:ellipsis;white-space:nowrap;">' + (p?p.body.slice(0,40)+'…':'不明') + '</span>' +
      '<span style="color:var(--text3);">' + (a.time?a.time+'分':'-') + '</span>' +
    '</div>';
  }).join('');

  var aiComment = buildWeaknessComment(wkUser, weak, mid, overallRate);
  var badgesHtml = weak.map(function(f){ return '<span class="wk-badge weak">' + f.field + '</span>'; }).join('') +
                   mid.map(function(f){  return '<span class="wk-badge mid">'  + f.field + '</span>'; }).join('');

  panel.innerHTML = '<div class="gen-output">' +
    '<div style="display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:16px;flex-wrap:wrap;gap:8px;">' +
      '<div><div style="font-size:15px;font-weight:500;color:var(--text);">' + wkUser + ' の弱点分析</div>' +
          '<div style="font-size:12px;color:var(--text3);margin-top:3px;">全' + ans.length + '問 / 正答率 ' + overallRate + '%</div></div>' +
      '<div style="display:flex;gap:6px;flex-wrap:wrap;">' + badgesHtml + '</div>' +
    '</div>' +
    '<div class="ai-block" style="margin-bottom:16px;"><div class="ai-label"><div class="ai-dot"></div>AI 弱点コメント</div>' + aiComment + '</div>' +
    '<div style="font-size:12px;font-weight:500;color:var(--text2);text-transform:uppercase;letter-spacing:.06em;margin-bottom:10px;">分野別正答率</div>' +
    (fieldRows || '<div style="color:var(--text3);font-size:13px;">データなし</div>') +
    '<div style="font-size:12px;font-weight:500;color:var(--text2);text-transform:uppercase;letter-spacing:.06em;margin:16px 0 10px;">最近の解答履歴</div>' +
    '<div>' + recentHtml + '</div>' +
  '</div>';
}

function buildWeaknessComment(user, weak, mid, rate) {
  var lines = [];
  if (rate >= 80) lines.push(user + 'さんの全体正答率は' + rate + '%と高く、安定した実力が伺えます。');
  else if (rate >= 60) lines.push(user + 'さんの全体正答率は' + rate + '%です。基礎はできていますが、さらなる強化が期待できます。');
  else lines.push(user + 'さんの全体正答率は' + rate + '%です。まず基礎固めから取り組むと効果的です。');
  if (weak.length) lines.push('弱点分野として <strong style="color:var(--red)">' + weak.map(function(f){ return f.field; }).join('・') + '</strong> が検出されました。集中的な復習が必要です。');
  if (mid.length)  lines.push('要注意分野: <strong style="color:var(--accent)">' + mid.map(function(f){ return f.field; }).join('・') + '</strong>（正答率40〜70%）。演習量を増やすと安定します。');
  if (weak.length || mid.length) lines.push('「弱点復習テストを生成」ボタンで、これらの分野に特化した復習問題を自動作成できます。');
  else if (rate >= 70) lines.push('現時点では明確な弱点は検出されていません。より高い難易度への挑戦をお勧めします。');
  return lines.join(' ');
}

function seedAnswers() {
  state.problems.forEach(function(p){
    state.users.filter(function(u){ return u !== 'ゲスト'; }).forEach(function(u){
      if (state.answers.find(function(a){ return a.probId===p.id&&a.user===u; })) return;
      var prob = 0.7;
      if (u==='田中'&&(p.field==='ベクトル・行列'||p.field==='数列・級数')) prob=0.3;
      if (u==='鈴木'&&p.field==='微分・積分') prob=0.35;
      if (u==='山田'&&p.field==='代数・方程式') prob=0.4;
      var ok = Math.random() < prob;
      var t  = Math.max(1, Math.round((p.regTime||10)*(0.6+Math.random()*0.9)*(ok?1:1.4)));
      state.answers.push({probId:p.id,user:u,correct:ok,time:t,at:Date.now()-Math.random()*864e5*10});
    });
  });
  save(); toast('サンプルデータを追加しました','success'); renderWkPanel();
}

function setRecResult(val) {
  recResult = val;
  var ok = document.getElementById('rec-correct-btn');
  var ng = document.getElementById('rec-wrong-btn');
  if (!ok || !ng) return;
  if (val === true) {
    ok.style.cssText = 'flex:1;background:rgba(76,175,125,0.25);color:var(--green);border:1px solid rgba(76,175,125,0.5);';
    ng.style.cssText = 'flex:1;background:var(--bg3);color:var(--text3);border:1px solid var(--border2);';
  } else if (val === false) {
    ng.style.cssText = 'flex:1;background:rgba(224,92,92,0.25);color:var(--red);border:1px solid rgba(224,92,92,0.5);';
    ok.style.cssText = 'flex:1;background:var(--bg3);color:var(--text3);border:1px solid var(--border2);';
  } else {
    ok.style.cssText = 'flex:1;background:rgba(76,175,125,0.12);color:var(--green);border:1px solid rgba(76,175,125,0.3);';
    ng.style.cssText = 'flex:1;background:var(--bg3);color:var(--text3);border:1px solid var(--border2);';
  }
}

function recordAnswer() {
  var probId = document.getElementById('rec-prob-id').value.trim().toUpperCase();
  if (!probId) { toast('問題IDを入力してください'); return; }
  if (!state.problems.find(function(p){ return p.id===probId; })) { toast('問題IDが見つかりません: '+probId); return; }
  if (recResult === null) { toast('正解/不正解を選択してください'); return; }
  var time = parseInt(document.getElementById('rec-time').value || '0') || null;
  state.answers.push({probId:probId,user:state.currentUser,correct:recResult,time:time,at:Date.now()});
  save();
  toast(probId + ' の解答を記録しました (' + (recResult?'正解':'不正解') + ')', 'success');
  document.getElementById('rec-prob-id').value = '';
  document.getElementById('rec-time').value = '';
  recResult = null;
  setRecResult(null);
  if (wkUser === state.currentUser) renderWkPanel();
}

async function generateReviewTest() {
  var wk = computeWeakness(wkUser).filter(function(f){ return f.total>0; });
  var weakFields = wk.filter(function(f){ return f.label==='弱点'||f.label==='要注意'; }).map(function(f){ return f.field; });
  if (!weakFields.length) { toast('弱点分野が検出されていません。まず解答を記録してください'); return; }

  var userAns = state.answers.filter(function(a){ return a.user===wkUser; });
  var levelCount = {};
  userAns.forEach(function(a){
    var p = state.problems.find(function(x){ return x.id===a.probId; });
    if (p) levelCount[p.level] = (levelCount[p.level]||0)+1;
  });
  var userLevel = Object.entries(levelCount).sort(function(a,b){ return b[1]-a[1]; })[0];
  userLevel = userLevel ? userLevel[0] : '高校生';
  var totalOk = userAns.filter(function(a){ return a.correct; }).length;
  var rate = userAns.length ? Math.round(totalOk/userAns.length*100) : 50;
  var sublevel = rate >= 70 ? '標準' : '基礎';

  var wkDetails = wk.filter(function(f){ return f.total>0&&(f.label==='弱点'||f.label==='要注意'); }).map(function(f){
    return f.field + ': 正答率' + f.rate + '%（' + f.correct + '/' + f.total + '問）、平均' + f.avgTime + '分/問';
  }).join('\n');

  var prompt = '数学の個別指導の専門家として、以下のデータをもとに弱点克服のための復習テストを作成してください。\n\n' +
    'ユーザー: ' + wkUser + '\n全体正答率: ' + rate + '%\n学習レベル: ' + userLevel + '\n\n' +
    '【弱点分野データ】\n' + wkDetails + '\n\n' +
    '対象分野: ' + weakFields.join('、') + '\n難易度: ' + sublevel + '\n問題数: 6問（弱点分野に多め配分）\n形式: 記述式・選択式の混合\n\n' +
    '以下のJSON形式のみで返答してください:\n{"title":"タイトル","targetUser":"' + wkUser + '","weakFields":' + JSON.stringify(weakFields) + ',"questions":[{"no":1,"field":"分野","type":"形式","body":"問題文","choices":["A:..."],"answer":"解答","explanation":"解説","reviewPoint":"弱点克服に効果的な理由（1文）"}]}';

  document.getElementById('wk-panel').innerHTML =
    '<div class="gen-loading"><div class="gen-spinner"></div>' +
    '<div class="gen-loading-text">' + wkUser + 'さんの弱点に合わせた復習テストを生成中...</div>' +
    '<div style="font-size:11px;color:var(--text3);">対象: ' + weakFields.join('・') + '</div></div>';

  try {
    var res = await fetch('https://api.anthropic.com/v1/messages', {
      method:'POST', headers:{'Content-Type':'application/json'},
      body: JSON.stringify({model:'claude-sonnet-4-20250514',max_tokens:4000,messages:[{role:'user',content:prompt}]})
    });
    var data = await res.json();
    var raw = (data.content||[]).map(function(b){ return b.text||''; }).join('');
    var clean = raw.replace(/```json|```/g,'').trim();
    var exam;
    try { exam = JSON.parse(clean); } catch(e) {
      document.getElementById('wk-panel').innerHTML = '<div class="gen-output"><pre style="white-space:pre-wrap;font-size:13px;">' + raw + '</pre></div>';
      return;
    }
    renderReviewResult(exam);
  } catch(e) {
    document.getElementById('wk-panel').innerHTML = '<div class="empty-state" style="padding:40px;"><div style="color:var(--red);">生成に失敗しました: ' + e.message + '</div></div>';
  }
}

function renderReviewResult(exam) {
  var qs = exam.questions || [];
  var qHtml = qs.map(function(q,i){
    var choicesHtml = '';
    if (q.choices && q.choices.length) {
      choicesHtml = '<div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;margin:8px 0;">';
      q.choices.forEach(function(c){ choicesHtml += '<div style="padding:7px 10px;border-radius:6px;border:1px solid var(--border);background:var(--bg3);font-size:13px;color:var(--text2);">' + c + '</div>'; });
      choicesHtml += '</div>';
    }
    var rpHtml = q.reviewPoint
      ? '<div style="font-size:12px;color:var(--purple);margin:6px 0;padding:6px 10px;background:rgba(155,127,232,0.07);border-radius:6px;border-left:2px solid var(--purple);">復習ポイント: ' + q.reviewPoint + '</div>'
      : '';
    return '<div class="q-block">' +
      '<div class="q-num">問' + (q.no||i+1) + '　<span style="color:var(--text3);font-weight:400;">' + (q.field||'') + '</span></div>' +
      '<div class="q-body">' + q.body + '</div>' + choicesHtml + rpHtml +
      '<details style="margin-top:8px;"><summary style="font-size:12px;color:var(--text3);cursor:pointer;">解答・解説を表示</summary>' +
        '<div class="q-answer" style="margin-top:8px;">' +
          '<div style="margin-bottom:4px;font-size:11px;color:var(--green);">解答: ' + q.answer + '</div>' +
          '<div style="color:var(--text2);font-family:var(--font-body);font-size:13px;">' + (q.explanation||'') + '</div>' +
        '</div></details>' +
    '</div>';
  }).join('');

  document.getElementById('wk-panel').innerHTML =
    '<div class="gen-output">' +
      '<div style="display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:16px;flex-wrap:wrap;gap:8px;">' +
        '<div><h3>' + (exam.title||'弱点復習テスト') + '</h3>' +
          '<div style="font-size:12px;color:var(--text3);margin-top:3px;">' + exam.targetUser + ' / ' + (exam.weakFields||[]).join('・') + ' / ' + qs.length + '問</div></div>' +
        '<div style="display:flex;gap:8px;">' +
          '<span style="font-size:11px;padding:3px 10px;border-radius:20px;background:rgba(224,92,92,0.1);color:var(--red);border:1px solid rgba(224,92,92,0.2);">弱点特化</span>' +
          '<button class="btn btn-secondary" style="font-size:12px;padding:5px 10px;" onclick="renderWkPanel()">← 分析に戻る</button>' +
          '<button class="btn btn-primary" style="font-size:12px;padding:5px 10px;" onclick="generateReviewTest()">再生成</button>' +
        '</div>' +
      '</div>' +
      qHtml +
    '</div>';
}

/* ── INIT ── */
load();
initSeed();
initRegStars();
renderDashboard();

document.querySelectorAll('.nav-tab').forEach(function(el,i){
  var pages = ['dashboard','problems','generate','weakness','register','analytics'];
  el.onclick = function(){ showPage(pages[i]); };
});

document.querySelectorAll('#gen-fields .chip').forEach(function(c){
  c.addEventListener('click', function(){ c.classList.toggle('on'); renderGenDBSummary(); });
});
</script>
</body>
</html>
