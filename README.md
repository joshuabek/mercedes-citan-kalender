<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Mercedes Citan – Buchungskalender</title>
<link href="https://fonts.googleapis.com/css2?family=Segoe+UI:wght@400;600;700&display=swap" rel="stylesheet"/>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --blue: #0078d4;
    --blue-light: #deecf9;
    --blue-dark: #005a9e;
    --gray1: #323130;
    --gray2: #605e5c;
    --gray3: #8a8886;
    --gray4: #edebe9;
    --gray5: #f3f2f1;
    --white: #ffffff;
    --red: #a4262c;
    --warn-bg: #fff4ce;
    --warn-border: #c19c00;
    --warn-text: #433500;
  }

  body { font-family: 'Segoe UI', Arial, sans-serif; background: var(--gray5); min-height: 100vh; display: flex; flex-direction: column; color: var(--gray1); }

  /* TOP BAR */
  .topbar { background: var(--blue); color: #fff; height: 48px; display: flex; align-items: center; padding: 0 18px; gap: 16px; box-shadow: 0 2px 6px rgba(0,0,0,0.25); flex-shrink: 0; z-index: 10; }
  .topbar-title { font-size: 17px; font-weight: 700; letter-spacing: 0.3px; display: flex; align-items: center; gap: 8px; }
  .topbar-title svg { width: 22px; height: 22px; fill: #fff; }
  .topbar .spacer { flex: 1; }
  .btn-new-top { background: #fff; color: var(--blue); border: none; border-radius: 2px; padding: 6px 18px; font-weight: 700; font-size: 13px; cursor: pointer; display: flex; align-items: center; gap: 5px; transition: background 0.15s; }
  .btn-new-top:hover { background: var(--blue-light); }

  /* LAYOUT */
  .layout { display: flex; flex: 1; min-height: 0; overflow: hidden; }

  /* SIDEBAR */
  .sidebar { width: 220px; background: var(--white); border-right: 1px solid var(--gray4); display: flex; flex-direction: column; flex-shrink: 0; }
  .mini-cal { padding: 14px 12px; border-bottom: 1px solid var(--gray4); }
  .mini-cal-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
  .mini-cal-header span { font-size: 12px; font-weight: 700; color: var(--gray1); }
  .mini-nav { background: none; border: none; cursor: pointer; font-size: 16px; color: var(--gray2); padding: 2px 6px; border-radius: 2px; }
  .mini-nav:hover { background: var(--gray5); }
  .mini-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 1px; }
  .mini-dow { text-align: center; font-size: 10px; font-weight: 700; color: var(--gray2); padding: 3px 0; }
  .mini-day { text-align: center; font-size: 11px; padding: 4px 2px; border-radius: 50%; cursor: pointer; position: relative; color: var(--gray1); width: 26px; height: 26px; display: flex; align-items: center; justify-content: center; margin: 1px auto; flex-direction: column; }
  .mini-day:hover { background: var(--gray5); }
  .mini-day.today { background: var(--blue-light); color: var(--blue); font-weight: 700; }
  .mini-day.selected { background: var(--blue); color: #fff; font-weight: 700; }
  .mini-dot { width: 4px; height: 4px; border-radius: 50%; background: var(--blue); margin-top: 1px; }
  .mini-day.selected .mini-dot { background: #fff; }

  .sidebar-section { padding: 10px 12px; }
  .sidebar-label { font-size: 11px; font-weight: 700; color: var(--gray2); text-transform: uppercase; letter-spacing: 0.6px; margin-bottom: 8px; }
  .view-btn { display: block; width: 100%; text-align: left; padding: 7px 10px; border: none; background: transparent; border-radius: 2px; cursor: pointer; font-size: 13px; color: var(--gray1); margin-bottom: 2px; }
  .view-btn:hover { background: var(--gray5); }
  .view-btn.active { background: var(--blue-light); color: var(--blue); font-weight: 700; }

  .sidebar-stats { margin-top: auto; padding: 12px; border-top: 1px solid var(--gray4); }
  .stat-row { display: flex; justify-content: space-between; font-size: 12px; color: var(--gray1); margin-bottom: 5px; }
  .stat-row span:last-child { font-weight: 700; }

  /* MAIN */
  .main { flex: 1; display: flex; flex-direction: column; min-width: 0; overflow: hidden; }

  /* TOOLBAR */
  .toolbar { background: var(--white); border-bottom: 1px solid var(--gray4); padding: 8px 16px; display: flex; align-items: center; gap: 8px; flex-shrink: 0; }
  .btn-today { background: var(--white); border: 1px solid var(--gray3); border-radius: 2px; padding: 5px 14px; cursor: pointer; font-size: 13px; color: var(--gray1); }
  .btn-today:hover { background: var(--gray5); }
  .nav-group { display: flex; border: 1px solid var(--gray3); border-radius: 2px; overflow: hidden; }
  .nav-group button { background: var(--white); border: none; font-size: 18px; padding: 4px 12px; cursor: pointer; color: var(--gray1); border-right: 1px solid var(--gray3); line-height: 1; }
  .nav-group button:last-child { border-right: none; }
  .nav-group button:hover { background: var(--gray5); }
  .toolbar-title { font-size: 18px; font-weight: 700; color: var(--gray1); flex: 1; }
  .view-group { display: flex; border: 1px solid var(--gray3); border-radius: 2px; overflow: hidden; }
  .view-group button { background: var(--white); border: none; border-right: 1px solid var(--gray3); padding: 5px 14px; cursor: pointer; font-size: 13px; color: var(--gray1); }
  .view-group button:last-child { border-right: none; }
  .view-group button:hover { background: var(--gray5); }
  .view-group button.active { background: var(--blue-light); color: var(--blue); font-weight: 700; }
  .btn-add { background: var(--blue); color: #fff; border: none; border-radius: 2px; padding: 5px 16px; cursor: pointer; font-size: 13px; font-weight: 700; }
  .btn-add:hover { background: var(--blue-dark); }

  /* MONTH VIEW */
  .cal-area { flex: 1; overflow: auto; }
  .month-grid { display: grid; grid-template-columns: repeat(7, 1fr); }
  .month-dow { background: var(--white); text-align: center; padding: 8px 0; font-size: 12px; font-weight: 700; color: var(--gray2); border-bottom: 1px solid var(--gray4); border-right: 1px solid var(--gray4); }
  .month-cell { background: var(--white); border-bottom: 1px solid var(--gray4); border-right: 1px solid var(--gray4); min-height: 100px; padding: 4px 3px; cursor: pointer; }
  .month-cell:hover { background: #faf9f8; }
  .month-cell.other-month { opacity: 0.4; }
  .day-num { width: 26px; height: 26px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 13px; margin-bottom: 2px; }
  .day-num.today { background: var(--blue); color: #fff; font-weight: 700; }
  .booking-pill { border-radius: 2px; font-size: 11px; padding: 1px 5px; margin-bottom: 2px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; color: #fff; cursor: pointer; }
  .booking-pill:hover { filter: brightness(0.9); }
  .more-label { font-size: 10px; color: var(--gray2); padding-left: 4px; }

  /* WEEK VIEW */
  .week-header { display: grid; grid-template-columns: 54px repeat(7, 1fr); background: var(--white); border-bottom: 1px solid var(--gray4); flex-shrink: 0; }
  .week-header-empty { border-right: 1px solid var(--gray4); }
  .week-header-day { text-align: center; padding: 8px 0; border-right: 1px solid var(--gray4); cursor: pointer; }
  .week-header-day:hover { background: var(--gray5); }
  .week-dow { font-size: 11px; font-weight: 700; color: var(--gray2); }
  .week-daynum { width: 30px; height: 30px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 16px; margin: 2px auto 0; }
  .week-daynum.today { background: var(--blue); color: #fff; font-weight: 700; }
  .time-grid { position: relative; }
  .time-row { display: grid; grid-template-columns: 54px repeat(7, 1fr); }
  .time-label { text-align: right; padding-right: 8px; font-size: 10px; color: var(--gray2); border-right: 1px solid var(--gray4); border-bottom: 1px solid var(--gray4); height: 60px; display: flex; align-items: flex-start; padding-top: 4px; }
  .time-cell { border-right: 1px solid var(--gray4); border-bottom: 1px solid var(--gray4); height: 60px; cursor: pointer; }
  .time-cell:hover { background: #f9f8f7; }
  .time-cell.today-col { background: #faf9f8; }
  .week-booking { position: absolute; border-radius: 3px; color: #fff; font-size: 11px; padding: 2px 5px; overflow: hidden; cursor: pointer; box-shadow: 0 1px 3px rgba(0,0,0,0.2); z-index: 2; }
  .week-booking:hover { filter: brightness(0.9); }
  .week-booking-title { font-weight: 700; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

  /* DAY VIEW */
  .day-header { background: var(--white); border-bottom: 1px solid var(--gray4); padding: 10px 16px; display: flex; align-items: center; gap: 14px; flex-shrink: 0; }
  .day-circle { width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 20px; font-weight: 700; background: var(--blue-light); color: var(--blue); }
  .day-circle.today { background: var(--blue); color: #fff; }
  .day-dow { font-weight: 700; font-size: 15px; }
  .day-month { font-size: 12px; color: var(--gray2); }
  .day-grid { position: relative; }
  .day-row { display: grid; grid-template-columns: 64px 1fr; }
  .day-label { text-align: right; padding-right: 10px; font-size: 11px; color: var(--gray2); border-right: 1px solid var(--gray4); border-bottom: 1px solid var(--gray4); height: 64px; display: flex; align-items: flex-start; padding-top: 4px; }
  .day-cell { border-bottom: 1px solid var(--gray4); height: 64px; cursor: pointer; }
  .day-cell:hover { background: #f9f8f7; }
  .day-booking { position: absolute; border-radius: 4px; color: #fff; padding: 5px 12px; cursor: pointer; box-shadow: 0 2px 6px rgba(0,0,0,0.15); z-index: 2; }
  .day-booking:hover { filter: brightness(0.9); }
  .day-booking-title { font-weight: 700; font-size: 13px; }
  .day-booking-sub { font-size: 12px; opacity: 0.9; }
  .day-booking-km { font-size: 11px; opacity: 0.8; }

  /* MODAL */
  .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.4); z-index: 100; display: flex; align-items: center; justify-content: center; }
  .modal { background: var(--white); border-radius: 4px; box-shadow: 0 8px 32px rgba(0,0,0,0.25); width: 520px; max-height: 90vh; overflow-y: auto; }
  .modal-header { background: var(--blue); color: #fff; padding: 14px 20px; display: flex; align-items: center; justify-content: space-between; }
  .modal-header span { font-weight: 700; font-size: 15px; }
  .modal-close { background: none; border: none; color: #fff; font-size: 22px; cursor: pointer; line-height: 1; }
  .modal-body { padding: 20px; display: flex; flex-direction: column; gap: 14px; }
  .field label { display: block; font-size: 12px; font-weight: 700; color: var(--gray1); margin-bottom: 4px; }
  .field input, .field select, .field textarea { width: 100%; padding: 6px 9px; border: 1px solid var(--gray3); border-radius: 2px; font-size: 13px; font-family: inherit; outline: none; }
  .field input:focus, .field select:focus, .field textarea:focus { border-color: var(--blue); box-shadow: 0 0 0 2px rgba(0,120,212,0.15); }
  .field textarea { resize: vertical; }
  .field-row { display: grid; gap: 10px; }
  .field-row.cols3 { grid-template-columns: 1fr 1fr 1fr; }
  .field-row.cols2 { grid-template-columns: 1fr 1fr; }
  .colors { display: flex; gap: 8px; flex-wrap: wrap; }
  .color-dot { width: 28px; height: 28px; border-radius: 50%; cursor: pointer; border: 3px solid transparent; transition: border-color 0.1s; }
  .color-dot.selected { border-color: var(--gray1); }
  .warning-box { background: var(--warn-bg); border: 1px solid var(--warn-border); border-radius: 3px; padding: 9px 13px; font-size: 12px; color: var(--warn-text); }
  .modal-actions { display: flex; gap: 8px; justify-content: flex-end; padding-top: 4px; }
  .btn-cancel { background: var(--white); border: 1px solid var(--gray3); border-radius: 2px; padding: 6px 16px; cursor: pointer; font-size: 13px; color: var(--gray1); }
  .btn-cancel:hover { background: var(--gray5); }
  .btn-save { background: var(--blue); color: #fff; border: none; border-radius: 2px; padding: 6px 18px; cursor: pointer; font-size: 13px; font-weight: 700; }
  .btn-save:hover { background: var(--blue-dark); }
  .btn-delete { background: var(--red); color: #fff; border: none; border-radius: 2px; padding: 6px 16px; cursor: pointer; font-size: 13px; margin-right: auto; }
  .btn-delete:hover { background: #8e1a1f; }

  /* CONFIRM */
  .confirm-box { background: var(--white); border-radius: 4px; box-shadow: 0 4px 20px rgba(0,0,0,0.2); padding: 24px; width: 340px; }
  .confirm-title { font-weight: 700; font-size: 15px; margin-bottom: 8px; }
  .confirm-msg { font-size: 13px; color: var(--gray2); margin-bottom: 20px; }
  .confirm-actions { display: flex; gap: 8px; justify-content: flex-end; }

  .hidden { display: none !important; }
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
  <div class="topbar-title">
    <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M20 8h-3V4H3c-1.1 0-2 .9-2 2v11h2c0 1.66 1.34 3 3 3s3-1.34 3-3h6c0 1.66 1.34 3 3 3s3-1.34 3-3h2v-5l-3-4zM6 18.5c-.83 0-1.5-.67-1.5-1.5s.67-1.5 1.5-1.5 1.5.67 1.5 1.5-.67 1.5-1.5 1.5zm13.5-9l1.96 2.5H17V9.5h2.5zm-1.5 9c-.83 0-1.5-.67-1.5-1.5s.67-1.5 1.5-1.5 1.5.67 1.5 1.5-.67 1.5-1.5 1.5z"/></svg>
    Mercedes Citan – Buchungskalender
  </div>
  <div class="spacer"></div>
  <button class="btn-new-top" onclick="openModal()">+ Neue Buchung</button>
</div>

<div class="layout">
  <!-- SIDEBAR -->
  <div class="sidebar">
    <div class="mini-cal">
      <div class="mini-cal-header">
        <button class="mini-nav" onclick="miniPrev()">‹</button>
        <span id="mini-label"></span>
        <button class="mini-nav" onclick="miniNext()">›</button>
      </div>
      <div class="mini-grid">
        <div class="mini-dow">M</div><div class="mini-dow">D</div><div class="mini-dow">M</div>
        <div class="mini-dow">D</div><div class="mini-dow">F</div><div class="mini-dow">S</div><div class="mini-dow">S</div>
        <div id="mini-days"></div>
      </div>
    </div>

    <div class="sidebar-section">
      <div class="sidebar-label">Ansicht</div>
      <button class="view-btn active" id="sb-month" onclick="setView('month')">Monat</button>
      <button class="view-btn" id="sb-week" onclick="setView('week')">Woche</button>
      <button class="view-btn" id="sb-day" onclick="setView('day')">Tag</button>
    </div>

    <div class="sidebar-stats">
      <div class="sidebar-label">Übersicht</div>
      <div class="stat-row"><span>Buchungen gesamt</span><span id="stat-total">0</span></div>
      <div class="stat-row"><span>Km geplant</span><span id="stat-km">0</span></div>
    </div>
  </div>

  <!-- MAIN -->
  <div class="main">
    <!-- TOOLBAR -->
    <div class="toolbar">
      <button class="btn-today" onclick="goToday()">Heute</button>
      <div class="nav-group">
        <button onclick="navPrev()">‹</button>
        <button onclick="navNext()">›</button>
      </div>
      <span class="toolbar-title" id="toolbar-title"></span>
      <div class="view-group">
        <button id="vg-month" class="active" onclick="setView('month')">Monat</button>
        <button id="vg-week" onclick="setView('week')">Woche</button>
        <button id="vg-day" onclick="setView('day')">Tag</button>
      </div>
      <button class="btn-add" onclick="openModal()">+ Buchung</button>
    </div>

    <div class="cal-area" id="cal-area"></div>
  </div>
</div>

<!-- MODAL -->
<div class="modal-overlay hidden" id="modal-overlay">
  <div class="modal">
    <div class="modal-header">
      <span id="modal-title">Neue Buchung</span>
      <button class="modal-close" onclick="closeModal()">×</button>
    </div>
    <div class="modal-body">
      <div class="field"><label>Titel *</label><input id="f-title" placeholder="z.B. Kundenbesuch Hamburg"/></div>
      <div class="field"><label>Fahrer / Person *</label><input id="f-person" placeholder="Name des Fahrers"/></div>
      <div class="field-row cols3">
        <div class="field"><label>Datum *</label><input type="date" id="f-date" oninput="checkConflict()"/></div>
        <div class="field"><label>Von</label><input type="time" id="f-start" value="08:00"/></div>
        <div class="field"><label>Bis</label><input type="time" id="f-end" value="17:00"/></div>
      </div>
      <div class="field-row cols2">
        <div class="field"><label>Zweck / Kategorie</label>
          <select id="f-purpose">
            <option value="">-- wählen --</option>
            <option>Kundentermin</option><option>Lieferung</option><option>Messe</option>
            <option>Dienstreise</option><option>Einkauf</option><option>Sonstiges</option>
          </select>
        </div>
        <div class="field"><label>Geplante km</label><input type="number" id="f-km" placeholder="0"/></div>
      </div>
      <div class="field">
        <label>Farbe</label>
        <div class="colors" id="color-picker"></div>
      </div>
      <div class="field"><label>Notizen</label><textarea id="f-notes" rows="2" placeholder="Zusätzliche Informationen..."></textarea></div>
      <div id="conflict-box" class="warning-box hidden"></div>
      <div class="modal-actions">
        <button class="btn-delete hidden" id="btn-delete" onclick="confirmDelete()">🗑 Löschen</button>
        <button class="btn-cancel" onclick="closeModal()">Abbrechen</button>
        <button class="btn-save" onclick="saveBooking()">Buchung erstellen</button>
      </div>
    </div>
  </div>
</div>

<!-- CONFIRM DELETE -->
<div class="modal-overlay hidden" id="confirm-overlay">
  <div class="confirm-box">
    <div class="confirm-title">Buchung löschen?</div>
    <div class="confirm-msg">Diese Buchung wird unwiderruflich gelöscht.</div>
    <div class="confirm-actions">
      <button class="btn-cancel" onclick="document.getElementById('confirm-overlay').classList.add('hidden')">Abbrechen</button>
      <button class="btn-delete" onclick="doDelete()">Löschen</button>
    </div>
  </div>
</div>

<script>
const MONTHS = ["Januar","Februar","März","April","Mai","Juni","Juli","August","September","Oktober","November","Dezember"];
const DAYS_SHORT = ["Mo","Di","Mi","Do","Fr","Sa","So"];
const COLORS = ["#0078d4","#107c10","#d83b01","#8764b8","#038387","#c19c00","#e3008c","#004e8c"];
const HOURS = Array.from({length:14},(_,i)=>i+6);

const today = new Date();
let currentYear = today.getFullYear();
let currentMonth = today.getMonth();
let view = "month";
let selectedDay = new Date(today);
let weekStart = getWeekStart(today);
let currentDay = new Date(today);
let editId = null;
let selectedColor = "#0078d4";

let bookings = [
  { id:1, title:"Kundenbesuch München", person:"Anna Becker", start:new Date(today.getFullYear(),today.getMonth(),today.getDate()+1,8,0), end:new Date(today.getFullYear(),today.getMonth(),today.getDate()+1,17,0), purpose:"Kundentermin", color:"#0078d4", km:340, notes:"" },
  { id:2, title:"Messe Frankfurt", person:"Thomas Klein", start:new Date(today.getFullYear(),today.getMonth(),today.getDate()+3,7,0), end:new Date(today.getFullYear(),today.getMonth(),today.getDate()+5,18,0), purpose:"Messe", color:"#107c10", km:180, notes:"" }
];

function getWeekStart(d) {
  const r = new Date(d);
  const day = (r.getDay()+6)%7;
  r.setDate(r.getDate()-day);
  r.setHours(0,0,0,0);
  return r;
}

function sameDay(a,b) { return a.getFullYear()===b.getFullYear()&&a.getMonth()===b.getMonth()&&a.getDate()===b.getDate(); }

function bookingsForDay(date) {
  return bookings.filter(b => {
    const s=new Date(b.start); s.setHours(0,0,0,0);
    const e=new Date(b.end); e.setHours(0,0,0,0);
    const t=new Date(date); t.setHours(0,0,0,0);
    return t>=s && t<=e;
  });
}

function fmt2(n){ return String(n).padStart(2,"0"); }
function fmtDate(d){ return `${d.getFullYear()}-${fmt2(d.getMonth()+1)}-${fmt2(d.getDate())}`; }

// ---- RENDER ----
function render() {
  renderMiniCal();
  renderToolbar();
  renderView();
  renderStats();
  updateViewButtons();
}

function renderStats() {
  document.getElementById("stat-total").textContent = bookings.length;
  document.getElementById("stat-km").textContent = bookings.reduce((s,b)=>s+(Number(b.km)||0),0);
}

function renderToolbar() {
  let label = "";
  if (view==="month") label = `${MONTHS[currentMonth]} ${currentYear}`;
  else if (view==="week") {
    const we = new Date(weekStart); we.setDate(we.getDate()+6);
    label = `${weekStart.getDate()}. ${MONTHS[weekStart.getMonth()]} – ${we.getDate()}. ${MONTHS[we.getMonth()]} ${we.getFullYear()}`;
  } else {
    label = `${currentDay.getDate()}. ${MONTHS[currentDay.getMonth()]} ${currentDay.getFullYear()}`;
  }
  document.getElementById("toolbar-title").textContent = label;
}

function updateViewButtons() {
  ["month","week","day"].forEach(v => {
    document.getElementById("sb-"+v).classList.toggle("active", view===v);
    document.getElementById("vg-"+v).classList.toggle("active", view===v);
  });
}

function renderMiniCal() {
  document.getElementById("mini-label").textContent = MONTHS[currentMonth].slice(0,3)+" "+currentYear;
  const firstDay = new Date(currentYear,currentMonth,1).getDay();
  const offset = (firstDay+6)%7;
  const daysInMonth = new Date(currentYear,currentMonth+1,0).getDate();
  const total = Math.ceil((offset+daysInMonth)/7)*7;
  let html = "";
  for(let i=0;i<total;i++){
    const dn = i-offset+1;
    if(dn<1||dn>daysInMonth){ html+=`<div></div>`; continue; }
    const d = new Date(currentYear,currentMonth,dn);
    const isToday = sameDay(d,today);
    const isSel = sameDay(d,selectedDay);
    const hasBk = bookingsForDay(d).length>0;
    let cls = "mini-day";
    if(isSel) cls+=" selected"; else if(isToday) cls+=" today";
    html+=`<div class="${cls}" onclick="miniDayClick(${currentYear},${currentMonth},${dn})">${dn}${hasBk?'<div class="mini-dot"></div>':''}</div>`;
  }
  document.getElementById("mini-days").innerHTML = html;
}

function miniDayClick(y,m,d) {
  selectedDay = new Date(y,m,d);
  currentDay = new Date(y,m,d);
  currentYear = y; currentMonth = m;
  setView("day");
}

function renderView() {
  const area = document.getElementById("cal-area");
  if(view==="month") area.innerHTML = renderMonth();
  else if(view==="week") area.innerHTML = renderWeek();
  else area.innerHTML = renderDay();
  attachEvents();
}

function renderMonth() {
  const firstDay = new Date(currentYear,currentMonth,1).getDay();
  const offset = (firstDay+6)%7;
  const daysInMonth = new Date(currentYear,currentMonth+1,0).getDate();
  const total = Math.ceil((offset+daysInMonth)/7)*7;
  let html = `<div class="month-grid">`;
  DAYS_SHORT.forEach(d=>{ html+=`<div class="month-dow">${d}</div>`; });
  for(let i=0;i<total;i++){
    const dn=i-offset+1;
    const isCurrentMonth = dn>=1&&dn<=daysInMonth;
    const d = isCurrentMonth ? new Date(currentYear,currentMonth,dn) : null;
    const isToday = d&&sameDay(d,today);
    const dayBks = d ? bookingsForDay(d) : [];
    html+=`<div class="month-cell${isCurrentMonth?'':' other-month'}" ${d?`onclick="monthDayClick(${d.getFullYear()},${d.getMonth()},${d.getDate()})"`:''}">`;
    if(d){
      html+=`<div class="day-num${isToday?' today':''}">${d.getDate()}</div>`;
      dayBks.slice(0,3).forEach(b=>{
        html+=`<div class="booking-pill" style="background:${b.color}" onclick="event.stopPropagation();editBooking(${b.id})">${b.title}</div>`;
      });
      if(dayBks.length>3) html+=`<div class="more-label">+${dayBks.length-3} mehr</div>`;
    }
    html+=`</div>`;
  }
  html+=`</div>`;
  return html;
}

function renderWeek() {
  const days=[];
  for(let i=0;i<7;i++){ const d=new Date(weekStart); d.setDate(d.getDate()+i); days.push(d); }
  let html=`<div style="display:flex;flex-direction:column;height:100%;">`;
  // header
  html+=`<div class="week-header"><div class="week-header-empty"></div>`;
  days.forEach((d,i)=>{
    const isToday=sameDay(d,today);
    html+=`<div class="week-header-day" onclick="weekDayClick(${d.getFullYear()},${d.getMonth()},${d.getDate()})">
      <div class="week-dow">${DAYS_SHORT[i]}</div>
      <div class="week-daynum${isToday?' today':''}">${d.getDate()}</div>
    </div>`;
  });
  html+=`</div>`;
  // grid
  html+=`<div style="flex:1;overflow:auto;position:relative;">`;
  html+=`<div class="time-grid" style="min-height:${HOURS.length*60}px;">`;
  HOURS.forEach(h=>{
    html+=`<div class="time-row">`;
    html+=`<div class="time-label">${fmt2(h)}:00</div>`;
    days.forEach((d,di)=>{
      const isToday=sameDay(d,today);
      html+=`<div class="time-cell${isToday?' today-col':''}" onclick="openNewDay(${d.getFullYear()},${d.getMonth()},${d.getDate()})"></div>`;
    });
    html+=`</div>`;
  });
  // bookings overlay
  const colW = 100/7;
  days.forEach((d,di)=>{
    bookingsForDay(d).forEach(b=>{
      const dayStart=new Date(d); dayStart.setHours(6,0,0,0);
      const s=b.start>dayStart?b.start:dayStart;
      const top=((s.getHours()-6)*60+s.getMinutes())/60*60;
      const dur=(b.end-s)/3600000*60;
      const height=Math.max(22,dur);
      html+=`<div class="week-booking" style="top:${top}px;height:${height}px;left:calc(54px + ${di}*(100% - 54px)/7 + 2px);width:calc((100% - 54px)/7 - 4px);background:${b.color}" onclick="event.stopPropagation();editBooking(${b.id})">
        <div class="week-booking-title">${b.title}</div>
        <div style="opacity:.85;font-size:10px;">${b.person}</div>
      </div>`;
    });
  });
  html+=`</div></div></div>`;
  return html;
}

function renderDay() {
  const isToday = sameDay(currentDay,today);
  const dayBks = bookingsForDay(currentDay);
  const dow = DAYS_SHORT[(currentDay.getDay()+6)%7];
  let html=`<div style="display:flex;flex-direction:column;height:100%;">`;
  html+=`<div class="day-header">
    <div class="day-circle${isToday?' today':''}">${currentDay.getDate()}</div>
    <div><div class="day-dow">${dow}</div><div class="day-month">${MONTHS[currentDay.getMonth()]} ${currentDay.getFullYear()}</div></div>
    <button class="btn-add" style="margin-left:auto" onclick="openNewDay(${currentDay.getFullYear()},${currentDay.getMonth()},${currentDay.getDate()})">+ Buchung</button>
  </div>`;
  html+=`<div style="flex:1;overflow:auto;position:relative;">`;
  html+=`<div class="day-grid" style="min-height:${HOURS.length*64}px;">`;
  HOURS.forEach(h=>{
    html+=`<div class="day-row"><div class="day-label">${fmt2(h)}:00</div><div class="day-cell" onclick="openNewDay(${currentDay.getFullYear()},${currentDay.getMonth()},${currentDay.getDate()})"></div></div>`;
  });
  dayBks.forEach(b=>{
    const dayStart=new Date(currentDay); dayStart.setHours(6,0,0,0);
    const s=b.start>dayStart?b.start:dayStart;
    const top=((s.getHours()-6)*60+s.getMinutes())/64*64;
    const dur=(b.end-s)/3600000*64;
    const height=Math.max(32,dur);
    html+=`<div class="day-booking" style="top:${top}px;height:${height}px;left:64px;right:16px;background:${b.color}" onclick="editBooking(${b.id})">
      <div class="day-booking-title">${b.title}</div>
      <div class="day-booking-sub">${b.person}${b.purpose?' · '+b.purpose:''}</div>
      ${b.km?`<div class="day-booking-km">${b.km} km geplant</div>`:''}
    </div>`;
  });
  html+=`</div></div></div>`;
  return html;
}

function attachEvents(){}

// ---- NAVIGATION ----
function navPrev(){
  if(view==="month"){ if(currentMonth===0){currentMonth=11;currentYear--;} else currentMonth--; }
  else if(view==="week"){ weekStart.setDate(weekStart.getDate()-7); }
  else { currentDay.setDate(currentDay.getDate()-1); }
  render();
}
function navNext(){
  if(view==="month"){ if(currentMonth===11){currentMonth=0;currentYear++;} else currentMonth++; }
  else if(view==="week"){ weekStart.setDate(weekStart.getDate()+7); }
  else { currentDay.setDate(currentDay.getDate()+1); }
  render();
}
function goToday(){
  currentYear=today.getFullYear(); currentMonth=today.getMonth();
  currentDay=new Date(today); weekStart=getWeekStart(today);
  render();
}
function miniPrev(){ if(currentMonth===0){currentMonth=11;currentYear--;} else currentMonth--; render(); }
function miniNext(){ if(currentMonth===11){currentMonth=0;currentYear++;} else currentMonth++; render(); }
function setView(v){ view=v; render(); }
function monthDayClick(y,m,d){ currentDay=new Date(y,m,d); currentYear=y; currentMonth=m; selectedDay=new Date(y,m,d); setView("day"); }
function weekDayClick(y,m,d){ currentDay=new Date(y,m,d); setView("day"); }
function openNewDay(y,m,d){ openModal(new Date(y,m,d)); }

// ---- MODAL ----
function buildColorPicker(){
  const cp = document.getElementById("color-picker");
  cp.innerHTML = COLORS.map(c=>`<div class="color-dot${c===selectedColor?' selected':''}" style="background:${c}" onclick="pickColor('${c}')"></div>`).join("");
}
function pickColor(c){ selectedColor=c; buildColorPicker(); }

function openModal(defaultDate){
  editId=null;
  document.getElementById("modal-title").textContent="Neue Buchung";
  document.getElementById("f-title").value="";
  document.getElementById("f-person").value="";
  const d = defaultDate||selectedDay||today;
  document.getElementById("f-date").value=fmtDate(d);
  document.getElementById("f-start").value="08:00";
  document.getElementById("f-end").value="17:00";
  document.getElementById("f-purpose").value="";
  document.getElementById("f-km").value="";
  document.getElementById("f-notes").value="";
  selectedColor="#0078d4";
  buildColorPicker();
  document.getElementById("btn-delete").classList.add("hidden");
  document.querySelector(".btn-save").textContent="Buchung erstellen";
  document.getElementById("conflict-box").classList.add("hidden");
  document.getElementById("modal-overlay").classList.remove("hidden");
  checkConflict();
}
function editBooking(id){
  const b = bookings.find(x=>x.id===id);
  if(!b) return;
  editId=id;
  document.getElementById("modal-title").textContent="Buchung bearbeiten";
  document.getElementById("f-title").value=b.title;
  document.getElementById("f-person").value=b.person;
  document.getElementById("f-date").value=fmtDate(b.start);
  document.getElementById("f-start").value=`${fmt2(b.start.getHours())}:${fmt2(b.start.getMinutes())}`;
  document.getElementById("f-end").value=`${fmt2(b.end.getHours())}:${fmt2(b.end.getMinutes())}`;
  document.getElementById("f-purpose").value=b.purpose||"";
  document.getElementById("f-km").value=b.km||"";
  document.getElementById("f-notes").value=b.notes||"";
  selectedColor=b.color;
  buildColorPicker();
  document.getElementById("btn-delete").classList.remove("hidden");
  document.querySelector(".btn-save").textContent="Speichern";
  document.getElementById("conflict-box").classList.add("hidden");
  document.getElementById("modal-overlay").classList.remove("hidden");
  checkConflict();
}
function closeModal(){ document.getElementById("modal-overlay").classList.add("hidden"); }

function checkConflict(){
  const dateVal = document.getElementById("f-date").value;
  if(!dateVal){ document.getElementById("conflict-box").classList.add("hidden"); return; }
  const [y,m,d]=dateVal.split("-").map(Number);
  const checkDay=new Date(y,m-1,d);
  const conflicts=bookingsForDay(checkDay).filter(b=>editId?b.id!==editId:true);
  const box=document.getElementById("conflict-box");
  if(conflicts.length>0){
    box.textContent=`⚠️ Achtung: Das Fahrzeug ist an diesem Tag bereits gebucht: ${conflicts.map(b=>b.title).join(", ")}`;
    box.classList.remove("hidden");
  } else { box.classList.add("hidden"); }
}

function saveBooking(){
  const title=document.getElementById("f-title").value.trim();
  const person=document.getElementById("f-person").value.trim();
  const dateVal=document.getElementById("f-date").value;
  if(!title||!person||!dateVal){ alert("Bitte Titel, Person und Datum ausfüllen."); return; }
  const [y,m,d]=dateVal.split("-").map(Number);
  const [sh,sm]=document.getElementById("f-start").value.split(":").map(Number);
  const [eh,em]=document.getElementById("f-end").value.split(":").map(Number);
  const start=new Date(y,m-1,d,sh,sm);
  const end=new Date(y,m-1,d,eh,em);
  const km=Number(document.getElementById("f-km").value)||0;
  const purpose=document.getElementById("f-purpose").value;
  const notes=document.getElementById("f-notes").value;
  if(editId){
    bookings=bookings.map(b=>b.id===editId?{...b,title,person,start,end,purpose,km,color:selectedColor,notes}:b);
  } else {
    bookings.push({id:Date.now(),title,person,start,end,purpose,km,color:selectedColor,notes});
  }
  closeModal();
  render();
}

function confirmDelete(){ document.getElementById("confirm-overlay").classList.remove("hidden"); }
function doDelete(){
  bookings=bookings.filter(b=>b.id!==editId);
  document.getElementById("confirm-overlay").classList.add("hidden");
  closeModal();
  render();
}

// ---- INIT ----
render();
</script>
</body>
</html>
