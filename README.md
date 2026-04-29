<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>דשבורד ניהול — הערכת אבטחת אירועים</title>
<link href="https://fonts.googleapis.com/css2?family=Heebo:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --navy:#0f2744;--navy2:#1a3a5c;--blue:#2563eb;--blue-light:#dbeafe;
  --green:#16a34a;--green-bg:#dcfce7;--amber:#d97706;--amber-bg:#fef3c7;
  --red:#dc2626;--red-bg:#fee2e2;
  --border:#e2e8f0;--bg:#f1f5f9;--white:#ffffff;
  --text:#1e293b;--muted:#64748b;--subtle:#94a3b8;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Heebo',sans-serif;background:var(--bg);color:var(--text);direction:rtl;min-height:100vh}
.sidebar{position:fixed;right:0;top:0;bottom:0;width:220px;background:var(--navy);color:white;padding:1.5rem 0;z-index:20;display:flex;flex-direction:column}
.sidebar-logo{padding:0 1.25rem 1.25rem;border-bottom:1px solid rgba(255,255,255,.1);margin-bottom:1rem}
.sidebar-logo h2{font-size:14px;font-weight:600;line-height:1.4}
.sidebar-logo p{font-size:11px;opacity:.6;margin-top:3px}
.nav-item{display:flex;align-items:center;gap:10px;padding:10px 1.25rem;font-size:13px;cursor:pointer;color:rgba(255,255,255,.7);transition:all .15s;border-right:3px solid transparent}
.nav-item:hover{background:rgba(255,255,255,.07);color:white}
.nav-item.active{background:rgba(255,255,255,.1);color:white;border-right-color:#60a5fa}
.nav-icon{font-size:16px;width:20px}
.main{margin-right:220px;min-height:100vh}
.topbar{background:white;border-bottom:1px solid var(--border);padding:12px 1.5rem;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:10}
.topbar h1{font-size:16px;font-weight:600;color:var(--navy2)}
.topbar-right{display:flex;gap:10px;align-items:center}
.btn{padding:7px 16px;border-radius:7px;font-size:12px;font-weight:600;cursor:pointer;font-family:'Heebo',sans-serif;border:1px solid var(--border);background:white;color:var(--muted);transition:all .15s}
.btn:hover{background:var(--bg)}
.btn-primary{background:var(--navy2);color:white;border-color:var(--navy2)}
.btn-primary:hover{background:var(--navy)}
.content{padding:1.5rem}
.stats-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:1.5rem}
@media(max-width:800px){.stats-grid{grid-template-columns:repeat(2,1fr)}}
.stat-card{background:white;border:1px solid var(--border);border-radius:10px;padding:1rem 1.25rem}
.stat-card .label{font-size:11px;color:var(--muted);margin-bottom:4px;font-weight:500}
.stat-card .value{font-size:26px;font-weight:700;color:var(--navy2)}
.stat-card .sub{font-size:11px;color:var(--subtle);margin-top:3px}
.panel{background:white;border:1px solid var(--border);border-radius:10px;margin-bottom:1.5rem}
.panel-header{display:flex;justify-content:space-between;align-items:center;padding:1rem 1.25rem;border-bottom:1px solid var(--border)}
.panel-header h3{font-size:14px;font-weight:600;color:var(--navy2)}
.filter-row{display:flex;gap:8px;padding:10px 1.25rem;border-bottom:1px solid var(--border);flex-wrap:wrap}
.filter-row input,.filter-row select{border:1px solid var(--border);border-radius:7px;padding:6px 10px;font-size:12px;font-family:'Heebo',sans-serif;direction:rtl;background:white}
.filter-row input{flex:1;min-width:160px}
.table-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:12px}
th{text-align:right;padding:9px 12px;background:#f8fafc;color:var(--muted);font-weight:600;font-size:11px;border-bottom:1px solid var(--border);white-space:nowrap}
td{padding:9px 12px;border-bottom:0.5px solid var(--border);color:var(--text);vertical-align:middle}
tr:hover td{background:#f8fafc}
tr:last-child td{border-bottom:none}
.badge{display:inline-block;padding:3px 10px;border-radius:12px;font-size:11px;font-weight:600;white-space:nowrap}
.badge-low{background:var(--green-bg);color:#15803d}
.badge-mid{background:var(--amber-bg);color:#92400e}
.badge-high{background:var(--red-bg);color:#991b1b}
.badge-new{background:#dbeafe;color:#1e40af}
.score-num{font-size:18px;font-weight:700}
.score-low{color:#16a34a}
.score-mid{color:#d97706}
.score-high{color:#dc2626}
.action-btn{padding:4px 10px;border-radius:6px;font-size:11px;font-weight:600;cursor:pointer;font-family:'Heebo',sans-serif;border:1px solid var(--border);background:white;transition:all .1s;margin-right:4px}
.action-btn:hover{background:var(--bg)}
.action-btn.approve{border-color:#16a34a;color:#16a34a}
.action-btn.reject{border-color:#dc2626;color:#dc2626}
.modal-bg{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);z-index:100;align-items:center;justify-content:center;padding:1rem}
.modal-bg.show{display:flex}
.modal{background:white;border-radius:14px;max-width:600px;width:100%;max-height:90vh;overflow-y:auto;direction:rtl}
.modal-head{padding:1.25rem 1.5rem;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;background:white;z-index:2}
.modal-head h3{font-size:16px;font-weight:600;color:var(--navy2)}
.close-btn{background:none;border:none;font-size:20px;cursor:pointer;color:var(--muted);padding:4px 8px}
.modal-body{padding:1.5rem}
.detail-section{margin-bottom:1.25rem}
.detail-section-title{font-size:11px;font-weight:700;color:var(--navy2);text-transform:uppercase;letter-spacing:.06em;border-bottom:1px solid var(--border);padding-bottom:6px;margin-bottom:10px}
.detail-row{display:flex;justify-content:space-between;align-items:center;padding:5px 0;font-size:13px}
.detail-row .dl{color:var(--muted)}
.detail-row .dv{font-weight:500;text-align:left}
.score-big{text-align:center;padding:1rem;background:var(--bg);border-radius:10px;margin-bottom:1.25rem}
.score-big .sn{font-size:52px;font-weight:700}
.gauge{height:10px;background:#e2e8f0;border-radius:20px;overflow:hidden;margin:8px 0}
.gauge-fill{height:100%;border-radius:20px}
.manual-score{display:flex;flex-direction:column;gap:8px;padding:1rem;background:#f8fafc;border-radius:10px;border:1px solid var(--border)}
.manual-score label{font-size:12px;font-weight:600;color:var(--muted)}
.manual-score-row{display:flex;gap:8px;align-items:center}
.manual-score input[type=range]{flex:1;accent-color:var(--navy2)}
.manual-score input[type=number]{width:60px;border:1px solid var(--border);border-radius:6px;padding:5px 8px;font-size:14px;font-weight:700;font-family:'Heebo',sans-serif;text-align:center}
.manual-score select{border:1px solid var(--border);border-radius:6px;padding:6px 10px;font-size:13px;font-family:'Heebo',sans-serif}
.notes-area{width:100%;border:1px solid var(--border);border-radius:8px;padding:8px 12px;font-size:13px;font-family:'Heebo',sans-serif;resize:vertical;min-height:80px;direction:rtl}
.modal-footer{padding:1rem 1.5rem;border-top:1px solid var(--border);display:flex;gap:8px;justify-content:flex-end;position:sticky;bottom:0;background:white}
.empty-state{text-align:center;padding:3rem;color:var(--muted)}
.empty-state .icon{font-size:40px;margin-bottom:12px}
.empty-state h3{font-size:15px;margin-bottom:6px}
.empty-state p{font-size:13px}
.chart-row{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:1.5rem}
@media(max-width:600px){.chart-row{grid-template-columns:1fr}}
.mini-bar-wrap{padding:1rem 1.25rem}
.mini-bar-item{display:flex;align-items:center;gap:8px;margin-bottom:8px;font-size:12px}
.mini-bar-label{min-width:60px;color:var(--muted);text-align:right}
.mini-bar-track{flex:1;height:8px;background:#e2e8f0;border-radius:20px;overflow:hidden}
.mini-bar-fill{height:100%;border-radius:20px}
.mini-bar-val{min-width:24px;text-align:left;font-weight:600;color:var(--text)}
.status-badge{padding:3px 9px;border-radius:12px;font-size:10px;font-weight:600}
.status-new{background:#dbeafe;color:#1e40af}
.status-reviewed{background:#dcfce7;color:#15803d}
.status-pending{background:#fef3c7;color:#92400e}
.status-approved{background:#dcfce7;color:#15803d}
.status-rejected{background:#fee2e2;color:#991b1b}
</style>
</head>
<body>

<div class="sidebar">
  <div class="sidebar-logo">
    <h2>מרכז ניהול אבטחה</h2>
    <p>הערכת סיכוני אירועים</p>
  </div>
  <div class="nav-item active" onclick="showView('dashboard')"><span class="nav-icon">📊</span> דשבורד</div>
  <div class="nav-item" onclick="showView('submissions')"><span class="nav-icon">📋</span> כל הגשות</div>
  <div class="nav-item" onclick="showView('high')"><span class="nav-icon">🔴</span> סיכון גבוה</div>
  <div class="nav-item" onclick="showView('pending')"><span class="nav-icon">⏳</span> ממתינות לבדיקה</div>
  <div style="margin-top:auto;padding:1rem 1.25rem;border-top:1px solid rgba(255,255,255,.1)">
    <div style="font-size:11px;opacity:.5">גרסה 1.0 | dswysh61@gmail.com</div>
  </div>
</div>

<div class="main">
  <div class="topbar">
    <h1 id="pageTitle">דשבורד ראשי</h1>
    <div class="topbar-right">
      <button class="btn" onclick="loadDemoData()">טען נתוני דמו</button>
      <button class="btn" onclick="exportCSV()">ייצא CSV</button>
      <button class="btn btn-primary" onclick="clearAll()">נקה הכל</button>
    </div>
  </div>
  <div class="content" id="mainContent"></div>
</div>

<!-- MODAL -->
<div class="modal-bg" id="modalBg" onclick="closeModal(event)">
  <div class="modal" id="modalBox">
    <div class="modal-head">
      <h3 id="modalTitle">פרטי הגשה</h3>
      <button class="close-btn" onclick="closeModalDirect()">✕</button>
    </div>
    <div class="modal-body" id="modalBody"></div>
    <div class="modal-footer" id="modalFooter"></div>
  </div>
</div>

<script>
var submissions=[];
var currentView='dashboard';

function getSubmissions(){
  return JSON.parse(localStorage.getItem('securitySubmissions')||'[]');
}
function saveSubmissions(d){localStorage.setItem('securitySubmissions',JSON.stringify(d));}

function levelInfo(score){
  if(score<=3)return{label:'נמוכה',cls:'badge-low',color:'#16a34a',fill:'#16a34a',scls:'score-low'};
  if(score<=7)return{label:'בינונית',cls:'badge-mid',color:'#d97706',fill:'#d97706',scls:'score-mid'};
  return{label:'גבוהה',cls:'badge-high',color:'#dc2626',fill:'#dc2626',scls:'score-high'};
}

function showView(v){
  currentView=v;
  document.querySelectorAll('.nav-item').forEach(function(n){n.classList.remove('active');});
  var idx={dashboard:0,submissions:1,high:2,pending:3};
  var navItems=document.querySelectorAll('.nav-item');
  if(navItems[idx[v]])navItems[idx[v]].classList.add('active');
  var titles={dashboard:'דשבורד ראשי',submissions:'כל ההגשות',high:'אירועים בסיכון גבוה',pending:'ממתינות לבדיקה'};
  document.getElementById('pageTitle').textContent=titles[v]||v;
  submissions=getSubmissions();
  if(v==='dashboard')renderDashboard();
  else if(v==='submissions')renderTable(submissions,'כל ההגשות');
  else if(v==='high')renderTable(submissions.filter(function(s){return (s.manualScore||s.score||0)>=8;}),'סיכון גבוה');
  else if(v==='pending')renderTable(submissions.filter(function(s){return !s.status||s.status==='new';}),'ממתינות לבדיקה');
}

function renderDashboard(){
  var subs=getSubmissions();
  var total=subs.length;
  var low=subs.filter(function(s){return (s.manualScore||s.score||0)<=3;}).length;
  var mid=subs.filter(function(s){var sc=s.manualScore||s.score||0;return sc>=4&&sc<=7;}).length;
  var high=subs.filter(function(s){return (s.manualScore||s.score||0)>=8;}).length;
  var pending=subs.filter(function(s){return !s.status||s.status==='new';}).length;
  var avgScore=total?Math.round(subs.reduce(function(a,b){return a+(b.manualScore||b.score||0);},0)/total):0;

  var html='<div class="stats-grid">';
  html+='<div class="stat-card"><div class="label">סה"כ הגשות</div><div class="value">'+total+'</div><div class="sub">כולל כל הסטטוסים</div></div>';
  html+='<div class="stat-card"><div class="label">ממתינות לבדיקה</div><div class="value" style="color:#d97706">'+pending+'</div><div class="sub">לא טופלו עדיין</div></div>';
  html+='<div class="stat-card"><div class="label">סיכון גבוה</div><div class="value" style="color:#dc2626">'+high+'</div><div class="sub">ציון 8 ומעלה</div></div>';
  html+='<div class="stat-card"><div class="label">ציון ממוצע</div><div class="value">'+avgScore+'</div><div class="sub">מתוך 10</div></div>';
  html+='</div>';

  html+='<div class="chart-row">';
  html+='<div class="panel"><div class="panel-header"><h3>התפלגות רמות סיכון</h3></div><div class="mini-bar-wrap">';
  var max=Math.max(low,mid,high,1);
  function bar(label,count,color){return '<div class="mini-bar-item"><span class="mini-bar-label">'+label+'</span><div class="mini-bar-track"><div class="mini-bar-fill" style="width:'+Math.round((count/max)*100)+'%;background:'+color+'"></div></div><span class="mini-bar-val">'+count+'</span></div>';}
  html+=bar('נמוכה',low,'#16a34a');html+=bar('בינונית',mid,'#d97706');html+=bar('גבוהה',high,'#dc2626');
  html+='</div></div>';

  html+='<div class="panel"><div class="panel-header"><h3>אירועים אחרונים</h3></div>';
  var recent=subs.slice(-5).reverse();
  if(!recent.length){html+='<div style="padding:1rem 1.25rem;font-size:12px;color:var(--muted)">אין הגשות עדיין</div>';}
  else{html+='<table><thead><tr><th>אירוע</th><th>ציון</th><th>סטטוס</th></tr></thead><tbody>';
  recent.forEach(function(s){var li=levelInfo(s.manualScore||s.score||0);var st=s.status||'new';
    html+='<tr><td>'+s.evname+'</td><td><span class="'+li.scls+'" style="font-weight:700">'+(s.manualScore||s.score||0)+'</span></td>';
    html+='<td><span class="status-badge status-'+st+'">'+{new:'חדש',reviewed:'נבדק',approved:'אושר',rejected:'נדחה',pending:'ממתין'}[st]+'</span></td></tr>';});
  html+='</tbody></table>';}
  html+='</div></div>';

  if(subs.length){
    html+='<div class="panel"><div class="panel-header"><h3>כל ההגשות</h3></div>';
    html+=buildTableHTML(subs.slice(-10).reverse());
    html+='</div>';
  }

  document.getElementById('mainContent').innerHTML=html;
}

function buildTableHTML(list){
  if(!list.length)return '<div class="empty-state"><div class="icon">📭</div><h3>אין הגשות</h3><p>טפסים שיוגשו יופיעו כאן</p></div>';
  var html='<div class="table-wrap"><table><thead><tr><th>אסמכתא</th><th>ארגון</th><th>שם האירוע</th><th>תאריך</th><th>ציון</th><th>רמת סיכון</th><th>סטטוס</th><th>פעולות</th></tr></thead><tbody>';
  list.forEach(function(s,i){
    var sc=s.manualScore||s.score||0;
    var li=levelInfo(sc);var st=s.status||'new';
    var idx=getSubmissions().findIndex(function(x){return x.ref===s.ref;});
    html+='<tr>';
    html+='<td style="font-family:monospace;font-size:11px;color:#64748b">'+s.ref+'</td>';
    html+='<td>'+s.org+'</td>';
    html+='<td style="font-weight:500">'+s.evname+'</td>';
    html+='<td style="color:#64748b">'+s.evstart+'</td>';
    html+='<td><span class="score-num '+li.scls+'">'+sc+'</span></td>';
    html+='<td><span class="badge '+li.cls+'">'+li.label+'</span></td>';
    html+='<td><span class="status-badge status-'+st+'">'+{new:'חדש',reviewed:'נבדק',approved:'אושר',rejected:'נדחה',pending:'ממתין'}[st]+'</span></td>';
    html+='<td><button class="action-btn" onclick="openDetail(\''+s.ref+'\')">צפה / דרג</button></td>';
    html+='</tr>';
  });
  html+='</tbody></table></div>';
  return html;
}

function renderTable(list,title){
  var html='<div class="panel"><div class="panel-header"><h3>'+title+' ('+list.length+')</h3></div>';
  html+='<div class="filter-row"><input type="text" placeholder="חיפוש..." oninput="filterTable(this.value)"><select onchange="filterByLevel(this.value)"><option value="">כל הרמות</option><option value="low">נמוכה</option><option value="mid">בינונית</option><option value="high">גבוהה</option></select></div>';
  html+=buildTableHTML(list);
  html+='</div>';
  document.getElementById('mainContent').innerHTML=html;
}

var filterList=[];
function filterTable(q){
  var subs=getSubmissions();
  var filtered=subs.filter(function(s){
    return (s.evname||'').includes(q)||(s.org||'').includes(q)||(s.ref||'').includes(q);
  });
  document.querySelector('.table-wrap').outerHTML=buildTableHTML(filtered);
}
function filterByLevel(level){
  var subs=getSubmissions();
  var filtered=subs;
  if(level==='low')filtered=subs.filter(function(s){return (s.manualScore||s.score||0)<=3;});
  else if(level==='mid')filtered=subs.filter(function(s){var sc=s.manualScore||s.score||0;return sc>=4&&sc<=7;});
  else if(level==='high')filtered=subs.filter(function(s){return (s.manualScore||s.score||0)>=8;});
  var tw=document.querySelector('.table-wrap');if(tw)tw.outerHTML=buildTableHTML(filtered);
}

function openDetail(ref){
  var subs=getSubmissions();
  var s=subs.find(function(x){return x.ref===ref;});
  if(!s)return;
  var sc=s.manualScore||s.score||0;
  var li=levelInfo(sc);
  var pct=Math.round((sc/10)*100);

  var html='<div class="score-big"><div class="sn '+li.scls+'">'+sc+'</div>';
  html+='<div style="font-size:12px;color:#64748b;margin-top:2px">מתוך 10</div>';
  html+='<div class="gauge"><div class="gauge-fill" style="width:'+pct+'%;background:'+li.fill+'"></div></div>';
  html+='<span class="badge '+li.cls+'" style="font-size:13px">רמת אבטחה: '+li.label+'</span></div>';

  function dr(l,v){var dv=v||'—';if(v==='כן')dv='<span style="background:#dbeafe;color:#1e40af;padding:2px 8px;border-radius:10px;font-size:11px;font-weight:600">כן</span>';else if(v==='לא')dv='<span style="background:#f1f5f9;color:#64748b;padding:2px 8px;border-radius:10px;font-size:11px">לא</span>';return '<div class="detail-row"><span class="dl">'+l+'</span><span class="dv">'+dv+'</span></div>';}

  html+='<div class="detail-section"><div class="detail-section-title">פרטי ארגון ואיש קשר</div>';
  html+=dr('ארגון',s.org);html+=dr('איש קשר',(s.fname||'')+" "+(s.lname||''));html+=dr('אימייל',s.email);html+=dr('טלפון',s.phone)+'</div>';
  html+='<div class="detail-section"><div class="detail-section-title">פרטי האירוע</div>';
  html+=dr('שם האירוע',s.evname);html+=dr('תאריכים',(s.evstart||'')+(s.evend&&s.evend!==s.evstart?' — '+s.evend:''));
  html+=dr('מיקום',s.venue);html+=dr('קטגוריה',s.evcat);html+=dr('קהל',(function(){var m={small:'עד 100',med:'100–500',large:'500–1000',xlarge:'מעל 1000'};return m[s.guestnum]||'—';})());html+='</div>';
  html+='<div class="detail-section"><div class="detail-section-title">גורמי סיכון</div>';
  html+=dr('שעות לילה',s.latenight);html+=dr('קרבה לממשלה',s.govprox);html+=dr('אלכוהול',s.alcohol);
  html+=dr('כניסה פתוחת לציבור',s.entrytype==='open'?'כן':'לא');html+=dr('ללא סינון',s.vetted==='לא'?'כן':'לא');
  html+=dr('אישים בכירים',s.vip);if(s.vip_detail)html+=dr('פירוט VIP',s.vip_detail);
  html+=dr('הפגנות צפויות',s.protest);html+=dr('היסטוריית אלימות',s.history);html+=dr('ספורט מגע',s.contact)+'</div>';
  if(s.extra)html+='<div class="detail-section"><div class="detail-section-title">מידע נוסף</div><div style="font-size:13px;line-height:1.6">'+s.extra+'</div></div>';

  html+='<div class="detail-section"><div class="detail-section-title">דירוג ידני (לפי שיקול דעתך)</div>';
  html+='<div class="manual-score">';
  html+='<label>שנה ציון</label>';
  html+='<div class="manual-score-row">';
  html+='<input type="range" min="0" max="10" step="1" value="'+sc+'" id="manualRange" oninput="document.getElementById(\'manualNum\').value=this.value">';
  html+='<input type="number" min="0" max="10" value="'+sc+'" id="manualNum" oninput="document.getElementById(\'manualRange\').value=this.value">';
  html+='</div>';
  html+='<label style="margin-top:8px">עדכן סטטוס</label>';
  html+='<select id="statusSel"><option value="new"'+(s.status==='new'?' selected':'')+'>חדש</option><option value="reviewed"'+(s.status==='reviewed'?' selected':'')+'>נבדק</option><option value="approved"'+(s.status==='approved'?' selected':'')+'>אושר</option><option value="pending"'+(s.status==='pending'?' selected':'')+'>ממתין למידע</option><option value="rejected"'+(s.status==='rejected'?' selected':'')+'>נדחה</option></select>';
  html+='<label style="margin-top:8px">הערות פנימיות</label>';
  html+='<textarea class="notes-area" id="notesArea" placeholder="הוסף הערות פנימיות...">'+(s.notes||'')+'</textarea>';
  html+='</div></div>';

  document.getElementById('modalTitle').textContent=s.evname;
  document.getElementById('modalBody').innerHTML=html;
  document.getElementById('modalFooter').innerHTML='<button class="btn" onclick="closeModalDirect()">סגור</button><button class="btn btn-primary" onclick="saveDetail(\''+ref+'\')">שמור שינויים</button>';
  document.getElementById('modalBg').classList.add('show');
}

function saveDetail(ref){
  var subs=getSubmissions();
  var idx=subs.findIndex(function(x){return x.ref===ref;});
  if(idx===-1)return;
  subs[idx].manualScore=parseInt(document.getElementById('manualNum').value)||0;
  subs[idx].status=document.getElementById('statusSel').value;
  subs[idx].notes=document.getElementById('notesArea').value;
  saveSubmissions(subs);
  closeModalDirect();
  showView(currentView);
}

function closeModal(e){if(e.target===document.getElementById('modalBg'))closeModalDirect();}
function closeModalDirect(){document.getElementById('modalBg').classList.remove('show');}

function loadDemoData(){
  var demo=[
    {ref:'SEC-DEMO01',org:'ארגון קהילת רמות',fname:'דוד',lname:'לוי',email:'david@org.il',phone:'050-1111111',evname:'ערב גיוס תרומות שנתי',evstart:'2026-06-15',evend:'2026-06-15',venue:'מלון הילטון תל אביב',evcat:'צדקה / גיוס כספים',guestnum:'large',entrytype:'invite',latenight:'כן',govprox:'לא',alcohol:'כן',advertised:'כן',vip:'כן',vip_detail:'חבר כנסת ושגריר',protest:'לא',history:'לא',contact:'לא',vetted:'כן',police:'כן',score:7,status:'new'},
    {ref:'SEC-DEMO02',org:'מועדון ספורט הפועל',fname:'מיכל',lname:'כהן',email:'michal@sport.il',phone:'052-2222222',evname:'ליגת כדורגל עירונית',evstart:'2026-05-20',evend:'2026-05-20',venue:'אצטדיון טדי ירושלים',evcat:'אירוע ספורט',guestnum:'xlarge',entrytype:'ticketed',latenight:'לא',govprox:'לא',alcohol:'כן',advertised:'כן',vip:'לא',protest:'כן',history:'כן',contact:'כן',vetted:'לא',police:'כן',score:9,status:'reviewed'},
    {ref:'SEC-DEMO03',org:'קהילת בית כנסת',fname:'יוסף',lname:'שמעוני',email:'yossi@bk.il',phone:'054-3333333',evname:'תפילות ראש השנה',evstart:'2026-09-22',evend:'2026-09-24',venue:'בית כנסת הגדול',evcat:'אירוע דתי',guestnum:'med',entrytype:'community',latenight:'לא',govprox:'לא',alcohol:'לא',advertised:'לא',vip:'לא',protest:'לא',history:'לא',contact:'לא',vetted:'לא',police:'לא',score:1,status:'approved'},
    {ref:'SEC-DEMO04',org:'חברת הייטק',fname:'שרה',lname:'גלברג',email:'sarah@tech.il',phone:'053-4444444',evname:'כנס שנתי טכנולוגיה',evstart:'2026-07-10',evend:'2026-07-11',venue:'מרכז הכנסים אביב',evcat:'כנס מקצועי',guestnum:'large',entrytype:'ticketed',latenight:'לא',govprox:'כן',alcohol:'כן',advertised:'כן',vip:'כן',vip_detail:'מנכ"ל ושרים',protest:'לא',history:'לא',contact:'לא',vetted:'כן',police:'כן',score:6,status:'pending'},
    {ref:'SEC-DEMO05',org:'עמותת נוער',fname:'רון',lname:'ברק',email:'ron@youth.il',phone:'058-5555555',evname:'מסיבת קיץ פתוחה',evstart:'2026-08-05',evend:'2026-08-05',venue:'גן העיר חיפה',evcat:'אירוע קהילתי',guestnum:'xlarge',entrytype:'open',latenight:'כן',govprox:'לא',alcohol:'כן',advertised:'כן',vip:'לא',protest:'לא',history:'כן',contact:'לא',vetted:'לא',police:'לא',score:8,status:'new'},
  ];
  saveSubmissions(demo);
  showView(currentView);
}

function exportCSV(){
  var subs=getSubmissions();
  if(!subs.length){alert('אין נתונים לייצוא');return;}
  var keys=['ref','org','fname','lname','email','evname','evstart','venue','guestnum','score','manualScore','status'];
  var hdrs=['אסמכתא','ארגון','שם פרטי','שם משפחה','אימייל','שם האירוע','תאריך','מיקום','קהל','ציון אוטומטי','ציון ידני','סטטוס'];
  var csv=hdrs.join(',')+'\n';
  subs.forEach(function(s){csv+=keys.map(function(k){return '"'+(s[k]||'')+'"';}).join(',')+'\n';});
  var blob=new Blob(['\uFEFF'+csv],{type:'text/csv;charset=utf-8'});
  var url=URL.createObjectURL(blob);var a=document.createElement('a');
  a.href=url;a.download='security_submissions.csv';a.click();URL.revokeObjectURL(url);
}

function clearAll(){if(confirm('למחוק את כל הנתונים?')){localStorage.removeItem('securitySubmissions');showView(currentView);}}

showView('dashboard');
</script>
</body>
</html>
