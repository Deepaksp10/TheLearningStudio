# TheLearningStudio
Track attendance of Learning Studio
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Fee & Attendance Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap" rel="stylesheet" />
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #F7F6F3;
    --surface: #FFFFFF;
    --surface-muted: #F0EEE9;
    --border: #E2DDD5;
    --border-strong: #C8C2B8;
    --text: #1A1917;
    --text-muted: #6B675F;
    --text-hint: #9B978F;
    --teal-bg: #E1F5EE;
    --teal-text: #0F6E56;
    --teal-border: #1D9E75;
    --coral-bg: #FAECE7;
    --coral-text: #993C1D;
    --coral-border: #D85A30;
    --amber-bg: #FAEEDA;
    --amber-text: #854F0B;
    --amber-border: #BA7517;
    --blue-bg: #E6F1FB;
    --blue-text: #185FA5;
    --accent: #1D9E75;
    --danger: #D85A30;
    --radius: 8px;
    --radius-lg: 12px;
  }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 0;
  }

  /* Header */
  .header {
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    padding: 1rem 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .header-title {
    font-size: 17px;
    font-weight: 600;
    letter-spacing: -0.01em;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .header-title .dot {
    width: 10px; height: 10px;
    border-radius: 50%;
    background: var(--accent);
    display: inline-block;
  }
  .header-meta {
    font-size: 13px;
    color: var(--text-muted);
  }

  /* Main layout */
  .main { max-width: 1100px; margin: 0 auto; padding: 2rem 1.5rem; }

  /* Stats row */
  .stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 2rem; }
  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1rem 1.25rem;
  }
  .stat-label { font-size: 12px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 6px; }
  .stat-value { font-size: 26px; font-weight: 600; line-height: 1; }
  .stat-sub { font-size: 12px; color: var(--text-hint); margin-top: 4px; }
  .stat-card.paid .stat-value { color: var(--teal-text); }
  .stat-card.pending .stat-value { color: var(--amber-text); }
  .stat-card.present .stat-value { color: var(--blue-text); }
  .stat-card.absent .stat-value { color: var(--coral-text); }

  /* Form panel */
  .panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-lg);
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }
  .panel-title {
    font-size: 14px;
    font-weight: 600;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.06em;
    margin-bottom: 1rem;
  }

  .form-row { display: grid; grid-template-columns: 1fr 1fr 1fr 1fr 1fr auto; gap: 10px; align-items: end; }
  .field label { font-size: 12px; color: var(--text-muted); display: block; margin-bottom: 4px; font-weight: 500; }
  .field input, .field select {
    width: 100%;
    padding: 8px 10px;
    border: 1px solid var(--border-strong);
    border-radius: var(--radius);
    font-size: 14px;
    font-family: inherit;
    background: var(--surface);
    color: var(--text);
    outline: none;
    transition: border-color 0.15s;
  }
  .field input:focus, .field select:focus { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(29,158,117,0.12); }

  .btn-add {
    padding: 9px 18px;
    background: var(--accent);
    color: white;
    border: none;
    border-radius: var(--radius);
    font-size: 14px;
    font-weight: 500;
    font-family: inherit;
    cursor: pointer;
    white-space: nowrap;
    display: flex;
    align-items: center;
    gap: 6px;
    transition: opacity 0.15s;
  }
  .btn-add:hover { opacity: 0.88; }
  .btn-add:active { transform: scale(0.98); }

  /* Filters */
  .filters { display: flex; gap: 10px; align-items: center; flex-wrap: wrap; margin-bottom: 1rem; }
  .filter-input {
    padding: 7px 12px;
    border: 1px solid var(--border-strong);
    border-radius: var(--radius);
    font-size: 13px;
    font-family: inherit;
    background: var(--surface);
    color: var(--text);
    outline: none;
    min-width: 180px;
  }
  .filter-input:focus { border-color: var(--accent); }
  .filter-select {
    padding: 7px 10px;
    border: 1px solid var(--border-strong);
    border-radius: var(--radius);
    font-size: 13px;
    font-family: inherit;
    background: var(--surface);
    color: var(--text);
    outline: none;
  }
  .filter-count { font-size: 13px; color: var(--text-hint); margin-left: auto; }

  /* Table */
  .table-wrap { overflow-x: auto; }
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
  }
  thead th {
    background: var(--surface-muted);
    padding: 10px 14px;
    text-align: left;
    font-size: 12px;
    font-weight: 600;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.05em;
    border-bottom: 1px solid var(--border);
    white-space: nowrap;
    cursor: pointer;
    user-select: none;
  }
  thead th:hover { color: var(--text); }
  thead th .sort-icon { opacity: 0.4; font-size: 10px; margin-left: 4px; }
  thead th.active .sort-icon { opacity: 1; color: var(--accent); }

  tbody tr {
    border-bottom: 1px solid var(--border);
    transition: background 0.1s;
  }
  tbody tr:last-child { border-bottom: none; }
  tbody tr:hover { background: var(--surface-muted); }

  td { padding: 12px 14px; vertical-align: middle; }

  .name-cell { font-weight: 500; }
  .name-avatar {
    display: inline-flex; align-items: center; justify-content: center;
    width: 30px; height: 30px; border-radius: 50%;
    background: var(--blue-bg); color: var(--blue-text);
    font-size: 11px; font-weight: 600;
    margin-right: 8px; vertical-align: middle;
    flex-shrink: 0;
  }

  /* Badges */
  .badge {
    display: inline-flex; align-items: center; gap: 4px;
    padding: 3px 10px; border-radius: 99px;
    font-size: 12px; font-weight: 500;
  }
  .badge-paid { background: var(--teal-bg); color: var(--teal-text); }
  .badge-pending { background: var(--amber-bg); color: var(--amber-text); }
  .badge-overdue { background: var(--coral-bg); color: var(--coral-text); }
  .badge-present { background: var(--teal-bg); color: var(--teal-text); }
  .badge-absent { background: var(--coral-bg); color: var(--coral-text); }
  .badge-late { background: var(--amber-bg); color: var(--amber-text); }
  .badge-leave { background: var(--blue-bg); color: var(--blue-text); }
  .badge-guru-yes { background: #EAF3DE; color: #27500A; }
  .badge-guru-no { background: var(--surface-muted); color: var(--text-muted); }

  /* Inline edit selects */
  .inline-select {
    padding: 4px 8px;
    border: 1px solid var(--border);
    border-radius: 6px;
    font-size: 12px;
    font-family: inherit;
    background: var(--surface);
    color: var(--text);
    outline: none;
    cursor: pointer;
  }
  .inline-select:focus { border-color: var(--accent); }

  .date-cell { color: var(--text-muted); font-size: 13px; white-space: nowrap; }

  .btn-delete {
    background: none;
    border: none;
    cursor: pointer;
    color: var(--text-hint);
    font-size: 16px;
    padding: 4px 6px;
    border-radius: 6px;
    transition: color 0.15s, background 0.15s;
  }
  .btn-delete:hover { color: var(--danger); background: var(--coral-bg); }

  .empty-state {
    text-align: center;
    padding: 3rem 1rem;
    color: var(--text-hint);
    font-size: 14px;
  }
  .empty-state .icon { font-size: 40px; margin-bottom: 0.75rem; opacity: 0.3; }

  /* Toast */
  .toast {
    position: fixed;
    bottom: 1.5rem;
    right: 1.5rem;
    background: #1A1917;
    color: white;
    padding: 10px 16px;
    border-radius: var(--radius);
    font-size: 13px;
    opacity: 0;
    transform: translateY(8px);
    transition: opacity 0.2s, transform 0.2s;
    pointer-events: none;
    z-index: 999;
  }
  .toast.show { opacity: 1; transform: translateY(0); }

  /* Export btn */
  .btn-export {
    padding: 7px 14px;
    background: var(--surface);
    color: var(--text-muted);
    border: 1px solid var(--border-strong);
    border-radius: var(--radius);
    font-size: 13px;
    font-weight: 500;
    font-family: inherit;
    cursor: pointer;
    display: flex; align-items: center; gap: 6px;
    transition: background 0.15s;
  }
  .btn-export:hover { background: var(--surface-muted); color: var(--text); }

  @media (max-width: 700px) {
    .stats { grid-template-columns: repeat(2, 1fr); }
    .form-row { grid-template-columns: 1fr 1fr; }
    .form-row .btn-wrap { grid-column: span 2; }
    .header { padding: 1rem; }
    .main { padding: 1rem; }
  }
</style>
</head>
<body>

<header class="header">
  <div class="header-title">
    <span class="dot"></span>
    Fee &amp; Attendance Tracker
  </div>
  <div class="header-meta" id="today-date"></div>
</header>

<main class="main">

  <!-- Stats -->
  <div class="stats">
    <div class="stat-card paid">
      <div class="stat-label">Fee Paid</div>
      <div class="stat-value" id="stat-paid">0</div>
      <div class="stat-sub">students</div>
    </div>
    <div class="stat-card pending">
      <div class="stat-label">Fee Pending</div>
      <div class="stat-value" id="stat-pending">0</div>
      <div class="stat-sub">students</div>
    </div>
    <div class="stat-card present">
      <div class="stat-label">Present Today</div>
      <div class="stat-value" id="stat-present">0</div>
      <div class="stat-sub">students</div>
    </div>
    <div class="stat-card absent">
      <div class="stat-label">Absent Today</div>
      <div class="stat-value" id="stat-absent">0</div>
      <div class="stat-sub">students</div>
    </div>
  </div>

  <!-- Add Entry -->
  <div class="panel">
    <div class="panel-title">Add Entry</div>
    <div class="form-row">
      <div class="field">
        <label for="inp-name">Name</label>
        <input type="text" id="inp-name" placeholder="Full name" />
      </div>
      <div class="field">
        <label for="inp-date">Date</label>
        <input type="date" id="inp-date" />
      </div>
      <div class="field">
        <label for="inp-fee">Fee Status</label>
        <select id="inp-fee">
          <option value="paid">Paid</option>
          <option value="pending">Pending</option>
          <option value="overdue">Overdue</option>
        </select>
      </div>
      <div class="field">
        <label for="inp-att">Attendance</label>
        <select id="inp-att">
          <option value="present">Present</option>
          <option value="absent">Absent</option>
          <option value="late">Late</option>
          <option value="leave">On Leave</option>
        </select>
      </div>
      <div class="field">
        <label for="inp-guru">Guru Present</label>
        <select id="inp-guru">
          <option value="yes">Yes</option>
          <option value="no">No</option>
        </select>
      </div>
      <div class="btn-wrap">
        <button class="btn-add" onclick="addEntry()">
          ＋ Add
        </button>
      </div>
    </div>
  </div>

  <!-- Table -->
  <div class="panel" style="padding: 1.25rem 1.5rem;">
    <div style="display: flex; align-items: center; justify-content: space-between; margin-bottom: 1rem; flex-wrap: wrap; gap: 10px;">
      <div class="panel-title" style="margin-bottom: 0;">Records</div>
      <button class="btn-export" onclick="exportCSV()">↓ Export CSV</button>
    </div>

    <div class="filters">
      <input class="filter-input" type="text" id="search" placeholder="Search by name…" oninput="render()" />
      <select class="filter-select" id="filter-fee" onchange="render()">
        <option value="">All fee status</option>
        <option value="paid">Paid</option>
        <option value="pending">Pending</option>
        <option value="overdue">Overdue</option>
      </select>
      <select class="filter-select" id="filter-att" onchange="render()">
        <option value="">All attendance</option>
        <option value="present">Present</option>
        <option value="absent">Absent</option>
        <option value="late">Late</option>
        <option value="leave">On Leave</option>
      </select>
      <input class="filter-select" type="date" id="filter-date" onchange="render()" title="Filter by date" style="font-family: inherit; font-size: 13px;" />
      <select class="filter-select" id="filter-guru" onchange="render()">
        <option value="">Guru — all</option>
        <option value="yes">Guru present</option>
        <option value="no">Guru absent</option>
      </select>
      <span class="filter-count" id="filter-count"></span>
    </div>

    <div class="table-wrap">
      <table>
        <thead>
          <tr>
            <th onclick="sortBy('name')">Name <span class="sort-icon" id="sort-name">↕</span></th>
            <th onclick="sortBy('date')">Date <span class="sort-icon" id="sort-date">↕</span></th>
            <th onclick="sortBy('fee')">Fee Status <span class="sort-icon" id="sort-fee">↕</span></th>
            <th onclick="sortBy('attendance')">Attendance <span class="sort-icon" id="sort-attendance">↕</span></th>
            <th onclick="sortBy('guru')">Guru Present <span class="sort-icon" id="sort-guru">↕</span></th>
            <th></th>
          </tr>
        </thead>
        <tbody id="table-body"></tbody>
      </table>
      <div class="empty-state" id="empty-state" style="display:none;">
        <div class="icon">📋</div>
        No records found. Add an entry above to get started.
      </div>
    </div>
  </div>

</main>

<div class="toast" id="toast"></div>

<script>
let records = JSON.parse(localStorage.getItem('fat-records') || '[]');
let sortKey = 'date';
let sortDir = 'desc';

const today = new Date().toISOString().split('T')[0];
document.getElementById('inp-date').value = today;
document.getElementById('today-date').textContent = new Date().toLocaleDateString('en-IN', { weekday:'long', year:'numeric', month:'long', day:'numeric' });

function save() { localStorage.setItem('fat-records', JSON.stringify(records)); }

function initials(name) {
  return name.split(' ').slice(0,2).map(w => w[0]?.toUpperCase() || '').join('');
}

const FEE_BADGE = {
  paid: 'badge-paid',
  pending: 'badge-pending',
  overdue: 'badge-overdue'
};
const ATT_BADGE = {
  present: 'badge-present',
  absent: 'badge-absent',
  late: 'badge-late',
  leave: 'badge-leave'
};
const FEE_LABEL = { paid: 'Paid', pending: 'Pending', overdue: 'Overdue' };
const ATT_LABEL = { present: 'Present', absent: 'Absent', late: 'Late', leave: 'On Leave' };

function addEntry() {
  const name = document.getElementById('inp-name').value.trim();
  const date = document.getElementById('inp-date').value;
  const fee = document.getElementById('inp-fee').value;
  const att = document.getElementById('inp-att').value;
  const guru = document.getElementById('inp-guru').value;

  if (!name) { showToast('Please enter a name.'); document.getElementById('inp-name').focus(); return; }
  if (!date) { showToast('Please pick a date.'); return; }

  records.unshift({ id: Date.now(), name, date, fee, attendance: att, guru });
  save();
  render();
  document.getElementById('inp-name').value = '';
  document.getElementById('inp-name').focus();
  showToast('Entry added.');
}

function deleteEntry(id) {
  records = records.filter(r => r.id !== id);
  save();
  render();
  showToast('Entry removed.');
}

function updateField(id, field, value) {
  const r = records.find(r => r.id === id);
  if (r) { r[field] = value; save(); updateStats(); }
}

function sortBy(key) {
  if (sortKey === key) sortDir = sortDir === 'asc' ? 'desc' : 'asc';
  else { sortKey = key; sortDir = 'asc'; }
  render();
}

function getFiltered() {
  const q = document.getElementById('search').value.toLowerCase();
  const ff = document.getElementById('filter-fee').value;
  const fa = document.getElementById('filter-att').value;
  const fd = document.getElementById('filter-date').value;
  const fg = document.getElementById('filter-guru').value;

  return records.filter(r => {
    if (q && !r.name.toLowerCase().includes(q)) return false;
    if (ff && r.fee !== ff) return false;
    if (fa && r.attendance !== fa) return false;
    if (fd && r.date !== fd) return false;
    if (fg && (r.guru || 'yes') !== fg) return false;
    return true;
  }).sort((a, b) => {
    let av = a[sortKey] || '', bv = b[sortKey] || '';
    if (av < bv) return sortDir === 'asc' ? -1 : 1;
    if (av > bv) return sortDir === 'asc' ? 1 : -1;
    return 0;
  });
}

function updateStats() {
  const todayRecs = records.filter(r => r.date === today);
  document.getElementById('stat-paid').textContent = records.filter(r => r.fee === 'paid').length;
  document.getElementById('stat-pending').textContent = records.filter(r => r.fee === 'pending' || r.fee === 'overdue').length;
  document.getElementById('stat-present').textContent = todayRecs.filter(r => r.attendance === 'present' || r.attendance === 'late').length;
  document.getElementById('stat-absent').textContent = todayRecs.filter(r => r.attendance === 'absent').length;
}

function render() {
  const data = getFiltered();
  const tbody = document.getElementById('table-body');

  ['name','date','fee','attendance','guru'].forEach(k => {
    const el = document.getElementById('sort-' + k);
    if (el) {
      el.textContent = sortKey === k ? (sortDir === 'asc' ? '↑' : '↓') : '↕';
      el.closest('th').classList.toggle('active', sortKey === k);
    }
  });

  document.getElementById('filter-count').textContent = data.length + ' record' + (data.length !== 1 ? 's' : '');
  document.getElementById('empty-state').style.display = data.length === 0 ? '' : 'none';

  tbody.innerHTML = data.map(r => `
    <tr>
      <td class="name-cell">
        <span class="name-avatar">${initials(r.name)}</span>${r.name}
      </td>
      <td class="date-cell">${formatDate(r.date)}</td>
      <td>
        <span class="badge ${FEE_BADGE[r.fee]}">${FEE_LABEL[r.fee]}</span>
        <select class="inline-select" style="margin-left:6px;" onchange="updateField(${r.id},'fee',this.value); render();">
          <option value="paid" ${r.fee==='paid'?'selected':''}>Paid</option>
          <option value="pending" ${r.fee==='pending'?'selected':''}>Pending</option>
          <option value="overdue" ${r.fee==='overdue'?'selected':''}>Overdue</option>
        </select>
      </td>
      <td>
        <span class="badge ${ATT_BADGE[r.attendance]}">${ATT_LABEL[r.attendance]}</span>
        <select class="inline-select" style="margin-left:6px;" onchange="updateField(${r.id},'attendance',this.value); render();">
          <option value="present" ${r.attendance==='present'?'selected':''}>Present</option>
          <option value="absent" ${r.attendance==='absent'?'selected':''}>Absent</option>
          <option value="late" ${r.attendance==='late'?'selected':''}>Late</option>
          <option value="leave" ${r.attendance==='leave'?'selected':''}>Leave</option>
        </select>
      </td>
      <td>
        ${(r.guru||'yes') === 'yes'
          ? '<span class="badge badge-guru-yes">✓ Present</span>'
          : '<span class="badge badge-guru-no">✗ Absent</span>'}
        <select class="inline-select" style="margin-left:6px;" onchange="updateField(${r.id},'guru',this.value); render();">
          <option value="yes" ${(r.guru||'yes')==='yes'?'selected':''}>Yes</option>
          <option value="no" ${(r.guru||'yes')==='no'?'selected':''}>No</option>
        </select>
      </td>
      <td style="text-align:right;">
        <button class="btn-delete" onclick="deleteEntry(${r.id})" title="Delete">✕</button>
      </td>
    </tr>
  `).join('');

  updateStats();
}

function formatDate(iso) {
  if (!iso) return '—';
  const [y, m, d] = iso.split('-');
  const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  return `${d} ${months[parseInt(m)-1]} ${y}`;
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2200);
}

function exportCSV() {
  const data = getFiltered();
  if (!data.length) { showToast('No records to export.'); return; }
  const header = ['Name', 'Date', 'Fee Status', 'Attendance', 'Guru Present'];
  const rows = data.map(r => [r.name, r.date, FEE_LABEL[r.fee], ATT_LABEL[r.attendance], (r.guru||'yes') === 'yes' ? 'Yes' : 'No']);
  const csv = [header, ...rows].map(r => r.map(c => `"${c}"`).join(',')).join('\n');
  const blob = new Blob([csv], { type: 'text/csv' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'tracker-export.csv';
  a.click();
  showToast('CSV exported.');
}

// Enter key on name input
document.getElementById('inp-name').addEventListener('keydown', e => {
  if (e.key === 'Enter') addEntry();
});

// Seed demo data if empty
if (records.length === 0) {
  const names = ['Aarav Sharma','Priya Nair','Riya Mehta','Arjun Patel','Sneha Rao','Kiran Reddy'];
  const feeOpts = ['paid','paid','pending','paid','overdue','pending'];
  const attOpts = ['present','present','absent','late','present','leave'];
  const guruOpts = ['yes','yes','no','yes','yes','no'];
  names.forEach((n, i) => {
    const d = new Date(); d.setDate(d.getDate() - i % 3);
    records.push({ id: Date.now() + i, name: n, date: d.toISOString().split('T')[0], fee: feeOpts[i], attendance: attOpts[i], guru: guruOpts[i] });
  });
  save();
}

render();
</script>
</body>
</html>
