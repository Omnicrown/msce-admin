# msce-admin
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>MSCE Admin Panel</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
<style>
  :root{
    --bg:#0a0e1a;--surface:#111827;--card:#161d2e;--border:#1e2a40;
    --accent:#f5a623;--accent2:#3ecf8e;--danger:#ff4d6d;--blue:#4a9eff;
    --text:#e8ecf4;--muted:#6b7a99;--font-head:'Syne',sans-serif;--font-body:'DM Sans',sans-serif;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  body{background:var(--bg);color:var(--text);font-family:var(--font-body);min-height:100vh;}

  /* Login */
  #login-screen{display:flex;align-items:center;justify-content:center;min-height:100vh;
    background:radial-gradient(ellipse at 60% 40%,#1a2540 0%,#0a0e1a 70%);}
  .login-box{background:var(--card);border:1px solid var(--border);border-radius:20px;
    padding:48px 40px;width:380px;text-align:center;}
  .login-box h1{font-family:var(--font-head);font-size:28px;font-weight:800;
    background:linear-gradient(135deg,var(--accent),#ff8c42);
    -webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:4px;}
  .login-box p{color:var(--muted);font-size:13px;margin-bottom:32px;}
  .field{width:100%;background:var(--surface);border:1px solid var(--border);
    border-radius:10px;padding:12px 16px;color:var(--text);font-size:14px;
    font-family:var(--font-body);margin-bottom:14px;outline:none;transition:.2s;}
  .field:focus{border-color:var(--accent);}
  .btn{width:100%;padding:13px;border-radius:10px;border:none;cursor:pointer;
    font-family:var(--font-head);font-weight:700;font-size:14px;letter-spacing:.5px;transition:.2s;}
  .btn-primary{background:linear-gradient(135deg,var(--accent),#e8920d);color:#0a0e1a;}
  .btn-primary:hover{opacity:.9;transform:translateY(-1px);}
  .btn-danger{background:var(--danger);color:#fff;padding:8px 14px;width:auto;font-size:12px;}
  .btn-green{background:var(--accent2);color:#0a0e1a;padding:9px 18px;width:auto;}
  .btn-blue{background:var(--blue);color:#fff;padding:9px 18px;width:auto;}

  /* App shell */
  #app{display:none;}
  .topbar{background:var(--surface);border-bottom:1px solid var(--border);
    padding:14px 28px;display:flex;align-items:center;justify-content:space-between;}
  .topbar h2{font-family:var(--font-head);font-size:18px;font-weight:800;
    color:var(--accent);}
  .topbar span{font-size:13px;color:var(--muted);}
  .logout-btn{background:none;border:1px solid var(--border);color:var(--muted);
    padding:6px 14px;border-radius:8px;cursor:pointer;font-size:13px;transition:.2s;}
  .logout-btn:hover{border-color:var(--danger);color:var(--danger);}

  .layout{display:grid;grid-template-columns:220px 1fr;min-height:calc(100vh - 57px);}
  .sidebar{background:var(--surface);border-right:1px solid var(--border);padding:24px 0;}
  .nav-item{padding:12px 24px;cursor:pointer;font-size:14px;color:var(--muted);
    transition:.2s;border-left:3px solid transparent;display:flex;align-items:center;gap:10px;}
  .nav-item:hover{color:var(--text);background:rgba(255,255,255,.03);}
  .nav-item.active{color:var(--accent);border-left-color:var(--accent);background:rgba(245,166,35,.06);}
  .nav-icon{font-size:16px;}

  .main{padding:28px;overflow-y:auto;}
  .page{display:none;}
  .page.active{display:block;}

  /* Stats cards */
  .stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:16px;margin-bottom:28px;}
  .stat-card{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:20px;
    position:relative;overflow:hidden;}
  .stat-card::before{content:'';position:absolute;top:-20px;right:-20px;width:80px;height:80px;
    border-radius:50%;opacity:.1;}
  .stat-card.gold::before{background:var(--accent);}
  .stat-card.green::before{background:var(--accent2);}
  .stat-card.blue::before{background:var(--blue);}
  .stat-card.red::before{background:var(--danger);}
  .stat-num{font-family:var(--font-head);font-size:32px;font-weight:800;margin-bottom:4px;}
  .stat-card.gold .stat-num{color:var(--accent);}
  .stat-card.green .stat-num{color:var(--accent2);}
  .stat-card.blue .stat-num{color:var(--blue);}
  .stat-card.red .stat-num{color:var(--danger);}
  .stat-label{font-size:12px;color:var(--muted);text-transform:uppercase;letter-spacing:.8px;}

  /* Section headers */
  .section-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:18px;}
  .section-head h3{font-family:var(--font-head);font-size:16px;font-weight:700;color:var(--text);}

  /* Upload form */
  .upload-card{background:var(--card);border:1px solid var(--border);border-radius:16px;padding:28px;max-width:600px;}
  .form-row{margin-bottom:16px;}
  .form-row label{display:block;font-size:12px;color:var(--muted);
    text-transform:uppercase;letter-spacing:.8px;margin-bottom:6px;}
  .form-control{width:100%;background:var(--surface);border:1px solid var(--border);
    border-radius:10px;padding:11px 14px;color:var(--text);font-family:var(--font-body);
    font-size:14px;outline:none;transition:.2s;}
  .form-control:focus{border-color:var(--accent);}
  select.form-control option{background:var(--surface);}
  .drop-zone{border:2px dashed var(--border);border-radius:12px;padding:32px;text-align:center;
    cursor:pointer;transition:.2s;background:var(--surface);}
  .drop-zone:hover,.drop-zone.dragover{border-color:var(--accent);background:rgba(245,166,35,.04);}
  .drop-zone p{color:var(--muted);font-size:13px;margin-top:8px;}
  .drop-icon{font-size:32px;}
  .file-name{color:var(--accent2);font-size:13px;margin-top:8px;font-weight:500;}

  /* Papers table */
  .papers-table{width:100%;border-collapse:collapse;}
  .papers-table th{text-align:left;padding:10px 14px;font-size:11px;color:var(--muted);
    text-transform:uppercase;letter-spacing:.8px;border-bottom:1px solid var(--border);}
  .papers-table td{padding:12px 14px;font-size:13px;border-bottom:1px solid rgba(255,255,255,.04);}
  .papers-table tr:hover td{background:rgba(255,255,255,.02);}
  .badge{display:inline-block;padding:3px 10px;border-radius:20px;font-size:11px;font-weight:600;}
  .badge-paper{background:rgba(74,158,255,.15);color:var(--blue);}
  .badge-scheme{background:rgba(62,207,142,.15);color:var(--accent2);}
  .badge-note{background:rgba(245,166,35,.15);color:var(--accent);}
  .table-wrap{background:var(--card);border:1px solid var(--border);border-radius:16px;overflow:hidden;}

  /* Filters */
  .filters{display:flex;gap:10px;margin-bottom:18px;flex-wrap:wrap;}
  .filter-btn{padding:7px 16px;border-radius:20px;border:1px solid var(--border);
    background:transparent;color:var(--muted);font-size:13px;cursor:pointer;transition:.2s;}
  .filter-btn:hover,.filter-btn.active{border-color:var(--accent);color:var(--accent);
    background:rgba(245,166,35,.08);}

  .toast{position:fixed;bottom:24px;right:24px;background:var(--accent2);color:#0a0e1a;
    padding:12px 20px;border-radius:10px;font-weight:600;font-size:13px;
    transform:translateY(80px);opacity:0;transition:.3s;z-index:999;}
  .toast.show{transform:translateY(0);opacity:1;}
  .toast.error{background:var(--danger);color:#fff;}

  .empty{text-align:center;padding:48px;color:var(--muted);font-size:14px;}
  .empty-icon{font-size:40px;margin-bottom:8px;}
</style>
</head>
<body>

<!-- LOGIN -->
<div id="login-screen">
  <div class="login-box">
    <h1>MSCE Portal</h1>
    <p>Admin access only</p>
    <input class="field" id="u" type="text" placeholder="Username" autocomplete="off"/>
    <input class="field" id="p" type="password" placeholder="Password"/>
    <button class="btn btn-primary" onclick="login()">Sign In →</button>
    <p id="login-err" style="color:#ff4d6d;font-size:13px;margin-top:14px;"></p>
  </div>
</div>

<!-- APP -->
<div id="app">
  <div class="topbar">
    <h2>✦ MSCE Admin</h2>
    <div style="display:flex;align-items:center;gap:16px;">
      <span id="topbar-stats"></span>
      <button class="logout-btn" onclick="logout()">Logout</button>
    </div>
  </div>
  <div class="layout">
    <div class="sidebar">
      <div class="nav-item active" onclick="showPage('dashboard',this)">
        <span class="nav-icon">◈</span> Dashboard
      </div>
      <div class="nav-item" onclick="showPage('upload',this)">
        <span class="nav-icon">↑</span> Upload Paper
      </div>
      <div class="nav-item" onclick="showPage('manage',this)">
        <span class="nav-icon">≡</span> Manage Papers
      </div>
      <div class="nav-item" onclick="showPage('subjects',this)">
        <span class="nav-icon">◉</span> Subjects
      </div>
    </div>
    <div class="main">

      <!-- DASHBOARD -->
      <div class="page active" id="page-dashboard">
        <div class="stats-grid">
          <div class="stat-card gold">
            <div class="stat-num" id="s-total">–</div>
            <div class="stat-label">Total Papers</div>
          </div>
          <div class="stat-card green">
            <div class="stat-num" id="s-subjects">–</div>
            <div class="stat-label">Subjects</div>
          </div>
          <div class="stat-card blue">
            <div class="stat-num" id="s-papers">–</div>
            <div class="stat-label">Past Papers</div>
          </div>
          <div class="stat-card red">
            <div class="stat-num" id="s-schemes">–</div>
            <div class="stat-label">Marking Schemes</div>
          </div>
        </div>
        <div class="section-head"><h3>Recent Uploads</h3></div>
        <div class="table-wrap"><table class="papers-table" id="recent-table">
          <thead><tr><th>Title</th><th>Subject</th><th>Year</th><th>Type</th></tr></thead>
          <tbody id="recent-body"></tbody>
        </table></div>
      </div>

      <!-- UPLOAD -->
      <div class="page" id="page-upload">
        <div class="section-head"><h3>Upload New Resource</h3></div>
        <div class="upload-card">
          <div class="form-row">
            <label>Subject</label>
            <select class="form-control" id="up-subject"></select>
          </div>
          <div class="form-row">
            <label>Title</label>
            <input class="form-control" id="up-title" placeholder="e.g. Mathematics Paper 1 2023"/>
          </div>
          <div class="form-row" style="display:grid;grid-template-columns:1fr 1fr;gap:14px;">
            <div>
              <label>Year</label>
              <input class="form-control" id="up-year" type="number" placeholder="2023"/>
            </div>
            <div>
              <label>Type</label>
              <select class="form-control" id="up-type">
                <option value="past_paper">Past Paper</option>
                <option value="marking_scheme">Marking Scheme</option>
                <option value="study_note">Study Note</option>
              </select>
            </div>
          </div>
          <div class="form-row">
            <label>PDF File</label>
            <div class="drop-zone" id="drop-zone" onclick="document.getElementById('file-inp').click()">
              <div class="drop-icon">📄</div>
              <p>Click to choose or drag & drop a PDF</p>
              <div class="file-name" id="file-name-lbl"></div>
            </div>
            <input type="file" id="file-inp" accept=".pdf" style="display:none" onchange="fileChosen(this)"/>
          </div>
          <button class="btn btn-primary" onclick="uploadPaper()" style="width:auto;padding:12px 28px;">
            Upload ↑
          </button>
        </div>
      </div>

      <!-- MANAGE -->
      <div class="page" id="page-manage">
        <div class="section-head">
          <h3>All Papers</h3>
          <div style="display:flex;gap:8px;">
            <input class="form-control" id="search-inp" placeholder="Search…" style="width:200px;" oninput="filterTable()"/>
          </div>
        </div>
        <div class="filters" id="type-filters">
          <button class="filter-btn active" onclick="setTypeFilter('',this)">All</button>
          <button class="filter-btn" onclick="setTypeFilter('past_paper',this)">Past Papers</button>
          <button class="filter-btn" onclick="setTypeFilter('marking_scheme',this)">Marking Schemes</button>
          <button class="filter-btn" onclick="setTypeFilter('study_note',this)">Study Notes</button>
        </div>
        <div class="table-wrap">
          <table class="papers-table">
            <thead><tr><th>Title</th><th>Subject</th><th>Year</th><th>Type</th><th>Size</th><th></th></tr></thead>
            <tbody id="papers-body"></tbody>
          </table>
        </div>
      </div>

      <!-- SUBJECTS -->
      <div class="page" id="page-subjects">
        <div class="section-head">
          <h3>Subjects</h3>
          <button class="btn btn-green" onclick="addSubjectPrompt()">+ Add Subject</button>
        </div>
        <div class="table-wrap">
          <table class="papers-table">
            <thead><tr><th>Subject Name</th><th>Code</th><th>Papers</th></tr></thead>
            <tbody id="subjects-body"></tbody>
          </table>
        </div>
      </div>

    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const API = 'http://msce-backend-production.up.railway.app';
let authHeader = '';
let allPapers = [];
let activeTypeFilter = '';

// ── Auth ───────────────────────────────────────────────────────
async function login(){
  const u = document.getElementById('u').value;
  const p = document.getElementById('p').value;
  const res = await fetch(`${API}/admin/login`,{
    method:'POST', headers:{'Content-Type':'application/json'},
    body: JSON.stringify({username:u,password:p})
  });
  const d = await res.json();
  if(d.success){
    authHeader = 'Basic ' + btoa(u+':'+p);
    document.getElementById('login-screen').style.display='none';
    document.getElementById('app').style.display='block';
    loadAll();
  } else {
    document.getElementById('login-err').textContent = d.message||'Login failed';
  }
}
function logout(){
  authHeader='';
  document.getElementById('login-screen').style.display='flex';
  document.getElementById('app').style.display='none';
}

// ── Navigation ─────────────────────────────────────────────────
function showPage(name, el){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  document.getElementById('page-'+name).classList.add('active');
  el.classList.add('active');
  if(name==='manage') loadPapers();
  if(name==='subjects') loadSubjects();
  if(name==='upload') loadSubjectSelect();
}

// ── Load all ───────────────────────────────────────────────────
async function loadAll(){
  await loadStats();
  await loadRecentPapers();
  await loadSubjectSelect();
}

async function loadStats(){
  const res = await fetch(`${API}/admin/stats`,{headers:{Authorization:authHeader}});
  const d = await res.json();
  document.getElementById('s-total').textContent   = d.total_papers;
  document.getElementById('s-subjects').textContent= d.total_subjects;
  document.getElementById('s-papers').textContent  = d.by_type?.past_paper||0;
  document.getElementById('s-schemes').textContent = d.by_type?.marking_scheme||0;
  document.getElementById('topbar-stats').textContent = `${d.total_papers} resources · ${d.total_subjects} subjects`;
}

async function loadRecentPapers(){
  const res = await fetch(`${API}/papers`);
  const papers = await res.json();
  allPapers = papers;
  const tbody = document.getElementById('recent-body');
  tbody.innerHTML = papers.slice(0,8).map(p=>`
    <tr>
      <td>${p.title}</td>
      <td>${p.subject_name}</td>
      <td>${p.year||'–'}</td>
      <td>${typeBadge(p.paper_type)}</td>
    </tr>`).join('');
}

async function loadPapers(){
  const res = await fetch(`${API}/papers`);
  allPapers = await res.json();
  renderPapersTable(allPapers);
}

function renderPapersTable(papers){
  const q = document.getElementById('search-inp').value.toLowerCase();
  const filtered = papers.filter(p=>{
    const matchType = !activeTypeFilter || p.paper_type===activeTypeFilter;
    const matchQ = !q || p.title.toLowerCase().includes(q) || p.subject_name.toLowerCase().includes(q);
    return matchType && matchQ;
  });
  const tbody = document.getElementById('papers-body');
  if(!filtered.length){
    tbody.innerHTML = `<tr><td colspan="6"><div class="empty"><div class="empty-icon">📭</div>No papers found</div></td></tr>`;
    return;
  }
  tbody.innerHTML = filtered.map(p=>`
    <tr>
      <td>${p.title}</td>
      <td>${p.subject_name}</td>
      <td>${p.year||'–'}</td>
      <td>${typeBadge(p.paper_type)}</td>
      <td style="color:var(--muted)">${p.filesize?Math.round(p.filesize/1024)+'KB':'–'}</td>
      <td><button class="btn btn-danger" onclick="deletePaper(${p.id},'${p.title.replace(/'/g,"\\'")}')">Delete</button></td>
    </tr>`).join('');
}

function filterTable(){ renderPapersTable(allPapers); }
function setTypeFilter(type,btn){
  activeTypeFilter=type;
  document.querySelectorAll('#type-filters .filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  filterTable();
}

async function loadSubjectSelect(){
  const res = await fetch(`${API}/subjects`);
  const subs = await res.json();
  const sel = document.getElementById('up-subject');
  sel.innerHTML = subs.map(s=>`<option value="${s.id}">${s.name}</option>`).join('');
}

async function loadSubjects(){
  const [sr, pr] = await Promise.all([
    fetch(`${API}/subjects`).then(r=>r.json()),
    fetch(`${API}/papers`).then(r=>r.json())
  ]);
  const counts = {};
  pr.forEach(p=>{ counts[p.subject_id]=(counts[p.subject_id]||0)+1; });
  document.getElementById('subjects-body').innerHTML = sr.map(s=>`
    <tr>
      <td>${s.name}</td>
      <td style="color:var(--muted)">${s.code||'–'}</td>
      <td style="color:var(--accent)">${counts[s.id]||0}</td>
    </tr>`).join('');
}

// ── Upload ─────────────────────────────────────────────────────
function fileChosen(inp){
  const f = inp.files[0];
  document.getElementById('file-name-lbl').textContent = f ? f.name : '';
}
function setupDrop(){
  const zone = document.getElementById('drop-zone');
  zone.addEventListener('dragover',e=>{e.preventDefault();zone.classList.add('dragover');});
  zone.addEventListener('dragleave',()=>zone.classList.remove('dragover'));
  zone.addEventListener('drop',e=>{
    e.preventDefault(); zone.classList.remove('dragover');
    const f = e.dataTransfer.files[0];
    if(f && f.type==='application/pdf'){
      const dt = new DataTransfer(); dt.items.add(f);
      document.getElementById('file-inp').files = dt.files;
      document.getElementById('file-name-lbl').textContent = f.name;
    } else { toast('Only PDF files are allowed','error'); }
  });
}
setupDrop();

async function uploadPaper(){
  const file = document.getElementById('file-inp').files[0];
  const title = document.getElementById('up-title').value.trim();
  const subject_id = document.getElementById('up-subject').value;
  const year = document.getElementById('up-year').value;
  const type = document.getElementById('up-type').value;
  if(!file || !title) return toast('Please fill in title and choose a PDF','error');
  const fd = new FormData();
  fd.append('file', file);
  fd.append('title', title);
  fd.append('subject_id', subject_id);
  fd.append('year', year);
  fd.append('paper_type', type);
  const res = await fetch(`${API}/admin/upload`,{method:'POST',headers:{Authorization:authHeader},body:fd});
  const d = await res.json();
  if(d.success){
    toast('Uploaded successfully ✓');
    document.getElementById('up-title').value='';
    document.getElementById('up-year').value='';
    document.getElementById('file-inp').value='';
    document.getElementById('file-name-lbl').textContent='';
    loadStats();
  } else { toast(d.error||'Upload failed','error'); }
}

asyn
