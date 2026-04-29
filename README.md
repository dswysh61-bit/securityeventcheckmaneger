<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>דשבורד ניהול אבטחה</title>
<link href="https://fonts.googleapis.com/css2?family=Heebo:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{--navy:#0f2744;--navy2:#1a3a5c;--blue:#2563eb;--blue-light:#dbeafe;--green:#16a34a;--green-bg:#dcfce7;--amber:#d97706;--amber-bg:#fef3c7;--red:#dc2626;--red-bg:#fee2e2;--border:#e2e8f0;--bg:#f1f5f9;--text:#1e293b;--muted:#64748b;--subtle:#94a3b8}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Heebo',sans-serif;background:var(--bg);color:var(--text);direction:rtl;min-height:100vh}
.sidebar{position:fixed;right:0;top:0;bottom:0;width:220px;background:var(--navy);color:white;z-index:20;display:flex;flex-direction:column}
.sb-logo{padding:1.25rem;border-bottom:1px solid rgba(255,255,255,.1)}
.sb-logo h2{font-size:14px;font-weight:600;line-height:1.4}
.sb-logo p{font-size:11px;opacity:.6;margin-top:2px}
.sb-logo .fb-dot{display:inline-block;width:8px;height:8px;border-radius:50%;background:#ef4444;margin-left:6px;transition:background .3s}
.sb-logo .fb-dot.connected{background:#22c55e}
.nav-item{display:flex;align-items:center;gap:10px;padding:10px 1.25rem;font-size:13px;cursor:pointer;color:rgba(255,255,255,.7);transition:all .15s;border-right:3px solid transparent}
.nav-item:hover{background:rgba(255,255,255,.07);color:white}
.nav-item.active{background:rgba(255,255,255,.1);color:white;border-right-color:#60a5fa}
.nav-icon{font-size:15px;width:20px}
.main{margin-right:220px;min-height:100vh}
.topbar{background:white;border-bottom:1px solid var(--border);padding:12px 1.5rem;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:10}
.topbar h1{font-size:16px;font-weight:600;color:var(--navy2)}
.btn{padding:7px 14px;border-radius:7px;font-size:12px;font-weight:600;cursor:pointer;font-family:'Heebo',sans-serif;border:1px solid var(--border);background:white;color:var(--muted);transition:all .15s}
.btn:hover{background:var(--bg)}
.btn-primary{background:var(--navy2);color:white;border-color:var(--navy2)}
.btn-primary:hover{background:var(--navy)}
.btn-danger{background:#fee2e2;color:#991b1b;border-color:#fca5a5}
.content{padding:1.5rem}
.stats-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:1.5rem}
@media(max-width:900px){.stats-grid{grid-template-columns:repeat(2,1fr)}}
.stat-card{background:white;border:1px solid var(--border);border-radius:10px;padding:1rem 1.25rem}
.stat-card .lbl{font-size:11px;color:var(--muted);margin-bottom:4px;font-weight:500}
.stat-card .val{font-size:26px;font-weight:700;color:var(--navy2)}
.stat-card .sub{font-size:11px;color:var(--subtle);margin-top:2px}
.panel{background:white;border:1px solid var(--border);border-radius:10px;margin-bottom:1.5rem}
.panel-head{display:flex;justify-content:space-between;align-items:center;padding:1rem 1.25rem;border-bottom:1px solid var(--border)}
.panel-head h3{font-size:14px;font-weight:600;color:var(--navy2)}
.filter-row{display:flex;gap:8px;padding:10px 1.25rem;border-bottom:1px solid var(--border);flex-wrap:wrap;align-items:center}
.filter-row input,.filter-row select{border:1px solid var(--border);border-radius:7px;padding:6px 10px;font-size:12px;font-family:'Heebo',sans-serif;direction:rtl}
.filter-row input{flex:1;min-width:160px}
.tbl-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:12px}
th{text-align:right;padding:9px 12px;background:#f8fafc;color:var(--muted);font-weight:600;font-size:11px;border-bottom:1px solid var(--border);white-space:nowrap}
td{padding:9px 12px;border-bottom:.5px solid var(--border);color:var(--text);vertical-align:middle}
tr:hover td{background:#f8fafc}
tr:last-child td{border-bottom:none}
.badge{display:inline-block;padding:3px 10px;border-radius:12px;font-size:11px;font-weight:600;white-space:nowrap}
.badge-low{background:var(--green-bg);color:#15803d}
.badge-mid{background:var(--amber-bg);color:#92400e}
.badge-high{background:var(--red-bg);color:#991b1b}
.score-num{font-size:18px;font-weight:700}
.score-low{color:#16a34a}.score-mid{color:#d97706}.score-high{color:#dc2626}
.act-btn{padding:4px 10px;border-radius:6px;font-size:11px;font-weight:600;cursor:pointer;font-family:'Heebo',sans-serif;border:1px solid var(--border);background:white;transition:all .1s;margin-right:3px}
.act-btn:hover{background:var(--bg)}
.status-badge{padding:3px 9px;border-radius:12px;font-size:10px;font-weight:600;white-space:nowrap}
.s-new{background:#dbeafe;color:#1e40af}
.s-reviewed{background:#dcfce7;color:#15803d}
.s-pending{background:#fef3c7;color:#92400e}
.s-approved{background:#dcfce7;color:#15803d}
.s-rejected{background:#fee2e2;color:#991b1b}
.modal-bg{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);z-index:100;align-items:center;justify-content:center;padding:1rem}
.modal-bg.show{display:flex}
.modal{background:white;border-radius:14px;max-width:600px;width:100%;max-height:92vh;overflow-y:auto;direction:rtl}
.modal-head{padding:1.25rem 1.5rem;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;background:white;z-index:2}
.modal-head h3{font-size:16px;font-weight:600;color:var(--navy2)}
.close-btn{background:none;border:none;font-size:20px;cursor:pointer;color:var(--muted)}
.modal-body{padding:1.5rem}
.dt{display:flex;justify-content:space-between;padding:5px 0;font-size:13px;border-bottom:.5px solid #f1f5f9}
.dt .dl{color:var(--muted)}
.dt .dv{font-weight:500}
.ds-title{font-size:11px;font-weight:700;color:var(--navy2);text-transform:uppercase;letter-spacing:.06em;border-bottom:1px solid var(--border);padding-bottom:6px;margin:1rem 0 8px}
.score-big{background:var(--bg);border-radius:10px;padding:1rem;text-align:center;margin-bottom:1.25rem}
.gauge{height:8px;background:#e2e8f0;border-radius:20px;overflow:hidden;margin:8px 0 4px}
.gauge-fill{height:100%;border-radius:20px}
.manual-section{background:#f8fafc;border:1px solid var(--border);border-radius:10px;padding:1rem;margin-top:1rem}
.manual-section label{font-size:12px;font-weight:600;color:var(--muted);display:block;margin-bottom:5px}
.range-row{display:flex;align-items:center;gap:10px;margin-bottom:10px}
.range-row input[type=range]{flex:1;accent-color:var(--navy2)}
.range-row input[type=number]{width:55px;border:1px solid var(--border);border-radius:6px;padding:5px 8px;font-size:14px;font-weight:700;font-family:'Heebo',sans-serif;text-align:center}
.sel-status{width:100%;border:1px solid var(--border);border-radius:7px;padding:7px 10px;font-size:13px;font-family:'Heebo',sans-serif;margin-bottom:10px}
.notes-area{width:100%;border:1px solid var(--border);border-radius:8px;padding:8px 12px;font-size:13px;font-family:'Heebo',sans-serif;resize:vertical;min-height:70px;direction:rtl}
.modal-foot{padding:1rem 1.5rem;border-top:1px solid var(--border);display:flex;gap:8px;justify-content:flex-end;position:sticky;bottom:0;background:white}
.empty{text-align:center;padding:3rem;color:var(--muted)}
.empty .icon{font-size:40px;margin-bottom:12px}
.fb-banner{background:#fef3c7;border:1px solid #fde68a;border-radius:10px;padding:12px 16px;margin-bottom:1.5rem;font-size:12px;color:#92400e;line-height:1.6}
.fb-banner code{background:#fff;padding:2px 6px;border-radius:4px}
.live-dot{width:8px;height:8px;border-radius:50%;background:#22c55e;display:inline-block;margin-left:6px;animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}
.mini-bar{margin-bottom:8px;display:flex;align-items:center;gap:8px;font-size:12px}
.mini-bar .ml{min-width:60px;text-align:right;color:var(--muted)}
.mini-bar .mt{flex:1;height:8px;background:#e2e8f0;border-radius:20px;overflow:hidden}
.mini-bar .mf{height:100%;border-radius:20px}
.mini-bar .mv{min-width:24px;font-weight:600}
.chart-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:1.5rem}
@media(max-width:700px){.chart-grid{grid-template-columns:1fr}}
</style>
</head>
<body>

<!-- ============================================================
  🔥 Firebase — שנה לכתובת הפרויקט שלך (אותה כתובת כמו בטופס!)
  ============================================================ -->
<script>
  var FIREBASE_URL = 'https://security-event-check-default-rtdb.europe-west1.firebasedatabase.app';
  var POLLING_INTERVAL = 15000; // רענון אוטומטי כל 15 שניות
</script>

<div class="sidebar">
  <div class="sb-logo">
    <h2><span class="fb-dot" id="fbDot"></span>מרכז ניהול אבטחה</h2>
    <p id="fbStatus">מתחבר ל-Firebase...</p>
  </div>
  <div class="nav-item active" onclick="showView('dashboard')"><span class="nav-icon">📊</span> דשבורד</div>
  <div class="nav-item" onclick="showView('all')"><span class="nav-icon">📋</span> כל ההגשות</div>
  <div class="nav-item" onclick="showView('high')"><span class="nav-icon">🔴</span> סיכון גבוה</div>
  <div class="nav-item" onclick="showView('pending')"><span class="nav-icon">⏳</span> ממתינות לבדיקה</div>
  <div style="margin-top:auto;padding:1rem 1.25rem;border-top:1px solid rgba(255,255,255,.1)">
    <button class="btn" style="width:100%;margin-bottom:8px;font-size:11px" onclick="loadFromFirebase()">🔄 רענן נתונים</button>
    <div style="font-size:10px;opacity:.4" id="lastSync">לא סונכרן עדיין</div>
  </div>
</div>

<div class="main">
  <div class="topbar">
    <h1 id="pageTitle">דשבורד ראשי</h1>
    <div style="display:flex;gap:8px">
      <button class="btn" onclick="exportCSV()">ייצא CSV</button>
      <button class="btn btn-danger" onclick="confirmDeleteAll()">🗑️ מחק הכל</button>
      <button class="btn btn-primary" onclick="loadFromFirebase()">🔄 רענן</button>
    </div>
  </div>
  <div class="content" id="mainContent">
    <div class="empty"><div class="icon">🔥</div><h3>טוען נתונים מ-Firebase...</h3><p>אנא המתן</p></div>
  </div>
</div>

<!-- MODAL -->
<div class="modal-bg" id="modalBg" onclick="closeModal(event)">
  <div class="modal">
    <div class="modal-head"><h3 id="modalTitle">פרטי הגשה</h3><button class="close-btn" onclick="closeModalDirect()">✕</button></div>
    <div class="modal-body" id="modalBody"></div>
    <div class="modal-foot" id="modalFoot"></div>
  </div>
</div>

<script>
/* =========================================================
   🔥 FIREBASE REST API — קריאה וכתיבה
   ========================================================= */

var allSubmissions = [];
var currentView = 'dashboard';
var pollingTimer = null;

// בדיקת חיבור וטעינה ראשונה
window.addEventListener('DOMContentLoaded', function() {
  if (!FIREBASE_URL || FIREBASE_URL.includes('YOUR-PROJECT')) {
    document.getElementById('fbDot').className = 'fb-dot';
    document.getElementById('fbStatus').textContent = 'Firebase לא מוגדר';
    loadFromLocalStorage();
    return;
  }
  loadFromFirebase();
  // רענון אוטומטי בזמן אמת
  pollingTimer = setInterval(function() {
    loadFromFirebase(true); // silent reload
  }, POLLING_INTERVAL);
});

// טעינה מ-Firebase
function loadFromFirebase(silent) {
  if (!FIREBASE_URL || FIREBASE_URL.includes('YOUR-PROJECT')) {
    loadFromLocalStorage();
    return;
  }
  if (!silent) {
    document.getElementById('fbStatus').textContent = 'טוען...';
  }
  fetch(FIREBASE_URL + '/submissions.json')
    .then(function(res) {
      if (!res.ok) throw new Error('HTTP ' + res.status);
      return res.json();
    })
    .then(function(data) {
      // המר אובייקט Firebase למערך
      allSubmissions = [];
      if (data) {
        Object.keys(data).forEach(function(key) {
          var item = data[key];
          item._fbKey = key;
          allSubmissions.push(item);
        });
        // מיין לפי תאריך הגשה (חדש ראשון)
        allSubmissions.sort(function(a,b){ return (b.timestamp||'').localeCompare(a.timestamp||''); });
      }
      document.getElementById('fbDot').className = 'fb-dot connected';
      document.getElementById('fbStatus').textContent = allSubmissions.length + ' הגשות';
      document.getElementById('lastSync').textContent = 'עודכן: ' + new Date().toLocaleTimeString('he-IL');
      showView(currentView);
    })
    .catch(function(err) {
      document.getElementById('fbDot').className = 'fb-dot';
      document.getElementById('fbStatus').textContent = 'שגיאת חיבור';
      console.error('Firebase load error:', err);
      if (!silent) {
        loadFromLocalStorage();
        document.getElementById('mainContent').insertAdjacentHTML('afterbegin',
          '<div class="fb-banner">⚠️ לא ניתן להתחבר ל-Firebase (<code>'+err.message+'</code>). מציג נתונים מהדפדפן.</div>');
      }
    });
}

// שמירת עדכון ב-Firebase (שינוי ציון / סטטוס / הערות)
function updateInFirebase(ref, updates) {
  if (!FIREBASE_URL || FIREBASE_URL.includes('YOUR-PROJECT')) {
    updateInLocalStorage(ref, updates);
    return Promise.resolve();
  }
  return fetch(FIREBASE_URL + '/submissions/' + ref + '.json', {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(updates)
  }).then(function(res) {
    if (!res.ok) throw new Error('HTTP ' + res.status);
    return res.json();
  });
}

// גיבוי localStorage
function loadFromLocalStorage() {
  var raw = JSON.parse(localStorage.getItem('securitySubmissions') || '[]');
  allSubmissions = raw.slice().reverse();
  showView(currentView);
}
function updateInLocalStorage(ref, updates) {
  var raw = JSON.parse(localStorage.getItem('securitySubmissions') || '[]');
  var idx = raw.findIndex(function(s){ return s.ref === ref; });
  if (idx >= 0) Object.assign(raw[idx], updates);
  localStorage.setItem('securitySubmissions', JSON.stringify(raw));
}

/* =========================================================
   תצוגות
   ========================================================= */
function levelInfo(sc) {
  if (sc <= 3) return { label:'נמוכה', cls:'badge-low', color:'#16a34a', fill:'#16a34a', scls:'score-low' };
  if (sc <= 7) return { label:'בינונית', cls:'badge-mid', color:'#d97706', fill:'#d97706', scls:'score-mid' };
  return { label:'גבוהה', cls:'badge-high', color:'#dc2626', fill:'#dc2626', scls:'score-high' };
}

function showView(v) {
  currentView = v;
  document.querySelectorAll('.nav-item').forEach(function(n,i){ n.classList.remove('active'); });
  var idx={dashboard:0,all:1,high:2,pending:3}[v]||0;
  document.querySelectorAll('.nav-item')[idx].classList.add('active');
  var titles={dashboard:'דשבורד ראשי',all:'כל ההגשות',high:'אירועים בסיכון גבוה',pending:'ממתינות לבדיקה'};
  document.getElementById('pageTitle').textContent = titles[v]||v;
  if (v==='dashboard') renderDashboard();
  else if (v==='all') renderTable(allSubmissions, 'כל ההגשות');
  else if (v==='high') renderTable(allSubmissions.filter(function(s){ return (s.manualScore||s.score||0)>=8; }), 'סיכון גבוה');
  else if (v==='pending') renderTable(allSubmissions.filter(function(s){ return !s.status||s.status==='new'; }), 'ממתינות לבדיקה');
}

function renderDashboard() {
  var subs = allSubmissions;
  var total = subs.length;
  var low = subs.filter(function(s){ return (s.manualScore||s.score||0)<=3; }).length;
  var mid = subs.filter(function(s){ var sc=s.manualScore||s.score||0; return sc>=4&&sc<=7; }).length;
  var high = subs.filter(function(s){ return (s.manualScore||s.score||0)>=8; }).length;
  var pending = subs.filter(function(s){ return !s.status||s.status==='new'; }).length;
  var avg = total ? Math.round(subs.reduce(function(a,b){ return a+(b.manualScore||b.score||0); },0)/total) : 0;

  var html = '';

  // Firebase config banner
  if (!FIREBASE_URL || FIREBASE_URL.includes('YOUR-PROJECT')) {
    html += '<div class="fb-banner">🔥 <strong>Firebase לא מוגדר.</strong> כדי לחבר בין הטופס לדשבורד:<br>';
    html += '1. צור פרויקט ב- <strong>console.firebase.google.com</strong><br>';
    html += '2. לך ל-Realtime Database ← Create<br>';
    html += '3. בחכללי כתיב: <code>{ "rules": { ".read": false, ".write": true } }</code><br>';
    html += '4. העתק את ה-URL לשני הקבצים: <code>var FIREBASE_URL = \'https://...\'</code></div>';
  }

  html += '<div class="stats-grid">';
  html += stat('סה"כ הגשות', total, 'כולל כל הסטטוסים');
  html += stat('ממתינות', pending, 'טרם טופלו', '#d97706');
  html += stat('סיכון גבוה', high, 'ציון 8 ומעלה', '#dc2626');
  html += stat('ציון ממוצע', avg, 'מתוך 10');
  html += '</div>';

  html += '<div class="chart-grid">';
  html += '<div class="panel"><div class="panel-head"><h3>התפלגות סיכון</h3></div><div style="padding:1rem 1.25rem">';
  var mx = Math.max(low,mid,high,1);
  html += miniBar('נמוכה', low, mx, '#16a34a');
  html += miniBar('בינונית', mid, mx, '#d97706');
  html += miniBar('גבוהה', high, mx, '#dc2626');
  html += '</div></div>';

  html += '<div class="panel"><div class="panel-head"><h3>הגשות אחרונות <span class="live-dot"></span></h3></div>';
  var recent = subs.slice(0,5);
  if (!recent.length) { html += '<div style="padding:1rem;font-size:12px;color:var(--muted)">אין הגשות עדיין</div>'; }
  else {
    html += '<table><thead><tr><th>אירוע</th><th>ציון</th><th>סטטוס</th></tr></thead><tbody>';
    recent.forEach(function(s) {
      var sc = s.manualScore||s.score||0;
      var li = levelInfo(sc);
      var st = s.status||'new';
      html += '<tr><td style="font-weight:500">'+(s.evname||'—')+'</td>';
      html += '<td><span class="'+li.scls+'" style="font-weight:700">'+sc+'</span></td>';
      html += '<td><span class="status-badge s-'+st+'">'+statusLabel(st)+'</span></td></tr>';
    });
    html += '</tbody></table>';
  }
  html += '</div></div>';

  if (subs.length) {
    html += '<div class="panel"><div class="panel-head"><h3>רשימת הגשות</h3></div>';
    html += buildTableHTML(subs.slice(0,15));
    html += '</div>';
  }

  document.getElementById('mainContent').innerHTML = html;
}

function stat(label, val, sub, color) {
  return '<div class="stat-card"><div class="lbl">'+label+'</div><div class="val"'+(color?' style="color:'+color+'"':'')+'>'+val+'</div><div class="sub">'+sub+'</div></div>';
}
function miniBar(label, count, max, color) {
  return '<div class="mini-bar"><span class="ml">'+label+'</span><div class="mt"><div class="mf" style="width:'+Math.round(count/max*100)+'%;background:'+color+'"></div></div><span class="mv">'+count+'</span></div>';
}
function statusLabel(st) { return {new:'חדש',reviewed:'נבדק',approved:'אושר',pending:'ממתין',rejected:'נדחה'}[st]||st; }

function renderTable(list, title) {
  var html = '<div class="panel">';
  html += '<div class="panel-head"><h3>'+title+' ('+list.length+')</h3></div>';
  html += '<div class="filter-row"><input type="text" placeholder="חיפוש לפי שם אירוע, ארגון, אסמכתא..." oninput="filterTable(this.value,\''+title+'\')" id="searchInput">';
  html += '<select onchange="filterByLevel(this.value,\''+title+'\')" id="levelFilter"><option value="">כל הרמות</option><option value="low">נמוכה</option><option value="mid">בינונית</option><option value="high">גבוהה</option></select>';
  html += '<select onchange="filterByStatus(this.value,\''+title+'\')" id="statusFilter"><option value="">כל הסטטוסים</option><option value="new">חדש</option><option value="reviewed">נבדק</option><option value="approved">אושר</option><option value="pending">ממתין</option><option value="rejected">נדחה</option></select>';
  html += '</div>';
  html += buildTableHTML(list);
  html += '</div>';
  document.getElementById('mainContent').innerHTML = html;
}

function buildTableHTML(list) {
  if (!list.length) return '<div class="empty"><div class="icon">📭</div><h3>אין הגשות</h3><p>טפסים שיוגשו יופיעו כאן</p></div>';
  var html = '<div class="tbl-wrap"><table><thead><tr><th>אסמכתא</th><th>ארגון</th><th>שם האירוע</th><th>תאריך</th><th>ציון</th><th>רמה</th><th>סטטוס</th><th>פעולות</th></tr></thead><tbody>';
  list.forEach(function(s) {
    var sc = s.manualScore||s.score||0;
    var li = levelInfo(sc);
    var st = s.status||'new';
    var ts = s.timestamp ? new Date(s.timestamp).toLocaleDateString('he-IL') : '—';
    html += '<tr>';
    html += '<td style="font-family:monospace;font-size:10px;color:var(--muted)">'+(s.ref||'—')+'</td>';
    html += '<td>'+(s.org||'—')+'</td>';
    html += '<td style="font-weight:500">'+(s.evname||'—')+'</td>';
    html += '<td style="color:var(--muted)">'+(s.evstart||ts)+'</td>';
    html += '<td><span class="score-num '+li.scls+'">'+sc+'</span></td>';
    html += '<td><span class="badge '+li.cls+'">'+li.label+'</span></td>';
    html += '<td><span class="status-badge s-'+st+'">'+statusLabel(st)+'</span></td>';
    html += '<td style="white-space:nowrap">';
    html += '<button class="act-btn" onclick="openDetail(\''+s.ref+'\')">📋 צפה / דרג</button>';
    html += ' <button class="act-btn" style="color:#dc2626;border-color:#fca5a5" onclick="confirmDelete(\''+s.ref+'\',\''+encodeURIComponent(s.evname||'')+'\')">🗑️ מחק</button>';
    html += '</td>';
    html += '</tr>';
  });
  html += '</tbody></table></div>';
  return html;
}

var filteredCache = [];
function filterTable(q, title) {
  var base = getBaseList();
  var filtered = base.filter(function(s){ return JSON.stringify(s).includes(q); });
  var tw = document.querySelector('.tbl-wrap');
  if (tw) tw.outerHTML = buildTableHTML(filtered);
}
function filterByLevel(level, title) {
  var base = getBaseList();
  var filtered = level==='low'?base.filter(function(s){return(s.manualScore||s.score||0)<=3;}):
                 level==='mid'?base.filter(function(s){var sc=s.manualScore||s.score||0;return sc>=4&&sc<=7;}):
                 level==='high'?base.filter(function(s){return(s.manualScore||s.score||0)>=8;}):base;
  var tw = document.querySelector('.tbl-wrap');
  if (tw) tw.outerHTML = buildTableHTML(filtered);
}
function filterByStatus(status, title) {
  var base = getBaseList();
  var filtered = status?base.filter(function(s){return(s.status||'new')===status;}):base;
  var tw = document.querySelector('.tbl-wrap');
  if (tw) tw.outerHTML = buildTableHTML(filtered);
}
function getBaseList() {
  if (currentView==='high') return allSubmissions.filter(function(s){return(s.manualScore||s.score||0)>=8;});
  if (currentView==='pending') return allSubmissions.filter(function(s){return!s.status||s.status==='new';});
  return allSubmissions;
}

/* =========================================================
   מודל פרטים + עריכה
   ========================================================= */
function openDetail(ref) {
  var s = allSubmissions.find(function(x){ return x.ref===ref; });
  if (!s) return;
  var sc = s.manualScore||s.score||0;
  var li = levelInfo(sc);
  var pct = Math.round(sc/10*100);

  var html = '<div class="score-big">';
  html += '<div style="font-size:44px;font-weight:700;color:'+li.color+'">'+sc+'</div>';
  html += '<div style="font-size:11px;color:var(--muted)">מתוך 10</div>';
  html += '<div class="gauge"><div class="gauge-fill" style="width:'+pct+'%;background:'+li.fill+'"></div></div>';
  html += '<span class="badge '+li.cls+'" style="font-size:13px">רמת אבטחה: '+li.label+'</span>';
  html += '</div>';

  function dt(l,v){
    var d=v||'—';
    if(v==='כן')d='<span style="background:#dbeafe;color:#1e40af;padding:2px 8px;border-radius:10px;font-size:11px;font-weight:600">כן</span>';
    else if(v==='לא')d='<span style="background:#f1f5f9;color:#64748b;padding:2px 8px;border-radius:10px;font-size:11px">לא</span>';
    return '<div class="dt"><span class="dl">'+l+'</span><span class="dv">'+d+'</span></div>';
  }

  html += '<div class="ds-title">פרטי ארגון ואיש קשר</div>';
  html += dt('ארגון',s.org); html += dt('איש קשר',(s.fname||'')+' '+(s.lname||''));
  html += dt('אימייל',s.email); html += dt('טלפון',s.phone);

  html += '<div class="ds-title">פרטי האירוע</div>';
  html += dt('שם האירוע',s.evname);
  html += dt('תאריכים',(s.evstart||'')+(s.evend&&s.evend!==s.evstart?' — '+s.evend:''));
  html += dt('מיקום',s.venue); html += dt('קטגוריה',s.evcat);
  html += dt('קהל',{small:'עד 100',med:'100–500',large:'500–1000',xlarge:'מעל 1000'}[s.guestnum]||s.guestnum||'—');
  html += dt('כניסה',{invite:'בהזמנה',community:'קהילתי',ticketed:'כרטיסים',open:'פתוח'}[s.entrytype]||s.entrytype||'—');

  html += '<div class="ds-title">גורמי סיכון</div>';
  html += dt('שעות לילה',s.latenight); html += dt('קרבה לממשלה',s.govprox);
  html += dt('אלכוהול',s.alcohol); html += dt('כניסה פתוחה',s.entrytype==='open'?'כן':'לא');
  html += dt('אישים בכירים',s.vip); if(s.vip_detail) html += dt('פירוט VIP',s.vip_detail);
  html += dt('הפגנות',s.protest); html += dt('היסטוריה',s.history); html += dt('ספורט מגע',s.contact);

  if (s.extra) { html += '<div class="ds-title">מידע נוסף</div><div style="font-size:13px;line-height:1.6">'+s.extra+'</div>'; }

  html += '<div class="manual-section">';
  html += '<div class="ds-title" style="margin-top:0">✏️ דירוג ידני</div>';
  html += '<label>שנה ציון (0-10)</label>';
  html += '<div class="range-row"><input type="range" min="0" max="10" value="'+sc+'" id="mRange" oninput="document.getElementById(\'mNum\').value=this.value"><input type="number" min="0" max="10" value="'+sc+'" id="mNum" oninput="document.getElementById(\'mRange\').value=this.value"></div>';
  html += '<label>סטטוס</label>';
  html += '<select class="sel-status" id="mStatus"><option value="new"'+(s.status==='new'?' selected':'')+'>חדש</option><option value="reviewed"'+(s.status==='reviewed'?' selected':'')+'>נבדק</option><option value="approved"'+(s.status==='approved'?' selected':'')+'>אושר</option><option value="pending"'+(s.status==='pending'?' selected':'')+'>ממתין למידע</option><option value="rejected"'+(s.status==='rejected'?' selected':'')+'>נדחה</option></select>';
  html += '<label>הערות פנימיות</label>';
  html += '<textarea class="notes-area" id="mNotes" placeholder="הוסף הערות פנימיות...">'+(s.notes||'')+'</textarea>';
  html += '</div>';

  document.getElementById('modalTitle').textContent = s.evname||'פרטי הגשה';
  document.getElementById('modalBody').innerHTML = html;
  document.getElementById('modalFoot').innerHTML =
    '<button class="btn" onclick="closeModalDirect()">סגור</button>' +
    '<button class="btn btn-primary" onclick="saveDetail(\''+ref+'\')">💾 שמור ב-Firebase</button>';
  document.getElementById('modalBg').classList.add('show');
}

function saveDetail(ref) {
  var updates = {
    manualScore: parseInt(document.getElementById('mNum').value)||0,
    status: document.getElementById('mStatus').value,
    notes: document.getElementById('mNotes').value,
    lastUpdated: new Date().toISOString()
  };

  // עדכן זמנית בזיכרון
  var idx = allSubmissions.findIndex(function(s){ return s.ref===ref; });
  if (idx>=0) Object.assign(allSubmissions[idx], updates);

  updateInFirebase(ref, updates)
    .then(function() {
      closeModalDirect();
      showView(currentView);
      // הצג אישור קצר
      var toast = document.createElement('div');
      toast.textContent = '✓ נשמר ב-Firebase';
      toast.style.cssText = 'position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#16a34a;color:white;padding:10px 24px;border-radius:8px;font-size:13px;font-weight:600;z-index:999;font-family:Heebo,sans-serif';
      document.body.appendChild(toast);
      setTimeout(function(){ toast.remove(); }, 2500);
    })
    .catch(function(err) {
      alert('שגיאה בשמירה: ' + err.message);
      showView(currentView);
    });
}

function closeModal(e) { if(e.target===document.getElementById('modalBg')) closeModalDirect(); }
function closeModalDirect() { document.getElementById('modalBg').classList.remove('show'); }

/* =========================================================
   ייצוא CSV
   ========================================================= */
function exportCSV() {
  if (!allSubmissions.length) { alert('אין נתונים לייצוא'); return; }
  var keys = ['ref','timestamp','org','fname','lname','email','phone','evname','evstart','venue','evcat','guestnum','score','manualScore','level','status','notes'];
  var hdrs = ['אסמכתא','תאריך הגשה','ארגון','שם פרטי','שם משפחה','אימייל','טלפון','שם האירוע','תאריך','מיקום','קטגוריה','קהל','ציון אוטומטי','ציון ידני','רמה','סטטוס','הערות'];
  var csv = hdrs.join(',') + '\n';
  allSubmissions.forEach(function(s) {
    csv += keys.map(function(k){ return '"'+(s[k]||'')+'"'; }).join(',') + '\n';
  });
  var blob = new Blob(['\uFEFF'+csv], {type:'text/csv;charset=utf-8'});
  var url = URL.createObjectURL(blob);
  var a = document.createElement('a'); a.href=url; a.download='security_'+new Date().toISOString().slice(0,10)+'.csv'; a.click();
  URL.revokeObjectURL(url);
}

/* =========================================================
   🗑️ מחיקת בקשות מ-Firebase
   ========================================================= */

function deleteFromFirebase(ref) {
  if (!FIREBASE_URL || FIREBASE_URL.includes('YOUR-PROJECT')) {
    // מחק מ-localStorage
    var raw = JSON.parse(localStorage.getItem('securitySubmissions') || '[]');
    raw = raw.filter(function(s){ return s.ref !== ref; });
    localStorage.setItem('securitySubmissions', JSON.stringify(raw));
    return Promise.resolve();
  }
  return fetch(FIREBASE_URL + '/submissions/' + ref + '.json', {
    method: 'DELETE'
  }).then(function(res) {
    if (!res.ok) throw new Error('HTTP ' + res.status);
    return res.json();
  });
}

function confirmDelete(ref, evnameEncoded) {
  var evname = decodeURIComponent(evnameEncoded);
  document.getElementById('deleteRef').textContent = ref;
  document.getElementById('deleteEvname').textContent = evname;
  document.getElementById('deleteModal').classList.add('show');
  document.getElementById('confirmDeleteBtn').onclick = function() {
    executeDelete(ref);
  };
}

function executeDelete(ref) {
  var btn = document.getElementById('confirmDeleteBtn');
  btn.textContent = 'מוחק...';
  btn.disabled = true;

  deleteFromFirebase(ref)
    .then(function() {
      // הסר מהמערך המקומי
      allSubmissions = allSubmissions.filter(function(s){ return s.ref !== ref; });
      closeDeleteModal();
      showView(currentView);
      // Toast הצלחה
      var toast = document.createElement('div');
      toast.textContent = '🗑️ הבקשה נמחקה';
      toast.style.cssText = 'position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#dc2626;color:white;padding:10px 24px;border-radius:8px;font-size:13px;font-weight:600;z-index:9999;font-family:Heebo,sans-serif';
      document.body.appendChild(toast);
      setTimeout(function(){ toast.remove(); }, 2500);
    })
    .catch(function(err) {
      closeDeleteModal();
      alert('שגיאה במחיקה: ' + err.message);
    });
}

function confirmDeleteAll() {
  if (!confirm('⚠️ האם אתה בטוח שברצונך למחוק את כל ההגשות? פעולה זו בלתי הפיכה!')) return;
  if (!confirm('אישור אחרון — מחק הכל?')) return;

  if (!FIREBASE_URL || FIREBASE_URL.includes('YOUR-PROJECT')) {
    localStorage.removeItem('securitySubmissions');
    allSubmissions = [];
    showView(currentView);
    return;
  }

  fetch(FIREBASE_URL + '/submissions.json', { method: 'DELETE' })
    .then(function() {
      allSubmissions = [];
      showView(currentView);
    })
    .catch(function(err) { alert('שגיאה: ' + err.message); });
}

function closeDeleteModal() {
  document.getElementById('deleteModal').classList.remove('show');
  var btn = document.getElementById('confirmDeleteBtn');
  btn.textContent = '🗑️ כן, מחק לצמיתות';
  btn.disabled = false;
}

</script>

<!-- DELETE CONFIRM MODAL -->
<div class="modal-bg" id="deleteModal" onclick="if(event.target===this)closeDeleteModal()">
  <div class="modal" style="max-width:420px">
    <div class="modal-head" style="background:#fff5f5">
      <h3 style="color:#dc2626">🗑️ מחיקת בקשה</h3>
      <button class="close-btn" onclick="closeDeleteModal()">✕</button>
    </div>
    <div class="modal-body" style="text-align:center;padding:2rem">
      <div style="font-size:48px;margin-bottom:12px">⚠️</div>
      <div style="font-size:15px;font-weight:600;color:#1e293b;margin-bottom:8px">האם אתה בטוח?</div>
      <div style="font-size:13px;color:#64748b;margin-bottom:16px;line-height:1.6">
        עומד למחוק את הבקשה:<br>
        <strong id="deleteEvname" style="color:#1e293b"></strong><br>
        <span style="font-family:monospace;font-size:11px;color:#94a3b8" id="deleteRef"></span>
      </div>
      <div style="background:#fee2e2;border-radius:8px;padding:10px 14px;font-size:12px;color:#991b1b;margin-bottom:20px">
        ⚠️ פעולה זו בלתי הפיכה — הנתונים יימחקו לצמיתות מ-Firebase
      </div>
      <div style="display:flex;gap:10px;justify-content:center">
        <button class="btn" onclick="closeDeleteModal()" style="padding:9px 22px">ביטול</button>
        <button id="confirmDeleteBtn" style="padding:9px 22px;background:#dc2626;color:white;border:none;border-radius:7px;font-size:13px;font-weight:600;cursor:pointer;font-family:'Heebo',sans-serif">
          🗑️ כן, מחק לצמיתות
        </button>
      </div>
    </div>
  </div>
</div>

</body>
</html>
