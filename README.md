<iframe srcdoc="&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;ja&quot;&gt;
&lt;head&gt;
&lt;meta charset=&quot;UTF-8&quot;&gt;
&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
&lt;title&gt;UIコンポーネント 確認ページ&lt;/title&gt;
&lt;link href=&quot;https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700&amp;family=DM+Mono:wght@400;500&amp;family=Playfair+Display:wght@700&amp;display=swap&quot; rel=&quot;stylesheet&quot;&gt;
&lt;style&gt;
  :root {
    --bg: #0f1117;
    --surface: #181c27;
    --surface2: #1e2333;
    --border: #2a3045;
    --border2: #3a4460;
    --text: #e8eaf0;
    --text-sub: #8892a8;
    --text-muted: #55607a;
    --accent: #4f7cff;
    --accent2: #7c4fff;
    --green: #22c87a;
    --yellow: #f0b429;
    --red: #ff4f6a;
    --orange: #ff8c42;
    --radius: 10px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: &#x27;Noto Sans JP&#x27;, sans-serif;
    background: var(--bg);
    color: var(--text);
    font-size: 14px;
    line-height: 1.6;
  }

  /* ========= LAYOUT ========= */
  .page-header {
    padding: 32px 40px 20px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: baseline;
    gap: 16px;
  }
  .page-header h1 {
    font-family: &#x27;Playfair Display&#x27;, serif;
    font-size: 22px;
    color: var(--text);
    letter-spacing: .5px;
  }
  .page-header span {
    font-size: 12px;
    color: var(--text-muted);
    font-family: &#x27;DM Mono&#x27;, monospace;
  }

  .container {
    max-width: 1080px;
    margin: 0 auto;
    padding: 40px 24px 120px;
    display: grid;
    gap: 40px;
  }

  .section-label {
    font-size: 10px;
    font-family: &#x27;DM Mono&#x27;, monospace;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 16px;
    padding-bottom: 8px;
    border-bottom: 1px solid var(--border);
  }

  .demo-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }
  .demo-grid.single { grid-template-columns: 1fr; }
  .demo-grid.triple { grid-template-columns: 1fr 1fr 1fr; }

  .demo-box {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
  }
  .demo-title {
    font-size: 11px;
    color: var(--text-muted);
    font-family: &#x27;DM Mono&#x27;, monospace;
    margin-bottom: 14px;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .demo-title::before {
    content: &#x27;&#x27;;
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--accent);
    display: inline-block;
    flex-shrink: 0;
  }

  /* ========= ACCORDION ========= */
  .accordion-item {
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
    margin-bottom: 8px;
  }
  .accordion-item:last-child { margin-bottom: 0; }
  .accordion-header {
    width: 100%;
    background: var(--surface2);
    border: none;
    color: var(--text);
    padding: 12px 16px;
    text-align: left;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 13px;
    font-family: &#x27;Noto Sans JP&#x27;, sans-serif;
    transition: background .2s;
  }
  .accordion-header:hover { background: var(--border); }
  .accordion-icon {
    font-size: 18px;
    color: var(--text-muted);
    transition: transform .25s;
    line-height: 1;
  }
  .accordion-body {
    max-height: 0;
    overflow: hidden;
    transition: max-height .3s ease, padding .3s ease;
    font-size: 13px;
    color: var(--text-sub);
    background: var(--surface);
    padding: 0 16px;
  }
  .accordion-body.open {
    max-height: 200px;
    padding: 12px 16px;
  }
  .accordion-icon.open { transform: rotate(45deg); }

  /* ========= MODAL ========= */
  .btn {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 8px 16px;
    border-radius: 6px;
    font-size: 13px;
    font-family: &#x27;Noto Sans JP&#x27;, sans-serif;
    cursor: pointer;
    border: none;
    transition: all .2s;
    font-weight: 500;
  }
  .btn-primary { background: var(--accent); color: #fff; }
  .btn-primary:hover { background: #3a6de8; }
  .btn-ghost { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
  .btn-ghost:hover { background: var(--border); }
  .btn-danger { background: var(--red); color: #fff; }
  .btn-danger:hover { background: #e03358; }
  .btn-sm { padding: 5px 10px; font-size: 12px; }

  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,.65);
    z-index: 100;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(2px);
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--surface);
    border: 1px solid var(--border2);
    border-radius: 14px;
    padding: 28px;
    width: 480px;
    max-width: 90vw;
    position: relative;
    animation: modalIn .2s ease;
  }
  @keyframes modalIn {
    from { opacity: 0; transform: scale(.94) translateY(10px); }
    to   { opacity: 1; transform: scale(1) translateY(0); }
  }
  .modal h3 { font-size: 16px; margin-bottom: 4px; }
  .modal p  { font-size: 13px; color: var(--text-sub); margin-bottom: 20px; }
  .modal-table { width: 100%; border-collapse: collapse; font-size: 12px; }
  .modal-table th, .modal-table td {
    border-bottom: 1px solid var(--border);
    padding: 7px 10px;
    text-align: left;
  }
  .modal-table th { color: var(--text-muted); font-weight: 500; }
  .modal-actions { display: flex; justify-content: flex-end; gap: 8px; margin-top: 20px; }
  .modal-close {
    position: absolute; top: 16px; right: 16px;
    background: none; border: none; color: var(--text-muted);
    font-size: 20px; cursor: pointer; line-height: 1;
  }
  .modal-close:hover { color: var(--text); }

  /* ========= DRAWER ========= */
  .drawer-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,.5);
    z-index: 90;
  }
  .drawer-overlay.open { display: block; }
  .drawer {
    position: fixed;
    top: 0; right: -380px;
    width: 340px;
    height: 100%;
    background: var(--surface);
    border-left: 1px solid var(--border2);
    z-index: 95;
    transition: right .3s cubic-bezier(.4,0,.2,1);
    padding: 28px 24px;
    overflow-y: auto;
  }
  .drawer.open { right: 0; }
  .drawer h3 { font-size: 15px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: center; }
  .drawer-close { background: none; border: none; color: var(--text-muted); font-size: 20px; cursor: pointer; }
  .drawer-section { margin-bottom: 24px; }
  .drawer-section label { display: block; font-size: 11px; color: var(--text-muted); margin-bottom: 8px; text-transform: uppercase; letter-spacing: 1px; }

  /* ========= TOOLTIP ========= */
  .tooltip-wrap {
    position: relative;
    display: inline-flex;
    align-items: center;
    gap: 4px;
  }
  .tooltip-icon {
    width: 16px; height: 16px;
    border-radius: 50%;
    background: var(--border2);
    color: var(--text-muted);
    font-size: 10px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    cursor: default;
    font-weight: 700;
  }
  .tooltip-bubble {
    display: none;
    position: absolute;
    bottom: 120%;
    left: 50%;
    transform: translateX(-50%);
    background: #2a3045;
    border: 1px solid var(--border2);
    color: var(--text);
    font-size: 12px;
    padding: 8px 12px;
    border-radius: 8px;
    white-space: nowrap;
    z-index: 50;
    pointer-events: none;
  }
  .tooltip-bubble::after {
    content: &#x27;&#x27;;
    position: absolute;
    top: 100%; left: 50%;
    transform: translateX(-50%);
    border: 5px solid transparent;
    border-top-color: var(--border2);
  }
  .tooltip-wrap:hover .tooltip-bubble { display: block; }

  .metric-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
    font-size: 13px;
  }
  .metric-row:last-child { border-bottom: none; }
  .metric-label { display: flex; align-items: center; gap: 6px; color: var(--text-sub); }
  .metric-value { font-family: &#x27;DM Mono&#x27;, monospace; color: var(--text); font-size: 14px; }

  /* ========= SLIDER ========= */
  .slider-wrap { margin: 12px 0; }
  .slider-label { display: flex; justify-content: space-between; font-size: 12px; color: var(--text-sub); margin-bottom: 6px; }
  input[type=range] {
    width: 100%;
    -webkit-appearance: none;
    height: 4px;
    border-radius: 2px;
    background: var(--border2);
    outline: none;
    cursor: pointer;
  }
  input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 16px; height: 16px;
    border-radius: 50%;
    background: var(--accent);
    border: 2px solid var(--bg);
    cursor: grab;
  }
  input[type=range]::-webkit-slider-thumb:active { cursor: grabbing; }
  .range-dual { position: relative; height: 20px; }
  .slider-output { text-align: center; margin-top: 8px; font-family: &#x27;DM Mono&#x27;, monospace; font-size: 13px; color: var(--accent); }

  /* ========= TOGGLE / CHECKBOX ========= */
  .toggle-row { display: flex; align-items: center; justify-content: space-between; padding: 9px 0; border-bottom: 1px solid var(--border); }
  .toggle-row:last-child { border-bottom: none; }
  .toggle-label { font-size: 13px; color: var(--text-sub); }
  .toggle {
    position: relative;
    width: 36px; height: 20px;
  }
  .toggle input { opacity: 0; width: 0; height: 0; }
  .toggle-slider {
    position: absolute; inset: 0;
    background: var(--border2);
    border-radius: 20px;
    cursor: pointer;
    transition: .2s;
  }
  .toggle-slider::before {
    content: &#x27;&#x27;;
    position: absolute;
    width: 14px; height: 14px;
    border-radius: 50%;
    background: #fff;
    left: 3px; top: 3px;
    transition: .2s;
  }
  .toggle input:checked + .toggle-slider { background: var(--accent); }
  .toggle input:checked + .toggle-slider::before { transform: translateX(16px); }

  .checkbox-row { display: flex; align-items: center; gap: 8px; padding: 7px 0; }
  .custom-cb { width: 16px; height: 16px; border-radius: 4px; border: 2px solid var(--border2); background: transparent; appearance: none; cursor: pointer; position: relative; flex-shrink: 0; transition: .15s; }
  .custom-cb:checked { background: var(--accent); border-color: var(--accent); }
  .custom-cb:checked::after {
    content: &#x27;✓&#x27;;
    position: absolute;
    font-size: 10px;
    color: #fff;
    top: -1px; left: 1px;
    font-weight: 700;
  }

  /* ========= SELECT ========= */
  .custom-select {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border2);
    border-radius: 8px;
    color: var(--text);
    padding: 9px 12px;
    font-size: 13px;
    font-family: &#x27;Noto Sans JP&#x27;, sans-serif;
    appearance: none;
    background-image: url(&quot;data:image/svg+xml,%3Csvg xmlns=&#x27;http://www.w3.org/2000/svg&#x27; width=&#x27;12&#x27; height=&#x27;8&#x27; viewBox=&#x27;0 0 12 8&#x27;%3E%3Cpath d=&#x27;M1 1l5 5 5-5&#x27; stroke=&#x27;%2355607a&#x27; stroke-width=&#x27;1.5&#x27; fill=&#x27;none&#x27; stroke-linecap=&#x27;round&#x27;/%3E%3C/svg%3E&quot;);
    background-repeat: no-repeat;
    background-position: right 12px center;
    cursor: pointer;
    margin-bottom: 8px;
  }
  .custom-select:focus { outline: 1px solid var(--accent); }

  /* ========= SEARCH BOX ========= */
  .search-wrap { position: relative; margin-bottom: 12px; }
  .search-wrap input {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border2);
    border-radius: 8px;
    color: var(--text);
    padding: 9px 12px 9px 36px;
    font-size: 13px;
    font-family: &#x27;Noto Sans JP&#x27;, sans-serif;
  }
  .search-wrap input:focus { outline: 1px solid var(--accent); }
  .search-wrap input::placeholder { color: var(--text-muted); }
  .search-icon { position: absolute; left: 11px; top: 50%; transform: translateY(-50%); color: var(--text-muted); font-size: 15px; }
  .search-results { font-size: 12px; color: var(--text-muted); margin-top: 4px; }

  /* ========= SORTABLE TABLE ========= */
  .sort-table { width: 100%; border-collapse: collapse; font-size: 12px; }
  .sort-table th {
    text-align: left;
    padding: 9px 10px;
    color: var(--text-muted);
    background: var(--surface2);
    cursor: pointer;
    user-select: none;
    border-bottom: 1px solid var(--border);
    white-space: nowrap;
  }
  .sort-table th:hover { color: var(--accent); }
  .sort-table th.active { color: var(--accent); }
  .sort-arrow { margin-left: 4px; font-size: 10px; }
  .sort-table td { padding: 8px 10px; border-bottom: 1px solid var(--border); color: var(--text-sub); }
  .sort-table tbody tr:hover td { background: var(--surface2); }
  .sort-table td.num { font-family: &#x27;DM Mono&#x27;, monospace; text-align: right; }

  /* ========= CARD GRID ========= */
  .card-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px;
    transition: border-color .2s, transform .15s;
    cursor: default;
  }
  .card:hover { border-color: var(--border2); transform: translateY(-1px); }
  .card-cat { font-size: 10px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 4px; }
  .card-name { font-size: 13px; font-weight: 500; margin-bottom: 8px; }
  .card-val { font-family: &#x27;DM Mono&#x27;, monospace; font-size: 16px; color: var(--accent); }
  .card-sub { font-size: 11px; color: var(--text-muted); margin-top: 2px; }

  /* ========= PROGRESS BAR ========= */
  .progress-item { margin-bottom: 14px; }
  .progress-item:last-child { margin-bottom: 0; }
  .progress-header { display: flex; justify-content: space-between; font-size: 12px; color: var(--text-sub); margin-bottom: 5px; }
  .progress-track { height: 6px; background: var(--border); border-radius: 3px; overflow: hidden; }
  .progress-fill {
    height: 100%;
    border-radius: 3px;
    background: var(--accent);
    transition: width .8s cubic-bezier(.4,0,.2,1);
  }
  .progress-fill.green { background: var(--green); }
  .progress-fill.yellow { background: var(--yellow); }
  .progress-fill.red { background: var(--red); }

  /* ========= STEPPER ========= */
  .stepper { display: flex; flex-direction: column; gap: 0; }
  .step-item { display: flex; gap: 14px; }
  .step-left { display: flex; flex-direction: column; align-items: center; }
  .step-dot {
    width: 28px; height: 28px;
    border-radius: 50%;
    border: 2px solid var(--border2);
    background: var(--surface2);
    color: var(--text-muted);
    font-size: 12px;
    font-weight: 700;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    transition: .2s;
  }
  .step-dot.done { background: var(--green); border-color: var(--green); color: #fff; }
  .step-dot.active { background: var(--accent); border-color: var(--accent); color: #fff; box-shadow: 0 0 0 4px rgba(79,124,255,.2); }
  .step-line { width: 2px; flex: 1; background: var(--border); margin: 4px 0; min-height: 24px; }
  .step-line.done { background: var(--green); }
  .step-body { padding-bottom: 20px; padding-top: 4px; }
  .step-title { font-size: 13px; font-weight: 500; margin-bottom: 2px; }
  .step-desc  { font-size: 12px; color: var(--text-muted); }
  .step-item:last-child .step-body { padding-bottom: 0; }
  .stepper-actions { display: flex; gap: 8px; margin-top: 16px; }

  /* ========= TIMELINE ========= */
  .timeline { position: relative; padding-left: 20px; }
  .timeline::before {
    content: &#x27;&#x27;;
    position: absolute;
    left: 6px; top: 8px; bottom: 8px;
    width: 2px;
    background: var(--border);
  }
  .tl-item { position: relative; margin-bottom: 20px; }
  .tl-item:last-child { margin-bottom: 0; }
  .tl-dot {
    position: absolute;
    left: -17px; top: 5px;
    width: 10px; height: 10px;
    border-radius: 50%;
    background: var(--accent);
    border: 2px solid var(--bg);
  }
  .tl-dot.green { background: var(--green); }
  .tl-dot.yellow { background: var(--yellow); }
  .tl-dot.muted { background: var(--text-muted); }
  .tl-date { font-size: 10px; font-family: &#x27;DM Mono&#x27;, monospace; color: var(--text-muted); margin-bottom: 2px; }
  .tl-title { font-size: 13px; font-weight: 500; margin-bottom: 2px; }
  .tl-desc { font-size: 12px; color: var(--text-sub); }

  /* ========= TOAST ========= */
  .toast-container {
    position: fixed;
    bottom: 24px;
    right: 24px;
    display: flex;
    flex-direction: column;
    gap: 8px;
    z-index: 200;
  }
  .toast {
    background: var(--surface2);
    border: 1px solid var(--border2);
    border-radius: 10px;
    padding: 12px 16px;
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 13px;
    box-shadow: 0 8px 24px rgba(0,0,0,.4);
    min-width: 220px;
    animation: toastIn .3s ease;
    position: relative;
  }
  @keyframes toastIn {
    from { opacity: 0; transform: translateX(30px); }
    to   { opacity: 1; transform: translateX(0); }
  }
  .toast.removing { animation: toastOut .3s ease forwards; }
  @keyframes toastOut {
    to { opacity: 0; transform: translateX(30px); }
  }
  .toast-icon { font-size: 16px; flex-shrink: 0; }
  .toast-close { margin-left: auto; background: none; border: none; color: var(--text-muted); cursor: pointer; font-size: 14px; padding: 0 0 0 8px; }

  .toast-buttons { display: flex; gap: 8px; flex-wrap: wrap; margin-top: 4px; }

  /* ========= BADGE ========= */
  .badge {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    padding: 3px 8px;
    border-radius: 20px;
    font-size: 11px;
    font-weight: 500;
    font-family: &#x27;DM Mono&#x27;, monospace;
  }
  .badge.blue   { background: rgba(79,124,255,.15); color: #7aa0ff; border: 1px solid rgba(79,124,255,.3); }
  .badge.green  { background: rgba(34,200,122,.15); color: #4de0a0; border: 1px solid rgba(34,200,122,.3); }
  .badge.red    { background: rgba(255,79,106,.15); color: #ff7088; border: 1px solid rgba(255,79,106,.3); }
  .badge.yellow { background: rgba(240,180,41,.15); color: #f5c842; border: 1px solid rgba(240,180,41,.3); }
  .badge.gray   { background: rgba(85,96,122,.2); color: var(--text-muted); border: 1px solid var(--border); }
  .badge-dot { width: 6px; height: 6px; border-radius: 50%; background: currentColor; }

  .badge-num {
    position: absolute;
    top: -6px; right: -6px;
    width: 18px; height: 18px;
    border-radius: 50%;
    background: var(--red);
    color: #fff;
    font-size: 10px;
    font-weight: 700;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 2px solid var(--bg);
  }

  .badge-demo { display: flex; flex-wrap: wrap; gap: 8px; align-items: center; }
  .badge-icon-wrap { position: relative; display: inline-flex; width: 36px; height: 36px; align-items: center; justify-content: center; background: var(--surface2); border-radius: 8px; font-size: 18px; }

  /* ========= STATUS INDICATOR ========= */
  .status-table { width: 100%; }
  .status-row { display: flex; align-items: center; justify-content: space-between; padding: 10px 0; border-bottom: 1px solid var(--border); }
  .status-row:last-child { border-bottom: none; }
  .status-name { font-size: 13px; color: var(--text-sub); }
  .status-indicators { display: flex; align-items: center; gap: 8px; }
  .status-pill {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    padding: 3px 10px;
    border-radius: 20px;
    font-size: 11px;
    font-weight: 500;
  }
  .status-pill::before {
    content: &#x27;&#x27;;
    width: 6px; height: 6px;
    border-radius: 50%;
    background: currentColor;
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: .4; }
  }
  .status-pill.ok     { background: rgba(34,200,122,.12); color: #4de0a0; }
  .status-pill.warn   { background: rgba(240,180,41,.12); color: #f5c842; }
  .status-pill.ng     { background: rgba(255,79,106,.12); color: #ff7088; }
  .status-pill.idle   { background: rgba(85,96,122,.15); color: var(--text-muted); }
  .status-val { font-family: &#x27;DM Mono&#x27;, monospace; font-size: 12px; color: var(--text-muted); }

&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;

&lt;div class=&quot;page-header&quot;&gt;
  &lt;h1&gt;UI Components&lt;/h1&gt;
  &lt;span&gt;保険事業ダッシュボード — インタラクション確認&lt;/span&gt;
&lt;/div&gt;

&lt;div class=&quot;container&quot;&gt;

  &lt;!-- ===== INTERACTION GROUP ===== --&gt;
  &lt;div&gt;
    &lt;div class=&quot;section-label&quot;&gt;インタラクション系&lt;/div&gt;
    &lt;div class=&quot;demo-grid&quot;&gt;

      &lt;!-- ACCORDION --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;アコーディオン — 費用項目の定義・注釈&lt;/div&gt;
        &lt;div class=&quot;accordion-item&quot;&gt;
          &lt;button class=&quot;accordion-header&quot; onclick=&quot;toggleAccordion(this)&quot;&gt;
            &lt;span&gt;テレマティクス保険とは&lt;/span&gt;
            &lt;span class=&quot;accordion-icon&quot;&gt;+&lt;/span&gt;
          &lt;/button&gt;
          &lt;div class=&quot;accordion-body&quot;&gt;
            走行データをもとに保険料を算出する仕組み。Aioi Nissay Dowaとの連携案件で適用予定。データ収集はOBD端末またはスマートフォンGPSを使用。
          &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;accordion-item&quot;&gt;
          &lt;button class=&quot;accordion-header&quot; onclick=&quot;toggleAccordion(this)&quot;&gt;
            &lt;span&gt;ウェルネス連動型保険とは&lt;/span&gt;
            &lt;span class=&quot;accordion-icon&quot;&gt;+&lt;/span&gt;
          &lt;/button&gt;
          &lt;div class=&quot;accordion-body&quot;&gt;
            健康データ（歩数・睡眠など）に応じて保険料が変動するスキーム。JMDC / Pep Up との統合で実現可能。個人情報の取り扱いに法的整理が必要。
          &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;accordion-item&quot;&gt;
          &lt;button class=&quot;accordion-header&quot; onclick=&quot;toggleAccordion(this)&quot;&gt;
            &lt;span&gt;不正検知スコアの定義&lt;/span&gt;
            &lt;span class=&quot;accordion-icon&quot;&gt;+&lt;/span&gt;
          &lt;/button&gt;
          &lt;div class=&quot;accordion-body&quot;&gt;
            RegTech Edge / ServiceWare のモデルが出力する 0〜100 のスコア。80以上を要調査フラグとして扱う。誤検知率は月次でキャリブレーション中。
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- TOOLTIP --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;ツールチップ — 指標名に ? アイコン&lt;/div&gt;
        &lt;div class=&quot;metric-row&quot;&gt;
          &lt;span class=&quot;metric-label&quot;&gt;
            損害率
            &lt;span class=&quot;tooltip-wrap&quot;&gt;
              &lt;span class=&quot;tooltip-icon&quot;&gt;?&lt;/span&gt;
              &lt;span class=&quot;tooltip-bubble&quot;&gt;支払保険金 ÷ 収入保険料 × 100。業界標準は65〜70%が目安。&lt;/span&gt;
            &lt;/span&gt;
          &lt;/span&gt;
          &lt;span class=&quot;metric-value&quot;&gt;68.4%&lt;/span&gt;
        &lt;/div&gt;
        &lt;div class=&quot;metric-row&quot;&gt;
          &lt;span class=&quot;metric-label&quot;&gt;
            事業費率
            &lt;span class=&quot;tooltip-wrap&quot;&gt;
              &lt;span class=&quot;tooltip-icon&quot;&gt;?&lt;/span&gt;
              &lt;span class=&quot;tooltip-bubble&quot;&gt;営業・管理費 ÷ 収入保険料 × 100。低いほど効率的な運営。&lt;/span&gt;
            &lt;/span&gt;
          &lt;/span&gt;
          &lt;span class=&quot;metric-value&quot;&gt;24.1%&lt;/span&gt;
        &lt;/div&gt;
        &lt;div class=&quot;metric-row&quot;&gt;
          &lt;span class=&quot;metric-label&quot;&gt;
            コンバインドレシオ
            &lt;span class=&quot;tooltip-wrap&quot;&gt;
              &lt;span class=&quot;tooltip-icon&quot;&gt;?&lt;/span&gt;
              &lt;span class=&quot;tooltip-bubble&quot;&gt;損害率＋事業費率。100%未満で収支黒字。&lt;/span&gt;
            &lt;/span&gt;
          &lt;/span&gt;
          &lt;span class=&quot;metric-value&quot;&gt;92.5%&lt;/span&gt;
        &lt;/div&gt;
        &lt;div class=&quot;metric-row&quot;&gt;
          &lt;span class=&quot;metric-label&quot;&gt;
            再保険回収率
            &lt;span class=&quot;tooltip-wrap&quot;&gt;
              &lt;span class=&quot;tooltip-icon&quot;&gt;?&lt;/span&gt;
              &lt;span class=&quot;tooltip-bubble&quot;&gt;再保険会社から回収した金額の総保険金支払に対する比率。&lt;/span&gt;
            &lt;/span&gt;
          &lt;/span&gt;
          &lt;span class=&quot;metric-value&quot;&gt;11.2%&lt;/span&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- MODAL --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;モーダル — グラフ棒クリックで月次明細を表示&lt;/div&gt;
        &lt;p style=&quot;font-size:12px;color:var(--text-sub);margin-bottom:12px;&quot;&gt;※ 棒グラフをクリックすると対象月の明細が表示されるイメージ&lt;/p&gt;
        &lt;div style=&quot;display:flex;gap:8px;align-items:flex-end;height:80px;margin-bottom:12px;&quot;&gt;
          &lt;div onclick=&quot;openModal(&#x27;2025年1月&#x27;)&quot; style=&quot;flex:1;background:var(--accent);opacity:.5;border-radius:4px 4px 0 0;height:45%;cursor:pointer;transition:.15s&quot; onmouseover=&quot;this.style.opacity=&#x27;.8&#x27;&quot; onmouseout=&quot;this.style.opacity=&#x27;.5&#x27;&quot;&gt;&lt;/div&gt;
          &lt;div onclick=&quot;openModal(&#x27;2025年2月&#x27;)&quot; style=&quot;flex:1;background:var(--accent);opacity:.5;border-radius:4px 4px 0 0;height:70%;cursor:pointer;transition:.15s&quot; onmouseover=&quot;this.style.opacity=&#x27;.8&#x27;&quot; onmouseout=&quot;this.style.opacity=&#x27;.5&#x27;&quot;&gt;&lt;/div&gt;
          &lt;div onclick=&quot;openModal(&#x27;2025年3月&#x27;)&quot; style=&quot;flex:1;background:var(--accent);opacity:.5;border-radius:4px 4px 0 0;height:100%;cursor:pointer;transition:.15s&quot; onmouseover=&quot;this.style.opacity=&#x27;.8&#x27;&quot; onmouseout=&quot;this.style.opacity=&#x27;.5&#x27;&quot;&gt;&lt;/div&gt;
          &lt;div onclick=&quot;openModal(&#x27;2025年4月&#x27;)&quot; style=&quot;flex:1;background:var(--accent);opacity:.5;border-radius:4px 4px 0 0;height:60%;cursor:pointer;transition:.15s&quot; onmouseover=&quot;this.style.opacity=&#x27;.8&#x27;&quot; onmouseout=&quot;this.style.opacity=&#x27;.5&#x27;&quot;&gt;&lt;/div&gt;
          &lt;div onclick=&quot;openModal(&#x27;2025年5月&#x27;)&quot; style=&quot;flex:1;background:var(--accent);opacity:.5;border-radius:4px 4px 0 0;height:80%;cursor:pointer;transition:.15s&quot; onmouseover=&quot;this.style.opacity=&#x27;.8&#x27;&quot; onmouseout=&quot;this.style.opacity=&#x27;.5&#x27;&quot;&gt;&lt;/div&gt;
        &lt;/div&gt;
        &lt;div style=&quot;display:flex;justify-content:space-between;font-size:10px;color:var(--text-muted);font-family:&#x27;DM Mono&#x27;,monospace;&quot;&gt;
          &lt;span&gt;1月&lt;/span&gt;&lt;span&gt;2月&lt;/span&gt;&lt;span&gt;3月&lt;/span&gt;&lt;span&gt;4月&lt;/span&gt;&lt;span&gt;5月&lt;/span&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- DRAWER --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;ドロワー — フィルター設定パネル&lt;/div&gt;
        &lt;p style=&quot;font-size:12px;color:var(--text-sub);margin-bottom:12px;&quot;&gt;メイン画面を邪魔せずフィルター操作ができます&lt;/p&gt;
        &lt;button class=&quot;btn btn-ghost&quot; onclick=&quot;openDrawer()&quot;&gt;⚙ フィルター設定を開く&lt;/button&gt;
      &lt;/div&gt;
    &lt;/div&gt;
  &lt;/div&gt;

  &lt;!-- ===== INPUT / FILTER GROUP ===== --&gt;
  &lt;div&gt;
    &lt;div class=&quot;section-label&quot;&gt;入力・フィルター系&lt;/div&gt;
    &lt;div class=&quot;demo-grid&quot;&gt;

      &lt;!-- SLIDER --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;スライダー — 期間・金額フィルター&lt;/div&gt;
        &lt;div class=&quot;slider-wrap&quot;&gt;
          &lt;div class=&quot;slider-label&quot;&gt;&lt;span&gt;表示期間&lt;/span&gt;&lt;span id=&quot;yearOut&quot;&gt;2023 〜 2025&lt;/span&gt;&lt;/div&gt;
          &lt;input type=&quot;range&quot; min=&quot;2020&quot; max=&quot;2026&quot; value=&quot;2023&quot; id=&quot;yearSlider&quot; oninput=&quot;updateYear(this.value)&quot;&gt;
        &lt;/div&gt;
        &lt;div class=&quot;slider-wrap&quot; style=&quot;margin-top:16px;&quot;&gt;
          &lt;div class=&quot;slider-label&quot;&gt;&lt;span&gt;損害額の上限&lt;/span&gt;&lt;span id=&quot;amtOut&quot;&gt;¥50,000,000&lt;/span&gt;&lt;/div&gt;
          &lt;input type=&quot;range&quot; min=&quot;0&quot; max=&quot;100&quot; value=&quot;50&quot; id=&quot;amtSlider&quot; oninput=&quot;updateAmt(this.value)&quot;&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- TOGGLE + CHECKBOX --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;チェックボックス・トグル — 表示項目 on/off&lt;/div&gt;
        &lt;div class=&quot;toggle-row&quot;&gt;
          &lt;span class=&quot;toggle-label&quot;&gt;テレマティクス費目を表示&lt;/span&gt;
          &lt;label class=&quot;toggle&quot;&gt;&lt;input type=&quot;checkbox&quot; checked&gt;&lt;span class=&quot;toggle-slider&quot;&gt;&lt;/span&gt;&lt;/label&gt;
        &lt;/div&gt;
        &lt;div class=&quot;toggle-row&quot;&gt;
          &lt;span class=&quot;toggle-label&quot;&gt;前年比較を重ねる&lt;/span&gt;
          &lt;label class=&quot;toggle&quot;&gt;&lt;input type=&quot;checkbox&quot;&gt;&lt;span class=&quot;toggle-slider&quot;&gt;&lt;/span&gt;&lt;/label&gt;
        &lt;/div&gt;
        &lt;div class=&quot;toggle-row&quot;&gt;
          &lt;span class=&quot;toggle-label&quot;&gt;異常値アラートを表示&lt;/span&gt;
          &lt;label class=&quot;toggle&quot;&gt;&lt;input type=&quot;checkbox&quot; checked&gt;&lt;span class=&quot;toggle-slider&quot;&gt;&lt;/span&gt;&lt;/label&gt;
        &lt;/div&gt;
        &lt;div style=&quot;margin-top:12px;padding-top:12px;border-top:1px solid var(--border);&quot;&gt;
          &lt;div class=&quot;checkbox-row&quot;&gt;&lt;input type=&quot;checkbox&quot; class=&quot;custom-cb&quot; checked&gt;&lt;span style=&quot;font-size:13px;color:var(--text-sub)&quot;&gt;Aioi Nissay Dowa&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;checkbox-row&quot;&gt;&lt;input type=&quot;checkbox&quot; class=&quot;custom-cb&quot; checked&gt;&lt;span style=&quot;font-size:13px;color:var(--text-sub)&quot;&gt;三井住友海上&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;checkbox-row&quot;&gt;&lt;input type=&quot;checkbox&quot; class=&quot;custom-cb&quot;&gt;&lt;span style=&quot;font-size:13px;color:var(--text-sub)&quot;&gt;第一生命&lt;/span&gt;&lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- SELECT --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;セレクトボックス — 部門・商品カテゴリ切り替え&lt;/div&gt;
        &lt;label style=&quot;font-size:11px;color:var(--text-muted);display:block;margin-bottom:4px;&quot;&gt;表示事業部&lt;/label&gt;
        &lt;select class=&quot;custom-select&quot;&gt;
          &lt;option&gt;保険事業部（全体）&lt;/option&gt;
          &lt;option&gt;テレマティクス推進G&lt;/option&gt;
          &lt;option&gt;ヘルスケア保険G&lt;/option&gt;
          &lt;option&gt;マイクロ保険G&lt;/option&gt;
          &lt;option&gt;不正検知推進室&lt;/option&gt;
        &lt;/select&gt;
        &lt;label style=&quot;font-size:11px;color:var(--text-muted);display:block;margin-bottom:4px;margin-top:12px;&quot;&gt;パートナー&lt;/label&gt;
        &lt;select class=&quot;custom-select&quot;&gt;
          &lt;option&gt;すべて&lt;/option&gt;
          &lt;option&gt;Aioi Nissay Dowa&lt;/option&gt;
          &lt;option&gt;三井住友海上&lt;/option&gt;
          &lt;option&gt;第一生命&lt;/option&gt;
          &lt;option&gt;JMDC / Pep Up&lt;/option&gt;
          &lt;option&gt;RegTech Edge&lt;/option&gt;
        &lt;/select&gt;
      &lt;/div&gt;

      &lt;!-- SEARCH --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;検索ボックス — テーブルのリアルタイム絞り込み&lt;/div&gt;
        &lt;div class=&quot;search-wrap&quot;&gt;
          &lt;span class=&quot;search-icon&quot;&gt;🔍&lt;/span&gt;
          &lt;input type=&quot;text&quot; placeholder=&quot;提案名・担当者で検索…&quot; id=&quot;searchInput&quot; oninput=&quot;filterTable()&quot;&gt;
        &lt;/div&gt;
        &lt;div class=&quot;search-results&quot; id=&quot;searchResults&quot;&gt;全 6 件表示中&lt;/div&gt;
        &lt;table class=&quot;sort-table&quot; style=&quot;margin-top:8px;&quot; id=&quot;searchTable&quot;&gt;
          &lt;thead&gt;&lt;tr&gt;
            &lt;th&gt;提案名&lt;/th&gt;&lt;th&gt;担当&lt;/th&gt;&lt;th&gt;スコア&lt;/th&gt;
          &lt;/tr&gt;&lt;/thead&gt;
          &lt;tbody id=&quot;searchBody&quot;&gt;
            &lt;tr&gt;&lt;td&gt;テレマティクス料率最適化&lt;/td&gt;&lt;td&gt;田中&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;88&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;健診連動型生命保険&lt;/td&gt;&lt;td&gt;山田&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;76&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;マイクロ農業保険&lt;/td&gt;&lt;td&gt;佐藤&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;71&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;EV向けカーシェア保険&lt;/td&gt;&lt;td&gt;鈴木&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;65&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;不正請求検知AI&lt;/td&gt;&lt;td&gt;田中&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;90&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;ウェルネス積立保険&lt;/td&gt;&lt;td&gt;中村&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;82&lt;/td&gt;&lt;/tr&gt;
          &lt;/tbody&gt;
        &lt;/table&gt;
      &lt;/div&gt;
    &lt;/div&gt;
  &lt;/div&gt;

  &lt;!-- ===== DISPLAY / LAYOUT GROUP ===== --&gt;
  &lt;div&gt;
    &lt;div class=&quot;section-label&quot;&gt;表示・レイアウト系&lt;/div&gt;
    &lt;div class=&quot;demo-grid&quot;&gt;

      &lt;!-- SORTABLE TABLE --&gt;
      &lt;div class=&quot;demo-box&quot; style=&quot;grid-column:1/-1;&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;ソート可能テーブル — ヘッダークリックで並び替え&lt;/div&gt;
        &lt;table class=&quot;sort-table&quot; id=&quot;sortTable&quot;&gt;
          &lt;thead&gt;
            &lt;tr&gt;
              &lt;th onclick=&quot;sortTable(0)&quot; data-col=&quot;0&quot;&gt;費目 &lt;span class=&quot;sort-arrow&quot;&gt;↕&lt;/span&gt;&lt;/th&gt;
              &lt;th onclick=&quot;sortTable(1)&quot; data-col=&quot;1&quot;&gt;2023年 &lt;span class=&quot;sort-arrow&quot;&gt;↕&lt;/span&gt;&lt;/th&gt;
              &lt;th onclick=&quot;sortTable(2)&quot; data-col=&quot;2&quot;&gt;2024年 &lt;span class=&quot;sort-arrow&quot;&gt;↕&lt;/span&gt;&lt;/th&gt;
              &lt;th onclick=&quot;sortTable(3)&quot; data-col=&quot;3&quot;&gt;前年比 &lt;span class=&quot;sort-arrow&quot;&gt;↕&lt;/span&gt;&lt;/th&gt;
            &lt;/tr&gt;
          &lt;/thead&gt;
          &lt;tbody&gt;
            &lt;tr&gt;&lt;td&gt;システム開発費&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;42,000&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;58,500&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;+39%&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;データ基盤構築&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;18,000&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;21,000&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;+17%&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;パートナー連携費&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;8,500&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;12,300&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;+45%&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;保険料支払&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;120,000&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;115,000&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;−4%&lt;/td&gt;&lt;/tr&gt;
            &lt;tr&gt;&lt;td&gt;調査・検証費&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;5,200&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;6,800&lt;/td&gt;&lt;td class=&quot;num&quot;&gt;+31%&lt;/td&gt;&lt;/tr&gt;
          &lt;/tbody&gt;
        &lt;/table&gt;
      &lt;/div&gt;

      &lt;!-- CARD LAYOUT --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;カード型レイアウト — 費目ごとに独立表示&lt;/div&gt;
        &lt;div class=&quot;card-grid&quot;&gt;
          &lt;div class=&quot;card&quot;&gt;&lt;div class=&quot;card-cat&quot;&gt;テレマ&lt;/div&gt;&lt;div class=&quot;card-name&quot;&gt;システム費&lt;/div&gt;&lt;div class=&quot;card-val&quot;&gt;¥58.5M&lt;/div&gt;&lt;div class=&quot;card-sub&quot;&gt;前年比 +39%&lt;/div&gt;&lt;/div&gt;
          &lt;div class=&quot;card&quot;&gt;&lt;div class=&quot;card-cat&quot;&gt;ヘルス&lt;/div&gt;&lt;div class=&quot;card-name&quot;&gt;データ基盤&lt;/div&gt;&lt;div class=&quot;card-val&quot;&gt;¥21.0M&lt;/div&gt;&lt;div class=&quot;card-sub&quot;&gt;前年比 +17%&lt;/div&gt;&lt;/div&gt;
          &lt;div class=&quot;card&quot;&gt;&lt;div class=&quot;card-cat&quot;&gt;連携&lt;/div&gt;&lt;div class=&quot;card-name&quot;&gt;パートナー費&lt;/div&gt;&lt;div class=&quot;card-val&quot;&gt;¥12.3M&lt;/div&gt;&lt;div class=&quot;card-sub&quot;&gt;前年比 +45%&lt;/div&gt;&lt;/div&gt;
          &lt;div class=&quot;card&quot;&gt;&lt;div class=&quot;card-cat&quot;&gt;損害&lt;/div&gt;&lt;div class=&quot;card-name&quot;&gt;保険料支払&lt;/div&gt;&lt;div class=&quot;card-val&quot;&gt;¥115M&lt;/div&gt;&lt;div class=&quot;card-sub&quot;&gt;前年比 −4%&lt;/div&gt;&lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- PROGRESS BAR --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;プログレスバー — 予算消化率・目標達成率&lt;/div&gt;
        &lt;div class=&quot;progress-item&quot;&gt;
          &lt;div class=&quot;progress-header&quot;&gt;&lt;span&gt;テレマティクス予算消化&lt;/span&gt;&lt;span&gt;78%&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;progress-track&quot;&gt;&lt;div class=&quot;progress-fill yellow&quot; style=&quot;width:78%&quot;&gt;&lt;/div&gt;&lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;progress-item&quot;&gt;
          &lt;div class=&quot;progress-header&quot;&gt;&lt;span&gt;新契約件数 目標達成率&lt;/span&gt;&lt;span&gt;112%&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;progress-track&quot;&gt;&lt;div class=&quot;progress-fill green&quot; style=&quot;width:100%&quot;&gt;&lt;/div&gt;&lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;progress-item&quot;&gt;
          &lt;div class=&quot;progress-header&quot;&gt;&lt;span&gt;不正検知 精度目標&lt;/span&gt;&lt;span&gt;62%&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;progress-track&quot;&gt;&lt;div class=&quot;progress-fill red&quot; style=&quot;width:62%&quot;&gt;&lt;/div&gt;&lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;progress-item&quot;&gt;
          &lt;div class=&quot;progress-header&quot;&gt;&lt;span&gt;Pep Up 連携スコープ&lt;/span&gt;&lt;span&gt;45%&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;progress-track&quot;&gt;&lt;div class=&quot;progress-fill&quot; style=&quot;width:45%&quot;&gt;&lt;/div&gt;&lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;

    &lt;/div&gt;

    &lt;!-- STEPPER + TIMELINE side by side --&gt;
    &lt;div class=&quot;demo-grid&quot; style=&quot;margin-top:16px;&quot;&gt;

      &lt;!-- STEPPER --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;ステッパー — 申請フローを段階的に案内&lt;/div&gt;
        &lt;div class=&quot;stepper&quot; id=&quot;stepper&quot;&gt;
          &lt;div class=&quot;step-item&quot;&gt;
            &lt;div class=&quot;step-left&quot;&gt;
              &lt;div class=&quot;step-dot done&quot; id=&quot;s1&quot;&gt;✓&lt;/div&gt;
              &lt;div class=&quot;step-line done&quot;&gt;&lt;/div&gt;
            &lt;/div&gt;
            &lt;div class=&quot;step-body&quot;&gt;
              &lt;div class=&quot;step-title&quot;&gt;基本情報の入力&lt;/div&gt;
              &lt;div class=&quot;step-desc&quot;&gt;提案名・担当部署・パートナー&lt;/div&gt;
            &lt;/div&gt;
          &lt;/div&gt;
          &lt;div class=&quot;step-item&quot;&gt;
            &lt;div class=&quot;step-left&quot;&gt;
              &lt;div class=&quot;step-dot done&quot; id=&quot;s2&quot;&gt;✓&lt;/div&gt;
              &lt;div class=&quot;step-line done&quot;&gt;&lt;/div&gt;
            &lt;/div&gt;
            &lt;div class=&quot;step-body&quot;&gt;
              &lt;div class=&quot;step-title&quot;&gt;概算コスト入力&lt;/div&gt;
              &lt;div class=&quot;step-desc&quot;&gt;初期投資・ランニングコスト&lt;/div&gt;
            &lt;/div&gt;
          &lt;/div&gt;
          &lt;div class=&quot;step-item&quot;&gt;
            &lt;div class=&quot;step-left&quot;&gt;
              &lt;div class=&quot;step-dot active&quot; id=&quot;s3&quot;&gt;3&lt;/div&gt;
              &lt;div class=&quot;step-line&quot;&gt;&lt;/div&gt;
            &lt;/div&gt;
            &lt;div class=&quot;step-body&quot;&gt;
              &lt;div class=&quot;step-title&quot; style=&quot;color:var(--accent)&quot;&gt;審査書類のアップロード&lt;/div&gt;
              &lt;div class=&quot;step-desc&quot;&gt;事業計画書・リスク評価シート&lt;/div&gt;
            &lt;/div&gt;
          &lt;/div&gt;
          &lt;div class=&quot;step-item&quot;&gt;
            &lt;div class=&quot;step-left&quot;&gt;
              &lt;div class=&quot;step-dot&quot; id=&quot;s4&quot;&gt;4&lt;/div&gt;
            &lt;/div&gt;
            &lt;div class=&quot;step-body&quot;&gt;
              &lt;div class=&quot;step-title&quot; style=&quot;color:var(--text-muted)&quot;&gt;上長承認&lt;/div&gt;
              &lt;div class=&quot;step-desc&quot; style=&quot;color:var(--text-muted)&quot;&gt;事業部長・本部長のサイン&lt;/div&gt;
            &lt;/div&gt;
          &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;stepper-actions&quot;&gt;
          &lt;button class=&quot;btn btn-ghost btn-sm&quot; onclick=&quot;stepBack()&quot;&gt;← 戻る&lt;/button&gt;
          &lt;button class=&quot;btn btn-primary btn-sm&quot; onclick=&quot;stepNext()&quot;&gt;次へ →&lt;/button&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- TIMELINE --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;タイムライン — プロジェクト進捗記録&lt;/div&gt;
        &lt;div class=&quot;timeline&quot;&gt;
          &lt;div class=&quot;tl-item&quot;&gt;
            &lt;div class=&quot;tl-dot green&quot;&gt;&lt;/div&gt;
            &lt;div class=&quot;tl-date&quot;&gt;2025.04.01&lt;/div&gt;
            &lt;div class=&quot;tl-title&quot;&gt;PoC開始 — テレマティクス&lt;/div&gt;
            &lt;div class=&quot;tl-desc&quot;&gt;Aioi NDとの共同実証実験キックオフ。OBD端末50台配布。&lt;/div&gt;
          &lt;/div&gt;
          &lt;div class=&quot;tl-item&quot;&gt;
            &lt;div class=&quot;tl-dot&quot;&gt;&lt;/div&gt;
            &lt;div class=&quot;tl-date&quot;&gt;2025.06.15&lt;/div&gt;
            &lt;div class=&quot;tl-title&quot;&gt;中間レビュー&lt;/div&gt;
            &lt;div class=&quot;tl-desc&quot;&gt;損害率改善の初期傾向確認。モデル精度 AUC 0.82 達成。&lt;/div&gt;
          &lt;/div&gt;
          &lt;div class=&quot;tl-item&quot;&gt;
            &lt;div class=&quot;tl-dot yellow&quot;&gt;&lt;/div&gt;
            &lt;div class=&quot;tl-date&quot;&gt;2025.09.01&lt;/div&gt;
            &lt;div class=&quot;tl-title&quot;&gt;Pep Up 連携 API 接続&lt;/div&gt;
            &lt;div class=&quot;tl-desc&quot;&gt;ヘルスデータ連携の認証フロー確立。個人情報取扱同意率67%。&lt;/div&gt;
          &lt;/div&gt;
          &lt;div class=&quot;tl-item&quot;&gt;
            &lt;div class=&quot;tl-dot muted&quot;&gt;&lt;/div&gt;
            &lt;div class=&quot;tl-date&quot;&gt;2025.12.01 予定&lt;/div&gt;
            &lt;div class=&quot;tl-title&quot;&gt;本番リリース判定&lt;/div&gt;
            &lt;div class=&quot;tl-desc&quot;&gt;経営会議へ最終報告。承認後 2026Q1 正式ローンチ。&lt;/div&gt;
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;

    &lt;/div&gt;
  &lt;/div&gt;

  &lt;!-- ===== NOTIFICATION / FEEDBACK GROUP ===== --&gt;
  &lt;div&gt;
    &lt;div class=&quot;section-label&quot;&gt;通知・フィードバック系&lt;/div&gt;
    &lt;div class=&quot;demo-grid triple&quot;&gt;

      &lt;!-- TOAST --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;トースト通知&lt;/div&gt;
        &lt;div class=&quot;toast-buttons&quot;&gt;
          &lt;button class=&quot;btn btn-ghost btn-sm&quot; onclick=&quot;showToast(&#x27;success&#x27;,&#x27;✅&#x27;,&#x27;保存しました&#x27;)&quot;&gt;保存&lt;/button&gt;
          &lt;button class=&quot;btn btn-ghost btn-sm&quot; onclick=&quot;showToast(&#x27;warn&#x27;,&#x27;⚠️&#x27;,&#x27;上限に近づいています&#x27;)&quot;&gt;警告&lt;/button&gt;
          &lt;button class=&quot;btn btn-ghost btn-sm&quot; onclick=&quot;showToast(&#x27;error&#x27;,&#x27;❌&#x27;,&#x27;送信に失敗しました&#x27;)&quot;&gt;エラー&lt;/button&gt;
          &lt;button class=&quot;btn btn-ghost btn-sm&quot; onclick=&quot;showToast(&#x27;info&#x27;,&#x27;📋&#x27;,&#x27;コピーしました&#x27;)&quot;&gt;コピー&lt;/button&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- BADGE --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;バッジ・ラベル&lt;/div&gt;
        &lt;div class=&quot;badge-demo&quot; style=&quot;margin-bottom:12px;&quot;&gt;
          &lt;span class=&quot;badge green&quot;&gt;&lt;span class=&quot;badge-dot&quot;&gt;&lt;/span&gt;承認済&lt;/span&gt;
          &lt;span class=&quot;badge red&quot;&gt;&lt;span class=&quot;badge-dot&quot;&gt;&lt;/span&gt;却下&lt;/span&gt;
          &lt;span class=&quot;badge yellow&quot;&gt;&lt;span class=&quot;badge-dot&quot;&gt;&lt;/span&gt;審査中&lt;/span&gt;
          &lt;span class=&quot;badge blue&quot;&gt;&lt;span class=&quot;badge-dot&quot;&gt;&lt;/span&gt;新規&lt;/span&gt;
          &lt;span class=&quot;badge gray&quot;&gt;&lt;span class=&quot;badge-dot&quot;&gt;&lt;/span&gt;保留&lt;/span&gt;
        &lt;/div&gt;
        &lt;div class=&quot;badge-demo&quot;&gt;
          &lt;div class=&quot;badge-icon-wrap&quot;&gt;🔔&lt;span class=&quot;badge-num&quot;&gt;5&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;badge-icon-wrap&quot;&gt;📨&lt;span class=&quot;badge-num&quot;&gt;12&lt;/span&gt;&lt;/div&gt;
          &lt;div class=&quot;badge-icon-wrap&quot;&gt;📋&lt;span class=&quot;badge-num&quot;&gt;3&lt;/span&gt;&lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;!-- STATUS INDICATOR --&gt;
      &lt;div class=&quot;demo-box&quot;&gt;
        &lt;div class=&quot;demo-title&quot;&gt;ステータスインジケーター&lt;/div&gt;
        &lt;div class=&quot;status-row&quot;&gt;
          &lt;span class=&quot;status-name&quot;&gt;テレマティクス連携&lt;/span&gt;
          &lt;div class=&quot;status-indicators&quot;&gt;
            &lt;span class=&quot;status-val&quot;&gt;稼働中&lt;/span&gt;
            &lt;span class=&quot;status-pill ok&quot;&gt;正常&lt;/span&gt;
          &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;status-row&quot;&gt;
          &lt;span class=&quot;status-name&quot;&gt;Pep Up API&lt;/span&gt;
          &lt;div class=&quot;status-indicators&quot;&gt;
            &lt;span class=&quot;status-val&quot;&gt;部分障害&lt;/span&gt;
            &lt;span class=&quot;status-pill warn&quot;&gt;警告&lt;/span&gt;
          &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;status-row&quot;&gt;
          &lt;span class=&quot;status-name&quot;&gt;不正検知モデル&lt;/span&gt;
          &lt;div class=&quot;status-indicators&quot;&gt;
            &lt;span class=&quot;status-val&quot;&gt;停止中&lt;/span&gt;
            &lt;span class=&quot;status-pill ng&quot;&gt;異常&lt;/span&gt;
          &lt;/div&gt;
        &lt;/div&gt;
        &lt;div class=&quot;status-row&quot;&gt;
          &lt;span class=&quot;status-name&quot;&gt;RegTech Edge&lt;/span&gt;
          &lt;div class=&quot;status-indicators&quot;&gt;
            &lt;span class=&quot;status-val&quot;&gt;未接続&lt;/span&gt;
            &lt;span class=&quot;status-pill idle&quot;&gt;待機&lt;/span&gt;
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;

    &lt;/div&gt;
  &lt;/div&gt;

&lt;/div&gt;

&lt;!-- MODAL --&gt;
&lt;div class=&quot;modal-overlay&quot; id=&quot;modalOverlay&quot; onclick=&quot;if(event.target===this)closeModal()&quot;&gt;
  &lt;div class=&quot;modal&quot;&gt;
    &lt;button class=&quot;modal-close&quot; onclick=&quot;closeModal()&quot;&gt;✕&lt;/button&gt;
    &lt;h3 id=&quot;modalTitle&quot;&gt;2025年3月 — 費用明細&lt;/h3&gt;
    &lt;p&gt;クリックした月の費用明細一覧です。&lt;/p&gt;
    &lt;table class=&quot;modal-table&quot;&gt;
      &lt;thead&gt;&lt;tr&gt;&lt;th&gt;費目&lt;/th&gt;&lt;th&gt;金額 (千円)&lt;/th&gt;&lt;th&gt;カテゴリ&lt;/th&gt;&lt;/tr&gt;&lt;/thead&gt;
      &lt;tbody&gt;
        &lt;tr&gt;&lt;td&gt;システム開発&lt;/td&gt;&lt;td&gt;18,500&lt;/td&gt;&lt;td&gt;&lt;span class=&quot;badge blue&quot; style=&quot;font-size:10px&quot;&gt;開発&lt;/span&gt;&lt;/td&gt;&lt;/tr&gt;
        &lt;tr&gt;&lt;td&gt;パートナー連携費&lt;/td&gt;&lt;td&gt;4,200&lt;/td&gt;&lt;td&gt;&lt;span class=&quot;badge green&quot; style=&quot;font-size:10px&quot;&gt;連携&lt;/span&gt;&lt;/td&gt;&lt;/tr&gt;
        &lt;tr&gt;&lt;td&gt;保険料支払&lt;/td&gt;&lt;td&gt;38,000&lt;/td&gt;&lt;td&gt;&lt;span class=&quot;badge yellow&quot; style=&quot;font-size:10px&quot;&gt;保険&lt;/span&gt;&lt;/td&gt;&lt;/tr&gt;
        &lt;tr&gt;&lt;td&gt;人件費（間接）&lt;/td&gt;&lt;td&gt;6,800&lt;/td&gt;&lt;td&gt;&lt;span class=&quot;badge gray&quot; style=&quot;font-size:10px&quot;&gt;内部&lt;/span&gt;&lt;/td&gt;&lt;/tr&gt;
      &lt;/tbody&gt;
    &lt;/table&gt;
    &lt;div class=&quot;modal-actions&quot;&gt;
      &lt;button class=&quot;btn btn-ghost&quot; onclick=&quot;closeModal()&quot;&gt;閉じる&lt;/button&gt;
      &lt;button class=&quot;btn btn-primary&quot; onclick=&quot;showToast(&#x27;info&#x27;,&#x27;📋&#x27;,&#x27;CSVをコピーしました&#x27;);closeModal()&quot;&gt;CSVコピー&lt;/button&gt;
    &lt;/div&gt;
  &lt;/div&gt;
&lt;/div&gt;

&lt;!-- DRAWER --&gt;
&lt;div class=&quot;drawer-overlay&quot; id=&quot;drawerOverlay&quot; onclick=&quot;closeDrawer()&quot;&gt;&lt;/div&gt;
&lt;div class=&quot;drawer&quot; id=&quot;drawer&quot;&gt;
  &lt;h3&gt;
    フィルター設定
    &lt;button class=&quot;drawer-close&quot; onclick=&quot;closeDrawer()&quot;&gt;✕&lt;/button&gt;
  &lt;/h3&gt;
  &lt;div class=&quot;drawer-section&quot;&gt;
    &lt;label&gt;表示年度&lt;/label&gt;
    &lt;select class=&quot;custom-select&quot;&gt;
      &lt;option&gt;2024年度&lt;/option&gt;
      &lt;option&gt;2023年度&lt;/option&gt;
      &lt;option&gt;2022年度&lt;/option&gt;
    &lt;/select&gt;
  &lt;/div&gt;
  &lt;div class=&quot;drawer-section&quot;&gt;
    &lt;label&gt;集計単位&lt;/label&gt;
    &lt;select class=&quot;custom-select&quot;&gt;
      &lt;option&gt;月次&lt;/option&gt;
      &lt;option&gt;四半期&lt;/option&gt;
      &lt;option&gt;年次&lt;/option&gt;
    &lt;/select&gt;
  &lt;/div&gt;
  &lt;div class=&quot;drawer-section&quot;&gt;
    &lt;label&gt;表示費目&lt;/label&gt;
    &lt;div class=&quot;checkbox-row&quot;&gt;&lt;input type=&quot;checkbox&quot; class=&quot;custom-cb&quot; checked&gt;&lt;span style=&quot;font-size:13px;color:var(--text-sub)&quot;&gt;システム費&lt;/span&gt;&lt;/div&gt;
    &lt;div class=&quot;checkbox-row&quot;&gt;&lt;input type=&quot;checkbox&quot; class=&quot;custom-cb&quot; checked&gt;&lt;span style=&quot;font-size:13px;color:var(--text-sub)&quot;&gt;パートナー費&lt;/span&gt;&lt;/div&gt;
    &lt;div class=&quot;checkbox-row&quot;&gt;&lt;input type=&quot;checkbox&quot; class=&quot;custom-cb&quot;&gt;&lt;span style=&quot;font-size:13px;color:var(--text-sub)&quot;&gt;間接人件費&lt;/span&gt;&lt;/div&gt;
    &lt;div class=&quot;checkbox-row&quot;&gt;&lt;input type=&quot;checkbox&quot; class=&quot;custom-cb&quot; checked&gt;&lt;span style=&quot;font-size:13px;color:var(--text-sub)&quot;&gt;保険料支払&lt;/span&gt;&lt;/div&gt;
  &lt;/div&gt;
  &lt;div class=&quot;drawer-section&quot;&gt;
    &lt;label&gt;損害額上限フィルター&lt;/label&gt;
    &lt;input type=&quot;range&quot; min=&quot;0&quot; max=&quot;100&quot; value=&quot;80&quot; style=&quot;width:100%&quot;&gt;
    &lt;div style=&quot;text-align:center;font-family:&#x27;DM Mono&#x27;,monospace;font-size:12px;color:var(--accent);margin-top:4px;&quot;&gt;¥80,000,000&lt;/div&gt;
  &lt;/div&gt;
  &lt;button class=&quot;btn btn-primary&quot; style=&quot;width:100%;margin-top:8px;&quot; onclick=&quot;showToast(&#x27;success&#x27;,&#x27;✅&#x27;,&#x27;フィルターを適用しました&#x27;);closeDrawer()&quot;&gt;適用する&lt;/button&gt;
&lt;/div&gt;

&lt;!-- TOAST CONTAINER --&gt;
&lt;div class=&quot;toast-container&quot; id=&quot;toastContainer&quot;&gt;&lt;/div&gt;

&lt;script&gt;
  // ACCORDION
  function toggleAccordion(btn) {
    const body = btn.nextElementSibling;
    const icon = btn.querySelector(&#x27;.accordion-icon&#x27;);
    const isOpen = body.classList.contains(&#x27;open&#x27;);
    document.querySelectorAll(&#x27;.accordion-body&#x27;).forEach(b =&gt; b.classList.remove(&#x27;open&#x27;));
    document.querySelectorAll(&#x27;.accordion-icon&#x27;).forEach(i =&gt; i.classList.remove(&#x27;open&#x27;));
    if (!isOpen) { body.classList.add(&#x27;open&#x27;); icon.classList.add(&#x27;open&#x27;); }
  }

  // MODAL
  function openModal(month) {
    document.getElementById(&#x27;modalTitle&#x27;).textContent = month + &#x27; — 費用明細&#x27;;
    document.getElementById(&#x27;modalOverlay&#x27;).classList.add(&#x27;open&#x27;);
  }
  function closeModal() { document.getElementById(&#x27;modalOverlay&#x27;).classList.remove(&#x27;open&#x27;); }

  // DRAWER
  function openDrawer() {
    document.getElementById(&#x27;drawerOverlay&#x27;).classList.add(&#x27;open&#x27;);
    document.getElementById(&#x27;drawer&#x27;).classList.add(&#x27;open&#x27;);
  }
  function closeDrawer() {
    document.getElementById(&#x27;drawerOverlay&#x27;).classList.remove(&#x27;open&#x27;);
    document.getElementById(&#x27;drawer&#x27;).classList.remove(&#x27;open&#x27;);
  }

  // SLIDER
  function updateYear(v) { document.getElementById(&#x27;yearOut&#x27;).textContent = v + &#x27; 〜 2025&#x27;; }
  function updateAmt(v) {
    document.getElementById(&#x27;amtOut&#x27;).textContent = &#x27;¥&#x27; + (v * 1000000).toLocaleString();
  }

  // SEARCH
  function filterTable() {
    const q = document.getElementById(&#x27;searchInput&#x27;).value.toLowerCase();
    const rows = document.querySelectorAll(&#x27;#searchBody tr&#x27;);
    let count = 0;
    rows.forEach(r =&gt; {
      const match = r.textContent.toLowerCase().includes(q);
      r.style.display = match ? &#x27;&#x27; : &#x27;none&#x27;;
      if (match) count++;
    });
    document.getElementById(&#x27;searchResults&#x27;).textContent = `${count} 件表示中`;
  }

  // SORT TABLE
  let sortDir = {};
  function sortTable(col) {
    const table = document.getElementById(&#x27;sortTable&#x27;);
    const tbody = table.tBodies[0];
    const rows = Array.from(tbody.rows);
    sortDir[col] = sortDir[col] === &#x27;asc&#x27; ? &#x27;desc&#x27; : &#x27;asc&#x27;;
    rows.sort((a, b) =&gt; {
      const av = a.cells[col].textContent.replace(/[,+\-%¥]/g, &#x27;&#x27;);
      const bv = b.cells[col].textContent.replace(/[,+\-%¥]/g, &#x27;&#x27;);
      const an = parseFloat(av), bn = parseFloat(bv);
      const cmp = isNaN(an) ? av.localeCompare(bv) : an - bn;
      return sortDir[col] === &#x27;asc&#x27; ? cmp : -cmp;
    });
    rows.forEach(r =&gt; tbody.appendChild(r));
    table.querySelectorAll(&#x27;th&#x27;).forEach((th, i) =&gt; {
      th.classList.toggle(&#x27;active&#x27;, i === col);
      th.querySelector(&#x27;.sort-arrow&#x27;).textContent = i === col ? (sortDir[col] === &#x27;asc&#x27; ? &#x27;↑&#x27; : &#x27;↓&#x27;) : &#x27;↕&#x27;;
    });
  }

  // STEPPER
  let currentStep = 3;
  function stepNext() {
    if (currentStep &gt;= 4) { showToast(&#x27;success&#x27;,&#x27;✅&#x27;,&#x27;申請を送信しました&#x27;); return; }
    currentStep++;
    updateStepper();
  }
  function stepBack() {
    if (currentStep &lt;= 1) return;
    currentStep--;
    updateStepper();
  }
  function updateStepper() {
    for (let i = 1; i &lt;= 4; i++) {
      const dot = document.getElementById(&#x27;s&#x27; + i);
      dot.className = &#x27;step-dot&#x27;;
      if (i &lt; currentStep) { dot.classList.add(&#x27;done&#x27;); dot.textContent = &#x27;✓&#x27;; }
      else if (i === currentStep) { dot.classList.add(&#x27;active&#x27;); dot.textContent = i; }
      else { dot.textContent = i; }
    }
    const lines = document.querySelectorAll(&#x27;.step-line&#x27;);
    lines.forEach((l, i) =&gt; { l.classList.toggle(&#x27;done&#x27;, i + 1 &lt; currentStep); });
  }

  // TOAST
  function showToast(type, icon, msg) {
    const container = document.getElementById(&#x27;toastContainer&#x27;);
    const toast = document.createElement(&#x27;div&#x27;);
    toast.className = &#x27;toast&#x27;;
    toast.innerHTML = `&lt;span class=&quot;toast-icon&quot;&gt;${icon}&lt;/span&gt;&lt;span&gt;${msg}&lt;/span&gt;&lt;button class=&quot;toast-close&quot; onclick=&quot;removeToast(this.parentElement)&quot;&gt;✕&lt;/button&gt;`;
    container.appendChild(toast);
    setTimeout(() =&gt; removeToast(toast), 3200);
  }
  function removeToast(el) {
    if (!el || !el.parentElement) return;
    el.classList.add(&#x27;removing&#x27;);
    setTimeout(() =&gt; el.remove(), 300);
  }
&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
" width="100%" height="950" frameborder="0" style="border:none; display:block;"></iframe>
