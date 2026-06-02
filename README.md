<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>Ops Dashboard</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.10.0/dist/tabler-icons.min.css"/>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#0f1117;--bg2:#181c27;--surface:#1e2336;--surface2:#252b40;
  --border:rgba(255,255,255,0.08);--border2:rgba(255,255,255,0.15);
  --text:#f0f2f8;--muted:#8b92b0;--accent:#4f8ef7;--accent2:#34c98a;
  --warn:#f5a623;--danger:#e74c3c;--purple:#9b6dff;
  --radius:10px;--radius-lg:16px;
}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;padding:0;}
.topbar{background:var(--bg2);border-bottom:1px solid var(--border);padding:14px 28px;display:flex;align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap;position:sticky;top:0;z-index:100;}
.topbar-left{display:flex;align-items:center;gap:14px;}
.logo{font-size:18px;font-weight:600;color:var(--accent);letter-spacing:-0.3px;display:flex;align-items:center;gap:8px;}
.logo i{font-size:20px;}
.tagline{font-size:12px;color:var(--muted);}
.topbar-right{display:flex;align-items:center;gap:10px;flex-wrap:wrap;}
#status-badge{font-size:11px;padding:4px 12px;border-radius:20px;background:rgba(52,201,138,0.15);color:var(--accent2);border:1px solid rgba(52,201,138,0.3);}
.btn{font-size:12px;padding:7px 16px;border-radius:var(--radius);border:1px solid var(--border2);background:var(--surface);color:var(--text);cursor:pointer;display:inline-flex;align-items:center;gap:6px;transition:all 0.15s;}
.btn:hover{background:var(--surface2);border-color:var(--accent);}
.btn-primary{background:var(--accent);border-color:var(--accent);color:#fff;}
.btn-primary:hover{background:#3a7de0;border-color:#3a7de0;}
.setup-panel{background:var(--bg2);border-bottom:1px solid var(--border);padding:20px 28px;}
.setup-panel h2{font-size:13px;color:var(--muted);margin-bottom:12px;text-transform:uppercase;letter-spacing:0.05em;}
.sheet-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;}
.sheet-input-wrap{display:flex;flex-direction:column;gap:4px;}
.sheet-input-wrap label{font-size:11px;color:var(--muted);}
.sheet-input-wrap input{font-size:13px;padding:8px 12px;border:1px solid var(--border2);border-radius:var(--radius);background:var(--bg);color:var(--text);outline:none;width:100%;}
.sheet-input-wrap input:focus{border-color:var(--accent);}
.sheet-actions{display:flex;gap:8px;flex-wrap:wrap;}
.main{padding:24px 28px;}
.section-title{font-size:12px;color:var(--muted);text-transform:uppercase;letter-spacing:0.06em;margin-bottom:14px;display:flex;align-items:center;gap:8px;}
.section-title::after{content:'';flex:1;height:1px;background:var(--border);}
.metrics{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;margin-bottom:28px;}
.metric{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-lg);padding:18px 20px;position:relative;overflow:hidden;}
.metric::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;background:var(--accent-color,var(--accent));}
.metric .icon{font-size:22px;color:var(--accent-color,var(--accent));margin-bottom:10px;opacity:0.85;}
.metric .lbl{font-size:11px;color:var(--muted);margin-bottom:6px;text-transform:uppercase;letter-spacing:0.04em;}
.metric .val{font-size:32px;font-weight:600;line-height:1;}
.metric .sub{font-size:11px;color:var(--muted);margin-top:6px;}
.charts-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:16px;margin-bottom:28px;}
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-lg);padding:20px 22px;}
.card-title{font-size:13px;font-weight:500;color:var(--muted);margin-bottom:16px;display:flex;align-items:center;gap:8px;}
.card-title i{font-size:16px;color:var(--accent);}
.filter-row{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:16px;align-items:center;}
.filter-row select{font-size:12px;padding:6px 12px;border:1px solid var(--border2);border-radius:var(--radius);background:var(--surface2);color:var(--text);cursor:pointer;outline:none;}
.filter-row select:focus{border-color:var(--accent);}
.filter-row label{font-size:12px;color:var(--muted);}
.table-wrap{overflow-x:auto;border-radius:var(--radius);}
table{width:100%;border-collapse:collapse;font-size:13px;}
th{text-align:left;font-weight:500;font-size:11px;color:var(--muted);border-bottom:1px solid var(--border);padding:10px 12px;text-transform:uppercase;letter-spacing:0.04em;white-space:nowrap;}
td{padding:10px 12px;border-bottom:1px solid var(--border);vertical-align:middle;white-space:nowrap;}
tr:last-child td{border-bottom:none;}
tr:hover td{background:var(--surface2);}
.badge{display:inline-flex;align-items:center;gap:4px;font-size:11px;padding:3px 10px;border-radius:20px;font-weight:500;}
.badge-green{background:rgba(52,201,138,0.15);color:#34c98a;border:1px solid rgba(52,201,138,0.25);}
.badge-blue{background:rgba(79,142,247,0.15);color:#4f8ef7;border:1px solid rgba(79,142,247,0.25);}
.badge-red{background:rgba(231,76,60,0.15);color:#e74c3c;border:1px solid rgba(231,76,60,0.25);}
.badge-orange{background:rgba(245,166,35,0.15);color:#f5a623;border:1px solid rgba(245,166,35,0.25);}
.badge-purple{background:rgba(155,109,255,0.15);color:#9b6dff;border:1px solid rgba(155,109,255,0.25);}
.progress-row{display:flex;align-items:center;gap:10px;margin-bottom:10px;}
.progress-label{font-size:12px;color:var(--text);min-width:120px;max-width:120px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;}
.progress-track{flex:1;height:8px;background:var(--surface2);border-radius:4px;overflow:hidden;}
.progress-fill{height:100%;border-radius:4px;transition:width 0.6s ease;}
.progress-count{font-size:12px;color:var(--muted);min-width:30px;text-align:right;}
.tabs{display:flex;gap:4px;background:var(--bg2);border-radius:var(--radius);padding:4px;margin-bottom:20px;overflow-x:auto;}
.tab{font-size:13px;padding:8px 18px;border-radius:8px;border:none;background:transparent;color:var(--muted);cursor:pointer;white-space:nowrap;transition:all 0.15s;}
.tab.active{background:var(--surface);color:var(--text);box-shadow:0 1px 4px rgba(0,0,0,0.3);}
.tab-content{display:none;}
.tab-content.active{display:block;}
.empty{text-align:center;color:var(--muted);padding:40px;font-size:13px;}
.empty i{font-size:32px;display:block;margin-bottom:8px;opacity:0.4;}
.pe-avatar{width:32px;height:32px;border-radius:50%;background:var(--surface2);display:inline-flex;align-items:center;justify-content:center;font-size:11px;font-weight:600;color:var(--accent);flex-shrink:0;}
#loading-overlay{position:fixed;inset:0;background:var(--bg);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:16px;z-index:999;}
#loading-overlay.hidden{display:none;}
.spinner{width:40px;height:40px;border:3px solid var(--border);border-top-color:var(--accent);border-radius:50%;animation:spin 0.8s linear infinite;}
@keyframes spin{to{transform:rotate(360deg)}}
.load-txt{font-size:14px;color:var(--muted);}
@media(max-width:600px){.main{padding:16px;}.topbar{padding:12px 16px;}.sheet-grid{grid-template-columns:1fr;}}
</style>
</head>
<body>

<div id="loading-overlay" class="hidden">
  <div class="spinner"></div>
  <div class="load-txt" id="load-txt">Loading data…</div>
</div>

<div class="topbar">
  <div class="topbar-left">
    <div class="logo"><i class="ti ti-bolt"></i> Ops Dashboard</div>
    <span class="tagline">PE · Attendance · TBT · DPR</span>
  </div>
  <div class="topbar-right">
    <span id="status-badge"><i class="ti ti-circle-dot" style="font-size:10px;vertical-align:1px;"></i> Not loaded</span>
    <span id="last-updated" style="font-size:11px;color:var(--muted);"></span>
    <button class="btn btn-primary" onclick="loadAllSheets()"><i class="ti ti-refresh"></i> Refresh</button>
  </div>
</div>

<div class="setup-panel" id="setup-panel">
  <h2>Google Sheet Configuration</h2>
  <div class="sheet-grid">
    <div class="sheet-input-wrap">
      <label>Sheet ID</label>
      <input type="text" id="sheet-id" placeholder="1NuqbG_4a3bGS4Wd6dKvxsP0FN2tE6yMupkQ-1fYDQNA" value="1NuqbG_4a3bGS4Wd6dKvxsP0FN2tE6yMupkQ-1fYDQNA"/>
    </div>
    <div class="sheet-input-wrap">
      <label>Sheet tabs (GIDs — leave blank to use names)</label>
      <input type="text" id="sheet-gids" placeholder="PE Master GID, Attendance GID, TBT GID, DPR GID" value=""/>
    </div>
  </div>
  <div class="sheet-actions">
    <button class="btn btn-primary" onclick="loadAllSheets()"><i class="ti ti-download"></i> Load Data</button>
    <button class="btn" onclick="document.getElementById('setup-panel').style.display='none'"><i class="ti ti-chevron-up"></i> Hide</button>
  </div>
  <p style="font-size:11px;color:var(--muted);margin-top:10px;">Make sure the Google Sheet is set to <em>Anyone with the link can view</em>. The dashboard will auto-detect columns.</p>
</div>

<div class="main">
  <div id="dashboard" style="display:none;">

    <!-- Tabs -->
    <div class="tabs">
      <button class="tab active" onclick="switchTab('pe')"><i class="ti ti-users" style="font-size:14px;vertical-align:-2px;margin-right:5px;"></i>PE Overview</button>
      <button class="tab" onclick="switchTab('attendance')"><i class="ti ti-calendar-check" style="font-size:14px;vertical-align:-2px;margin-right:5px;"></i>Attendance</button>
      <button class="tab" onclick="switchTab('tbt')"><i class="ti ti-clipboard-list" style="font-size:14px;vertical-align:-2px;margin-right:5px;"></i>TBT</button>
      <button class="tab" onclick="switchTab('dpr')"><i class="ti ti-file-report" style="font-size:14px;vertical-align:-2px;margin-right:5px;"></i>DPR</button>
    </div>

    <!-- PE Overview Tab -->
    <div id="tab-pe" class="tab-content active">
      <div class="section-title">PE Summary</div>
      <div class="metrics" id="pe-metrics"></div>
      <div class="charts-grid">
        <div class="card" style="grid-column:span 2;">
          <div class="card-title"><i class="ti ti-table"></i> All Project Engineers</div>
          <div class="filter-row">
            <label>Dept:</label>
            <select id="pe-filter-dept" onchange="renderPETable()"><option value="">All</option></select>
            <label>Region:</label>
            <select id="pe-filter-region" onchange="renderPETable()"><option value="">All</option></select>
            <label>City:</label>
            <select id="pe-filter-city" onchange="renderPETable()"><option value="">All</option></select>
          </div>
          <div class="table-wrap"><table><thead id="pe-thead"></thead><tbody id="pe-tbody"></tbody></table></div>
        </div>
      </div>
    </div>

    <!-- Attendance Tab -->
    <div id="tab-attendance" class="tab-content">
      <div class="section-title">Attendance Summary</div>
      <div class="metrics" id="att-metrics"></div>
      <div class="charts-grid">
        <div class="card">
          <div class="card-title"><i class="ti ti-map-pin"></i> City-wise Attendance</div>
          <div id="city-chart-wrap"></div>
        </div>
        <div class="card">
          <div class="card-title"><i class="ti ti-building"></i> Project-wise Visits</div>
          <div id="project-chart-wrap"></div>
        </div>
        <div class="card" style="grid-column:1/-1;">
          <div class="card-title"><i class="ti ti-list-details"></i> Attendance Log</div>
          <div class="filter-row">
            <label>Date:</label>
            <input type="date" id="att-date-filter" onchange="renderAttTable()" style="font-size:12px;padding:6px 10px;border:1px solid var(--border2);border-radius:var(--radius);background:var(--surface2);color:var(--text);">
            <label>Status:</label>
            <select id="att-status-filter" onchange="renderAttTable()"><option value="">All</option></select>
            <label>City:</label>
            <select id="att-city-filter" onchange="renderAttTable()"><option value="">All</option></select>
            <button class="btn" onclick="document.getElementById('att-date-filter').value='';renderAttTable()"><i class="ti ti-x"></i> Clear</button>
          </div>
          <div class="table-wrap"><table><thead id="att-thead"></thead><tbody id="att-tbody"></tbody></table></div>
        </div>
      </div>
    </div>

    <!-- TBT Tab -->
    <div id="tab-tbt" class="tab-content">
      <div class="section-title">TBT Summary</div>
      <div class="metrics" id="tbt-metrics"></div>
      <div class="charts-grid">
        <div class="card">
          <div class="card-title"><i class="ti ti-users-group"></i> TBT by Vendor</div>
          <div id="tbt-vendor-wrap"></div>
        </div>
        <div class="card" style="grid-column:1/-1;">
          <div class="card-title"><i class="ti ti-list-details"></i> TBT Log</div>
          <div class="table-wrap"><table><thead id="tbt-thead"></thead><tbody id="tbt-tbody"></tbody></table></div>
        </div>
      </div>
    </div>

    <!-- DPR Tab -->
    <div id="tab-dpr" class="tab-content">
      <div class="section-title">DPR Summary</div>
      <div class="metrics" id="dpr-metrics"></div>
      <div class="charts-grid">
        <div class="card">
          <div class="card-title"><i class="ti ti-chart-bar"></i> Completion Rate</div>
          <div id="dpr-completion-wrap"></div>
        </div>
        <div class="card" style="grid-column:1/-1;">
          <div class="card-title"><i class="ti ti-list-details"></i> DPR Log</div>
          <div class="table-wrap"><table><thead id="dpr-thead"></thead><tbody id="dpr-tbody"></tbody></table></div>
        </div>
      </div>
    </div>

  </div>

  <div id="no-data" style="text-align:center;padding:80px 20px;">
    <i class="ti ti-database-off" style="font-size:48px;color:var(--muted);display:block;margin-bottom:16px;opacity:0.4;"></i>
    <p style="color:var(--muted);font-size:14px;">Enter your Sheet ID above and click <strong style="color:var(--text);">Load Data</strong> to begin.</p>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
<script>
// ─── State ───────────────────────────────────────────────────────────
let peData=[], attData=[], tbtData=[], dprData=[];
let peHeaders=[], attHeaders=[], tbtHeaders=[], dprHeaders=[];

// Sheet tab names → GID map (will be auto-fetched or user-provided)
const SHEET_TABS = [
  {name:'PE Master',      gid:'0'},
  {name:'Attendance Tracker', gid:'2088990270'},
  {name:'TBT',            gid:''},
  {name:'DPR',            gid:''},
];

// ─── Helpers ─────────────────────────────────────────────────────────
function csvUrl(sheetId, gid) {
  return `https://docs.google.com/spreadsheets/d/${sheetId}/export?format=csv&gid=${gid}`;
}

function col(headers, ...keywords) {
  for (const kw of keywords) {
    const i = headers.findIndex(h => h.toLowerCase().includes(kw.toLowerCase()));
    if (i >= 0) return i;
  }
  return -1;
}

function val(row, idx) { return idx >= 0 ? (row[idx]||'').trim() : ''; }

function showLoading(txt) {
  document.getElementById('load-txt').textContent = txt;
  document.getElementById('loading-overlay').classList.remove('hidden');
}
function hideLoading() { document.getElementById('loading-overlay').classList.add('hidden'); }

function parseCSV(url) {
  return new Promise((resolve, reject) => {
    Papa.parse(url, {
      download: true, header: false, skipEmptyLines: true,
      complete: r => resolve(r.data),
      error: e => reject(e)
    });
  });
}

function metricHTML(icon, label, value, sub, accentColor) {
  return `<div class="metric" style="--accent-color:${accentColor}">
    <div class="icon"><i class="ti ${icon}"></i></div>
    <div class="lbl">${label}</div>
    <div class="val">${value}</div>
    ${sub ? `<div class="sub">${sub}</div>` : ''}
  </div>`;
}

function progressBar(label, count, max, color) {
  const pct = max ? Math.round(count/max*100) : 0;
  return `<div class="progress-row">
    <div class="progress-label" title="${label}">${label}</div>
    <div class="progress-track"><div class="progress-fill" style="width:${pct}%;background:${color}"></div></div>
    <div class="progress-count">${count}</div>
  </div>`;
}

function switchTab(name) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
  document.getElementById('tab-'+name).classList.add('active');
  event.currentTarget.classList.add('active');
}

function fillSelect(selId, values) {
  const sel = document.getElementById(selId);
  if (!sel) return;
  const cur = sel.value;
  while (sel.options.length > 1) sel.remove(1);
  [...new Set(values)].filter(Boolean).sort().forEach(v => {
    const o = document.createElement('option'); o.value = v; o.textContent = v;
    if (v === cur) o.selected = true;
    sel.appendChild(o);
  });
}

function renderSimpleTable(theadId, tbodyId, headers, rows, renderRow) {
  document.getElementById(theadId).innerHTML = '<tr>' + headers.map(h=>`<th>${h}</th>`).join('') + '</tr>';
  const tbody = document.getElementById(tbodyId);
  if (!rows.length) { tbody.innerHTML = `<tr><td colspan="${headers.length}" class="empty"><i class="ti ti-mood-empty"></i>No records found</td></tr>`; return; }
  tbody.innerHTML = rows.map(renderRow).join('');
}

// ─── Load All Sheets ──────────────────────────────────────────────────
async function loadAllSheets() {
  const sheetId = document.getElementById('sheet-id').value.trim();
  if (!sheetId) { alert('Please enter your Google Sheet ID'); return; }

  showLoading('Loading PE Master…');
  try {
    // Try known GIDs first, fallback to 0
    const gids = document.getElementById('sheet-gids').value.trim().split(',').map(s=>s.trim());

    showLoading('Loading PE Master…');
    const pRaw = await parseCSV(csvUrl(sheetId, gids[0]||'0'));
    peHeaders = pRaw[0]||[]; peData = pRaw.slice(1);

    showLoading('Loading Attendance Tracker…');
    const aRaw = await parseCSV(csvUrl(sheetId, gids[1]||'2088990270'));
    attHeaders = aRaw[0]||[]; attData = aRaw.slice(1);

    showLoading('Loading TBT…');
    const tRaw = await parseCSV(csvUrl(sheetId, gids[2]||''));
    tbtHeaders = tRaw[0]||[]; tbtData = tRaw.slice(1);

    showLoading('Loading DPR…');
    const dRaw = await parseCSV(csvUrl(sheetId, gids[3]||''));
    dprHeaders = dRaw[0]||[]; dprData = dRaw.slice(1);

    buildDashboard();
    document.getElementById('status-badge').innerHTML = '<i class="ti ti-circle-check" style="font-size:10px;vertical-align:1px;"></i> Live';
    document.getElementById('status-badge').style.cssText = 'font-size:11px;padding:4px 12px;border-radius:20px;background:rgba(52,201,138,0.15);color:#34c98a;border:1px solid rgba(52,201,138,0.3);';
    document.getElementById('last-updated').textContent = 'Updated ' + new Date().toLocaleTimeString();
  } catch(e) {
    alert('Failed to load sheet. Make sure it is public and the Sheet ID is correct.\n\n'+e);
    console.error(e);
  } finally { hideLoading(); }
}

// ─── Build Dashboard ──────────────────────────────────────────────────
function buildDashboard() {
  document.getElementById('no-data').style.display = 'none';
  document.getElementById('dashboard').style.display = 'block';

  buildPE();
  buildAttendance();
  buildTBT();
  buildDPR();
}

// ─── PE Tab ───────────────────────────────────────────────────────────
function buildPE() {
  const nameI = col(peHeaders,'Employee Name','name');
  const sseI  = col(peHeaders,'SSE ID','sse');
  const mobileI = col(peHeaders,'Mobile','mobile','phone');
  const mailI = col(peHeaders,'Mail','email','mail');
  const deptI = col(peHeaders,'Dept','department');
  const regionI = col(peHeaders,'Region');
  const clusterI = col(peHeaders,'Cluster head');
  const cityMgrI = col(peHeaders,'City Manager');
  const cityMgrMailI = col(peHeaders,'City Manager mail');

  const totalPE = peData.length;

  // Who did check-in (match by email)
  const attEmailI = col(attHeaders,'User Email','email');
  const attCheckinI = col(attHeaders,'Check-in Time','checkin','check-in');
  const attCheckoutI = col(attHeaders,'Check-out Time','checkout','check-out');

  const emailsWithCheckin = new Set(attData.filter(r=>val(r,attCheckinI)).map(r=>val(r,attEmailI).toLowerCase()));
  const emailsWithFull    = new Set(attData.filter(r=>val(r,attCheckinI)&&val(r,attCheckoutI)).map(r=>val(r,attEmailI).toLowerCase()));

  // TBT unique IDs
  const tbtUidI = col(tbtHeaders,'Unique id','unique');
  const tbtUids = new Set(tbtData.map(r=>val(r,tbtUidI)).filter(Boolean));

  // DPR unique IDs
  const dprUidI = col(dprHeaders,'Unique ID','unique');
  const dprUids = new Set(dprData.map(r=>val(r,dprUidI)).filter(Boolean));

  // PE SSE IDs and mail
  const peEmails = peData.map(r=>val(r,mailI).toLowerCase());
  const peSseIds = peData.map(r=>val(r,sseI));

  const checkinCount = peEmails.filter(e=>emailsWithCheckin.has(e)).length;
  const fullCount    = peEmails.filter(e=>emailsWithFull.has(e)).length;

  // TBT/DPR leads — by unique ID matching SSE ID or similar
  const tbtLeads = tbtData.length > 0 ? new Set(tbtData.map(r=>val(r,tbtUidI)).filter(Boolean)).size : 0;
  const dprLeads = dprData.length > 0 ? new Set(dprData.map(r=>val(r,dprUidI)).filter(Boolean)).size : 0;

  document.getElementById('pe-metrics').innerHTML =
    metricHTML('ti-users','Total PEs', totalPE, 'In PE Master', '#4f8ef7') +
    metricHTML('ti-login','Checked In', checkinCount, `${Math.round(checkinCount/totalPE*100)||0}% of total`, '#34c98a') +
    metricHTML('ti-circle-check','Full Attendance', fullCount, 'Check-in + Check-out', '#9b6dff') +
    metricHTML('ti-clipboard-list','TBT Leads', tbtLeads, 'Unique IDs submitted', '#f5a623') +
    metricHTML('ti-file-report','DPR Leads', dprLeads, 'Unique IDs submitted', '#e74c3c');

  // Fill filters
  fillSelect('pe-filter-dept', peData.map(r=>val(r,deptI)));
  fillSelect('pe-filter-region', peData.map(r=>val(r,regionI)));
  fillSelect('pe-filter-city', peData.map(r=>val(r,cityMgrI)));

  renderPETable();
}

function renderPETable() {
  const nameI=col(peHeaders,'Employee Name','name');
  const sseI=col(peHeaders,'SSE ID','sse');
  const mobileI=col(peHeaders,'Mobile','mobile');
  const mailI=col(peHeaders,'Mail','email','mail');
  const deptI=col(peHeaders,'Dept','department');
  const regionI=col(peHeaders,'Region');
  const clusterI=col(peHeaders,'Cluster head');
  const cityMgrI=col(peHeaders,'City Manager');

  const attEmailI=col(attHeaders,'User Email','email');
  const attCheckinI=col(attHeaders,'Check-in Time','checkin');
  const attCheckoutI=col(attHeaders,'Check-out Time','checkout');
  const tbtUidI=col(tbtHeaders,'Unique id','unique');
  const dprUidI=col(dprHeaders,'Unique ID','unique');

  const emailsCheckin=new Set(attData.filter(r=>val(r,attCheckinI)).map(r=>val(r,attEmailI).toLowerCase()));
  const emailsFull=new Set(attData.filter(r=>val(r,attCheckinI)&&val(r,attCheckoutI)).map(r=>val(r,attEmailI).toLowerCase()));

  // Count visits per email
  const visitCount={};
  attData.forEach(r=>{const e=val(r,attEmailI).toLowerCase();visitCount[e]=(visitCount[e]||0)+1;});

  const tbtIds=new Set(tbtData.map(r=>val(r,tbtUidI)).filter(Boolean));
  const dprIds=new Set(dprData.map(r=>val(r,dprUidI)).filter(Boolean));

  const fDept=document.getElementById('pe-filter-dept').value;
  const fRegion=document.getElementById('pe-filter-region').value;
  const fCity=document.getElementById('pe-filter-city').value;

  let rows=peData.filter(r=>{
    if(fDept&&val(r,deptI)!==fDept)return false;
    if(fRegion&&val(r,regionI)!==fRegion)return false;
    if(fCity&&val(r,cityMgrI)!==fCity)return false;
    return true;
  });

  document.getElementById('pe-thead').innerHTML=`<tr><th>#</th><th>Name</th><th>SSE ID</th><th>Dept</th><th>Region</th><th>City Mgr</th><th>Attendance</th><th>Visits</th><th>TBT</th><th>DPR</th></tr>`;

  const tbody=document.getElementById('pe-tbody');
  if(!rows.length){tbody.innerHTML='<tr><td colspan="10" class="empty"><i class="ti ti-mood-empty"></i>No records</td></tr>';return;}

  tbody.innerHTML=rows.map((r,i)=>{
    const email=val(r,mailI).toLowerCase();
    const hasCheckin=emailsCheckin.has(email);
    const hasFull=emailsFull.has(email);
    const attBadge=hasFull?'<span class="badge badge-green">Full</span>':hasCheckin?'<span class="badge badge-blue">Check-in</span>':'<span class="badge badge-red">Absent</span>';
    const visits=visitCount[email]||0;
    const name=val(r,nameI)||'—';
    const initials=name.split(' ').map(w=>w[0]).join('').slice(0,2).toUpperCase();
    return `<tr>
      <td style="color:var(--muted)">${i+1}</td>
      <td><div style="display:flex;align-items:center;gap:8px;"><div class="pe-avatar">${initials}</div><div><div style="font-weight:500">${name}</div><div style="font-size:11px;color:var(--muted)">${val(r,mailI)||''}</div></div></div></td>
      <td>${val(r,sseI)||'—'}</td>
      <td>${val(r,deptI)||'—'}</td>
      <td>${val(r,regionI)||'—'}</td>
      <td>${val(r,cityMgrI)||'—'}</td>
      <td>${attBadge}</td>
      <td><span class="badge badge-blue">${visits}</span></td>
      <td>${tbtIds.has(val(r,sseI))?'<span class="badge badge-green">✓</span>':'<span class="badge badge-red">✗</span>'}</td>
      <td>${dprIds.has(val(r,sseI))?'<span class="badge badge-green">✓</span>':'<span class="badge badge-red">✗</span>'}</td>
    </tr>`;
  }).join('');
}

// ─── Attendance Tab ───────────────────────────────────────────────────
function buildAttendance() {
  const emailI=col(attHeaders,'User Email','email');
  const dateI=col(attHeaders,'Date');
  const projI=col(attHeaders,'Project Name','project');
  const projIdI=col(attHeaders,'Project ID');
  const checkinI=col(attHeaders,'Check-in Time','checkin');
  const checkoutI=col(attHeaders,'Check-out Time','checkout');
  const locI=col(attHeaders,'Check-in Location','location');
  const statusI=col(attHeaders,'Visit Status','status');
  const remarkI=col(attHeaders,'Remark','remark');

  const total=attData.length;
  const withCheckin=attData.filter(r=>val(r,checkinI)).length;
  const withFull=attData.filter(r=>val(r,checkinI)&&val(r,checkoutI)).length;
  const uniquePE=new Set(attData.map(r=>val(r,emailI))).size;
  const uniqueProj=new Set(attData.map(r=>val(r,projI))).size;

  document.getElementById('att-metrics').innerHTML=
    metricHTML('ti-calendar','Total Visits',total,'All records','#4f8ef7')+
    metricHTML('ti-users','Unique PEs',uniquePE,'With attendance','#34c98a')+
    metricHTML('ti-login','Check-ins',withCheckin,`${Math.round(withCheckin/total*100)||0}% of visits`,'#9b6dff')+
    metricHTML('ti-circle-check','Full (In+Out)',withFull,`${Math.round(withFull/total*100)||0}% complete`,'#f5a623')+
    metricHTML('ti-building','Unique Projects',uniqueProj,'Visited','#e74c3c');

  // City-wise chart (derive city from location or project)
  // Use Project Name as grouping since city may not be direct column
  const projCount={};
  attData.forEach(r=>{ const p=val(r,projI)||'Unknown'; projCount[p]=(projCount[p]||0)+1; });
  const topProj=Object.entries(projCount).sort((a,b)=>b[1]-a[1]).slice(0,10);
  const maxP=topProj[0]?.[1]||1;
  document.getElementById('city-chart-wrap').innerHTML=topProj.map(([c,n])=>progressBar(c,n,maxP,'#4f8ef7')).join('')||'<div class="empty">No data</div>';

  // Project-wise
  const projVisits={};
  attData.forEach(r=>{ const p=val(r,projI)||'Unknown'; projVisits[p]=(projVisits[p]||0)+1; });
  const topPV=Object.entries(projVisits).sort((a,b)=>b[1]-a[1]).slice(0,10);
  const maxPV=topPV[0]?.[1]||1;
  document.getElementById('project-chart-wrap').innerHTML=topPV.map(([p,n])=>progressBar(p,n,maxPV,'#34c98a')).join('')||'<div class="empty">No data</div>';

  // Fill filters
  fillSelect('att-status-filter', attData.map(r=>val(r,statusI)));
  fillSelect('att-city-filter', attData.map(r=>val(r,projI)));

  renderAttTable();
}

function renderAttTable() {
  const emailI=col(attHeaders,'User Email','email');
  const dateI=col(attHeaders,'Date');
  const projI=col(attHeaders,'Project Name','project');
  const checkinI=col(attHeaders,'Check-in Time','checkin');
  const checkoutI=col(attHeaders,'Check-out Time','checkout');
  const locI=col(attHeaders,'Check-in Location','location');
  const statusI=col(attHeaders,'Visit Status','status');
  const remarkI=col(attHeaders,'Remark','remark');

  const fDate=document.getElementById('att-date-filter').value;
  const fStatus=document.getElementById('att-status-filter').value;
  const fCity=document.getElementById('att-city-filter').value;

  let rows=attData.filter(r=>{
    if(fStatus&&val(r,statusI)!==fStatus)return false;
    if(fCity&&val(r,projI)!==fCity)return false;
    if(fDate){
      const d=val(r,dateI);
      if(!d.includes(fDate)&&!fDate.includes(d))return false;
    }
    return true;
  });

  document.getElementById('att-thead').innerHTML='<tr><th>#</th><th>Email</th><th>Date</th><th>Project</th><th>Check-in</th><th>Check-out</th><th>Location</th><th>Status</th><th>Remarks</th></tr>';
  const tbody=document.getElementById('att-tbody');
  if(!rows.length){tbody.innerHTML='<tr><td colspan="9" class="empty"><i class="ti ti-mood-empty"></i>No records</td></tr>';return;}

  tbody.innerHTML=rows.slice(0,200).map((r,i)=>{
    const checkin=val(r,checkinI),checkout=val(r,checkoutI);
    const status=val(r,statusI);
    const sBadge=checkin&&checkout?'<span class="badge badge-green">Complete</span>':checkin?'<span class="badge badge-blue">Checked In</span>':'<span class="badge badge-red">Absent</span>';
    return `<tr>
      <td style="color:var(--muted)">${i+1}</td>
      <td style="font-size:12px">${val(r,emailI)||'—'}</td>
      <td>${val(r,dateI)||'—'}</td>
      <td>${val(r,projI)||'—'}</td>
      <td>${checkin||'—'}</td>
      <td>${checkout||'—'}</td>
      <td style="font-size:11px;max-width:150px;overflow:hidden;text-overflow:ellipsis">${val(r,locI)||'—'}</td>
      <td>${status?`<span class="badge badge-blue">${status}</span>`:sBadge}</td>
      <td style="font-size:12px;color:var(--muted)">${val(r,remarkI)||'—'}</td>
    </tr>`;
  }).join('');
  if(rows.length>200) tbody.innerHTML+=`<tr><td colspan="9" style="text-align:center;color:var(--muted);font-size:12px;padding:10px;">Showing 200 of ${rows.length} records</td></tr>`;
}

// ─── TBT Tab ──────────────────────────────────────────────────────────
function buildTBT() {
  const vendorI=col(tbtHeaders,'Vendor','vendor','I&C');
  const topicI=col(tbtHeaders,'Topic','topic');
  const techI=col(tbtHeaders,'Technician','technician');
  const helperI=col(tbtHeaders,'Helper','helper');
  const tsI=col(tbtHeaders,'Time stamp','timestamp','time');
  const uidI=col(tbtHeaders,'Unique id','unique');
  const reportI=col(tbtHeaders,'TBT Report','report');
  const mailI=col(tbtHeaders,'Mail status','mail');
  const planI=col(tbtHeaders,'Tomorrow','tomorrow','plan');

  const total=tbtData.length;
  const uniqueIds=new Set(tbtData.map(r=>val(r,uidI)).filter(Boolean)).size;
  const totalTech=tbtData.reduce((s,r)=>s+(parseInt(val(r,techI))||0),0);
  const totalHelp=tbtData.reduce((s,r)=>s+(parseInt(val(r,helperI))||0),0);
  const withReport=tbtData.filter(r=>val(r,reportI)).length;
  const mailSent=tbtData.filter(r=>(val(r,mailI)||'').toLowerCase().includes('sent')||val(r,mailI)).length;

  document.getElementById('tbt-metrics').innerHTML=
    metricHTML('ti-clipboard-list','Total TBT',total,'Sessions recorded','#4f8ef7')+
    metricHTML('ti-id','Unique IDs',uniqueIds,'Unique PE submissions','#34c98a')+
    metricHTML('ti-tool','Technicians',totalTech,'Total attendees','#9b6dff')+
    metricHTML('ti-users','Helpers',totalHelp,'Total helpers','#f5a623')+
    metricHTML('ti-file-check','Reports',withReport,`of ${total} sessions`,'#e74c3c');

  // Vendor chart
  const vendorCount={};
  tbtData.forEach(r=>{ const v=val(r,vendorI)||'Unknown'; vendorCount[v]=(vendorCount[v]||0)+1; });
  const topV=Object.entries(vendorCount).sort((a,b)=>b[1]-a[1]).slice(0,10);
  const maxV=topV[0]?.[1]||1;
  document.getElementById('tbt-vendor-wrap').innerHTML=topV.map(([v,n])=>progressBar(v,n,maxV,'#9b6dff')).join('')||'<div class="empty">No data</div>';

  // Table
  const cols=['#','Unique ID','Vendor','Topic','Technicians','Helpers','Timestamp','Report','Mail'];
  document.getElementById('tbt-thead').innerHTML='<tr>'+cols.map(c=>`<th>${c}</th>`).join('')+'</tr>';
  const tbody=document.getElementById('tbt-tbody');
  if(!tbtData.length){tbody.innerHTML=`<tr><td colspan="${cols.length}" class="empty"><i class="ti ti-mood-empty"></i>No TBT records</td></tr>`;return;}
  tbody.innerHTML=tbtData.slice(0,200).map((r,i)=>{
    const report=val(r,reportI);
    const mail=val(r,mailI);
    return `<tr>
      <td style="color:var(--muted)">${i+1}</td>
      <td><span class="badge badge-purple">${val(r,uidI)||'—'}</span></td>
      <td>${val(r,vendorI)||'—'}</td>
      <td style="max-width:180px;overflow:hidden;text-overflow:ellipsis">${val(r,topicI)||'—'}</td>
      <td>${val(r,techI)||'0'}</td>
      <td>${val(r,helperI)||'0'}</td>
      <td style="font-size:11px">${val(r,tsI)||'—'}</td>
      <td>${report?'<span class="badge badge-green">✓ Yes</span>':'<span class="badge badge-red">✗ No</span>'}</td>
      <td>${mail?`<span class="badge badge-blue">${mail}</span>`:'<span class="badge badge-red">Pending</span>'}</td>
    </tr>`;
  }).join('');
}

// ─── DPR Tab ──────────────────────────────────────────────────────────
function buildDPR() {
  const cidI=col(dprHeaders,'Child ID','child');
  const planI=col(dprHeaders,'Today\'s Work Plan','plan','work');
  const mpI=col(dprHeaders,'Manpower','manpower');
  const taskI=col(dprHeaders,'Completed Task','completed','task');
  const pctI=col(dprHeaders,'% of completion','completion','%','percent');
  const issueI=col(dprHeaders,'Issues','issue','challenges');
  const remarkI=col(dprHeaders,'Remarks','remark');
  const tsI=col(dprHeaders,'Timestamp','timestamp','time');
  const uidI=col(dprHeaders,'Unique ID','unique');

  const total=dprData.length;
  const uniqueIds=new Set(dprData.map(r=>val(r,uidI)).filter(Boolean)).size;
  const totalMP=dprData.reduce((s,r)=>s+(parseInt(val(r,mpI))||0),0);
  const withIssues=dprData.filter(r=>val(r,issueI)&&val(r,issueI).toLowerCase()!=='no'&&val(r,issueI).toLowerCase()!=='none').length;

  const pcts=dprData.map(r=>parseFloat(val(r,pctI))||0).filter(n=>n>0);
  const avgPct=pcts.length?Math.round(pcts.reduce((a,b)=>a+b,0)/pcts.length):0;

  document.getElementById('dpr-metrics').innerHTML=
    metricHTML('ti-file-report','Total DPR',total,'Reports submitted','#4f8ef7')+
    metricHTML('ti-id','Unique IDs',uniqueIds,'Unique PE submissions','#34c98a')+
    metricHTML('ti-users','Total Manpower',totalMP,'Across all DPRs','#9b6dff')+
    metricHTML('ti-percentage','Avg Completion',avgPct+'%','Average across DPRs','#f5a623')+
    metricHTML('ti-alert-triangle','With Issues',withIssues,`of ${total} reports`,'#e74c3c');

  // Completion distribution
  const buckets={'0-25%':0,'26-50%':0,'51-75%':0,'76-100%':0};
  dprData.forEach(r=>{
    const p=parseFloat(val(r,pctI))||0;
    if(p<=25)buckets['0-25%']++;
    else if(p<=50)buckets['26-50%']++;
    else if(p<=75)buckets['51-75%']++;
    else buckets['76-100%']++;
  });
  const maxB=Math.max(...Object.values(buckets));
  const bColors={'0-25%':'#e74c3c','26-50%':'#f5a623','51-75%':'#4f8ef7','76-100%':'#34c98a'};
  document.getElementById('dpr-completion-wrap').innerHTML=Object.entries(buckets).map(([k,v])=>progressBar(k,v,maxB,bColors[k])).join('');

  // Table
  const cols=['#','Unique ID','Child ID','Work Plan','Manpower','Completion','Issues','Remarks','Timestamp'];
  document.getElementById('dpr-thead').innerHTML='<tr>'+cols.map(c=>`<th>${c}</th>`).join('')+'</tr>';
  const tbody=document.getElementById('dpr-tbody');
  if(!dprData.length){tbody.innerHTML=`<tr><td colspan="${cols.length}" class="empty"><i class="ti ti-mood-empty"></i>No DPR records</td></tr>`;return;}
  tbody.innerHTML=dprData.slice(0,200).map((r,i)=>{
    const pct=parseFloat(val(r,pctI))||0;
    const pctColor=pct>=75?'badge-green':pct>=50?'badge-blue':pct>=25?'badge-orange':'badge-red';
    const issue=val(r,issueI);
    const hasIssue=issue&&issue.toLowerCase()!=='no'&&issue.toLowerCase()!=='none';
    return `<tr>
      <td style="color:var(--muted)">${i+1}</td>
      <td><span class="badge badge-purple">${val(r,uidI)||'—'}</span></td>
      <td style="font-size:11px">${val(r,cidI)||'—'}</td>
      <td style="max-width:160px;overflow:hidden;text-overflow:ellipsis;font-size:12px">${val(r,planI)||'—'}</td>
      <td>${val(r,mpI)||'—'}</td>
      <td><span class="badge ${pctColor}">${pct}%</span></td>
      <td>${hasIssue?`<span class="badge badge-red">⚠ Yes</span>`:'<span class="badge badge-green">None</span>'}</td>
      <td style="font-size:11px;color:var(--muted);max-width:130px;overflow:hidden;text-overflow:ellipsis">${val(r,remarkI)||'—'}</td>
      <td style="font-size:11px">${val(r,tsI)||'—'}</td>
    </tr>`;
  }).join('');
}

// Auto-load on page open
window.addEventListener('DOMContentLoaded', () => {
  const saved = localStorage.getItem('ops_sheet_id');
  if (saved) document.getElementById('sheet-id').value = saved;
});
</script>
</body>
</html>
