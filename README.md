# riceage.github.io
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>MeduLingo — Duolingo‑style Med School SRS</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Baloo+2:wght@600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg: #0f172a;            /* slate-900 */
      --panel: #0b1225;         /* deeper */
      --panel-2: #111a33;
      --muted: #94a3b8;        /* slate-400 */
      --text: #e2e8f0;         /* slate-200 */
      --accent: #6ee7b7;       /* emerald-300 */
      --accent-2: #a78bfa;     /* violet-400 */
      --accent-3: #22d3ee;     /* cyan-400 */
      --danger: #fb7185;       /* rose-400 */
      --gold: #facc15;         /* amber-400 */
      --ok: #34d399;           /* emr-400 */
      --good: #60a5fa;         /* blue-400 */
      --easy: #86efac;         /* green-300 */
      --round: 18px;
      --shadow: 0 10px 30px rgba(0,0,0,.35), 0 2px 6px rgba(0,0,0,.3);
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; background:
        radial-gradient(1200px 700px at 10% -10%, rgba(167,139,250,.25), transparent 60%),
        radial-gradient(1000px 600px at 110% 10%, rgba(34,211,238,.18), transparent 60%),
        linear-gradient(0deg, var(--bg), var(--bg));
      color:var(--text); font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji", "Segoe UI Emoji";
      display:grid; grid-template-rows:auto 1fr; gap:18px;
    }
    header{
      display:flex; align-items:center; justify-content:space-between; padding:16px 20px;
    }
    .brand{display:flex; gap:12px; align-items:center}
    .logo{
      width:42px; height:42px; border-radius:14px; background: conic-gradient(from 220deg, var(--accent-2), var(--accent-3), var(--accent));
      box-shadow: inset 0 0 18px rgba(255,255,255,.15), var(--shadow);
    }
    h1{font-family:"Baloo 2", Inter, sans-serif; font-weight:800; letter-spacing:.5px; font-size:24px; margin:0}
    .sub{color:var(--muted); font-size:12px}
    .controls{display:flex; gap:10px; align-items:center}
    button, .btn{
      background:linear-gradient(180deg, #1b2648, #121a34);
      color:var(--text); border:1px solid #223061; border-radius:14px; padding:10px 14px; cursor:pointer; font-weight:600; transition:.2s transform, .2s filter, .2s background;
      box-shadow: var(--shadow);
    }
    button:hover{transform:translateY(-1px); filter:brightness(1.06)}
    button:active{transform:translateY(0)}
    input, select{
      background:#0c1530; border:1px solid #223061; color:var(--text); border-radius:12px; padding:10px 12px; box-shadow:var(--shadow);
    }
    main{display:grid; grid-template-columns:280px 1fr; gap:18px; padding:0 20px 24px}
    .card{background:linear-gradient(180deg, var(--panel), var(--panel-2)); border:1px solid #1f2a4d; border-radius:var(--round); padding:16px; box-shadow: var(--shadow)}
    .stack{display:flex; flex-direction:column; gap:12px}
    .row{display:flex; gap:10px; align-items:center; flex-wrap:wrap}
    .split{display:grid; grid-template-columns:1fr auto; gap:8px; align-items:center}
    .muted{color:var(--muted)}
    .pill{padding:6px 10px; background:#14204a; border:1px solid #223061; border-radius:999px; font-size:12px}

    /* Left rail */
    #profile{position:sticky; top:10px}
    .avatar{width:64px; height:64px; border-radius:22px; background:linear-gradient(135deg, #2a345b, #1b244b); display:grid; place-items:center; font-weight:800; font-size:22px}
    .xpbar{height:10px; background:#121a34; border:1px solid #223061; border-radius:999px; overflow:hidden}
    .xpbar > div{height:100%; background:linear-gradient(90deg, var(--gold), #ffd166); border-radius:999px}

    /* Tree area */
    #treeWrap{position:relative; overflow:auto; min-height:520px}
    svg.tree{width:100%; height:100%; min-height:560px}
    .node{cursor:pointer}
    .node circle{filter: drop-shadow(0 10px 18px rgba(0,0,0,.45));}
    .node-label{font-weight:700; font-size:12px; pointer-events:none}
    .badge{position:absolute; top:10px; right:10px}

    /* Review modal */
    dialog{border:none; border-radius:24px; padding:0; width:min(720px, 94vw); background:linear-gradient(180deg, #0b1225, #0e1731); color:var(--text); box-shadow: var(--shadow)}
    dialog::backdrop{background:rgba(2,6,23,.7)}
    .modal-header{display:flex; justify-content:space-between; align-items:center; padding:16px 18px; border-bottom:1px solid #1f2a4d}
    .modal-body{padding:18px; display:grid; gap:12px}
    .qa{background:#0c1530; border:1px solid #223061; border-radius:18px; padding:18px; min-height:100px}
    .ans{display:none}
    .srs-buttons{display:grid; grid-template-columns:repeat(4, 1fr); gap:10px}
    .srs-buttons button{border-radius:14px}
    .again{background:linear-gradient(180deg, #3b0a18, #2a0812); border:1px solid #5b1126}
    .hard{background:linear-gradient(180deg, #2a1a0b, #1c1409); border:1px solid #6b4d1f}
    .good{background:linear-gradient(180deg, #112131, #0c1624); border:1px solid #335a8e}
    .easy{background:linear-gradient(180deg, #0f2616, #0b1b10); border:1px solid #22633f}

    /* Leaderboard */
    table{width:100%; border-collapse:separate; border-spacing:0 10px}
    th, td{padding:8px 10px; text-align:left}
    tbody tr{background:#0c1530; border:1px solid #223061}
    tbody td:first-child{border-radius:12px 0 0 12px}
    tbody td:last-child{border-radius:0 12px 12px 0}

    /* Toast */
    .toast{position:fixed; bottom:18px; left:50%; transform:translateX(-50%); background:#0c1530; border:1px solid #223061; border-radius:999px; padding:10px 14px; display:none; box-shadow:var(--shadow)}

    /* Mobile */
    @media (max-width: 980px){
      main{grid-template-columns:1fr}
      #treeWrap{min-height:420px}
    }
  </style>
</head>
<body>
  <header>
    <div class="brand">
      <div class="logo" aria-hidden></div>
      <div>
        <h1>MeduLingo</h1>
        <div class="sub">Duolingo‑style lessons • Spaced‑repetition • Anki/CrowdAnki import</div>
      </div>
    </div>
    <div class="controls">
      <select id="treeStyle">
        <option value="flow">Tree: Flow</option>
        <option value="grid">Tree: Grid</option>
        <option value="radial">Tree: Radial</option>
      </select>
      <label class="btn" for="deckFile">Import JSON Deck</label>
      <input type="file" id="deckFile" accept="application/json" style="display:none" />
      <button id="demoBtn">Load Demo Deck</button>
      <button id="signOut">Sign out</button>
    </div>
  </header>

  <main>
    <!-- Left rail: profile & stats -->
    <section id="profile" class="stack">
      <div class="card stack">
        <div class="row">
          <div id="avatar" class="avatar">🧠</div>
          <div>
            <div style="font-weight:800; font-size:18px" id="username">—</div>
            <div class="row">
              <span class="pill">Streak <span id="streak">0</span> 🔥</span>
              <span class="pill">XP <span id="xp">0</span> ⭐</span>
            </div>
          </div>
        </div>
        <div class="stack">
          <div class="split">
            <div class="muted">Daily XP</div>
            <div><b id="dailyXp">0</b>/50</div>
          </div>
          <div class="xpbar"><div id="xpbarFill" style="width:0%"></div></div>
        </div>
        <div class="row">
          <button id="reviewDue">Review Due (0)</button>
          <button id="newLesson">New Lesson</button>
        </div>
      </div>

      <div class="card">
        <div class="split">
          <h3 style="margin:0">Leaderboard</h3>
          <button id="refreshLb">↻</button>
        </div>
        <table>
          <thead><tr><th>Rank</th><th>User</th><th>Streak</th><th>XP</th></tr></thead>
          <tbody id="lb"></tbody>
        </table>
      </div>

      <div class="card stack">
        <h3 style="margin:0 0 6px 0">Decks</h3>
        <div id="deckList" class="stack"></div>
      </div>
    </section>

    <!-- Right: lesson tree & content -->
    <section class="card" id="treeWrap">
      <div class="badge pill" id="deckMeta">No deck loaded</div>
      <svg class="tree" id="treeSvg" viewBox="0 0 1200 800" preserveAspectRatio="xMidYMid meet" aria-label="Lesson Tree"></svg>
    </section>
  </main>

  <dialog id="reviewModal">
    <div class="modal-header">
      <div class="row">
        <span class="pill" id="reviewType">Review</span>
        <span class="pill" id="reviewCounter">0/0</span>
      </div>
      <button id="closeModal">✕</button>
    </div>
    <div class="modal-body">
      <div class="qa" id="question"></div>
      <div class="qa ans" id="answer"></div>
      <div class="row">
        <button id="showAnswer">Show answer</button>
        <span class="muted" id="dueHint"></span>
      </div>
      <div class="srs-buttons">
        <button class="again" data-rating="again">Again</button>
        <button class="hard" data-rating="hard">Hard</button>
        <button class="good" data-rating="good">Good</button>
        <button class="easy" data-rating="easy">Easy</button>
      </div>
    </div>
  </dialog>

  <div class="toast" id="toast"></div>

  <script>
  /******************** UTILITIES ********************/
  const $ = (sel, el=document) => el.querySelector(sel);
  const $$ = (sel, el=document) => Array.from(el.querySelectorAll(sel));
  const fmtDate = (d) => new Date(d).toISOString().slice(0,10);
  const todayKey = () => new Date().toLocaleDateString('en-GB', { timeZone: Intl.DateTimeFormat().resolvedOptions().timeZone });
  const uid = () => Math.random().toString(36).slice(2,10);
  const clamp = (n, a, b) => Math.max(a, Math.min(b, n));
  const toast = (msg) => { const t = $('#toast'); t.textContent = msg; t.style.display='block'; setTimeout(()=> t.style.display='none', 2000); };

  /******************** STORAGE & USERS ********************/
  const store = {
    read(key, def){ try { return JSON.parse(localStorage.getItem(key)) ?? def; } catch { return def; } },
    write(key, val){ localStorage.setItem(key, JSON.stringify(val)); }
  };

  const USERS_KEY = 'medu_users_v1';
  const STATE_KEY = (userId) => `medu_state_${userId}`; // decks, cards, srs

  function ensureUser(){
    let users = store.read(USERS_KEY, []);
    if(!users.length){
      const name = prompt('Choose a username');
      const id = uid();
      users.push({ id, name, avatar: randomAvatarEmoji(), xp:0, streak:0, lastActive: null });
      store.write(USERS_KEY, users);
      store.write(STATE_KEY(id), defaultState());
      sessionStorage.setItem('medu_current', id);
    }
    let current = sessionStorage.getItem('medu_current');
    if(!current){
      // simple switcher
      const name = prompt('Login — enter existing username or new');
      let u = users.find(x=> x.name === name);
      if(!u){ u = { id: uid(), name, avatar: randomAvatarEmoji(), xp:0, streak:0, lastActive:null }; users.push(u); store.write(USERS_KEY, users); store.write(STATE_KEY(u.id), defaultState()); }
      sessionStorage.setItem('medu_current', u.id);
    }
    return currentUser();
  }
  function currentUser(){ const id = sessionStorage.getItem('medu_current'); const users = store.read(USERS_KEY, []); return users.find(u=>u.id===id); }
  function updateUser(patch){ const users = store.read(USERS_KEY, []); const id = sessionStorage.getItem('medu_current'); const i = users.findIndex(u=>u.id===id); users[i] = {...users[i], ...patch}; store.write(USERS_KEY, users); }
  function defaultState(){ return { decks: {}, xpByDay: {}, trees: {}, theme:'flow' }; }

  function randomAvatarEmoji(){ const em = ['🧠','🫀','🫁','🧬','🧪','🦴','🧫','💉','🦠','🫀']; return em[Math.floor(Math.random()*em.length)]; }

  /******************** ANKI / DECK PARSER ********************/
  // Accepts: CrowdAnki deck JSON, or array of Q/A objects, or simple {notes:[...]}
  async function parseDeck(json){
    let name = json.name || json.deckName || 'Imported Deck';
    if(json.__type__ === 'Deck' || json.crowdanki_uuid){
      // CrowdAnki format
      const notes = json.notes || json['notes'] || [];
      const cards = notes.map((n,i)=>{
        const fields = n.fields || n['fields'] || [];
        const front = Array.isArray(fields) ? (fields[0]??'') : (fields['Front']||fields['front']||'');
        const back  = Array.isArray(fields) ? (fields[1]??'') : (fields['Back']||fields['back']||'');
        const tags  = (n.tags || []).map(t=> String(t));
        return { id: n.guid || n.note_uuid || uid(), front, back, tags };
      });
      return { name, id: json.crowdanki_uuid || uid(), cards };
    }
    // AnkiConnect‑like
    if(Array.isArray(json.notes)){
      const cards = json.notes.map(n=>({ id: n.id || uid(), front: n.front || n.question || '', back: n.back || n.answer || '', tags: n.tags || [] }));
      return { name, id: uid(), cards };
    }
    if(Array.isArray(json)){
      const cards = json.map(n=>({ id: n.id||uid(), front:n.front||n.q||'', back:n.back||n.a||'', tags:n.tags||[] }));
      return { name, id: uid(), cards };
    }
    throw new Error('Unrecognized deck JSON shape. Expect CrowdAnki export or array of {front, back, tags}');
  }

  function tagsToPath(tags){
    // pick the first tag that contains ::
    const long = (tags||[]).find(t=> t.includes('::')) || (tags||[])[0] || 'Misc';
    return String(long).split('::').filter(Boolean);
  }

  /******************** SRS (SM‑2 flavored with ease drift) ********************/
  function srsInit(card){
    return { cid: card.id, ivl: 0, ease: 2.5, reps:0, lapses:0, due: Date.now(), state:'new' };
  }
  function rateCard(s, rating){
    const now = Date.now();
    const minutes = 60*1000; const day = 24*60*60*1000;
    const againIvl = 10 * minutes; // learning step
    const hardF = 1.2; const easyBonus = 1.3;

    let ease = s.ease, ivl = s.ivl, reps = s.reps, lapses = s.lapses, state = s.state;

    if(state === 'new' || state === 'learning'){
      if(rating==='again'){ ivl = againIvl; state = 'learning'; ease = Math.max(1.3, ease - 0.2); }
      else if(rating==='hard'){ ivl = 20*minutes; state='learning'; ease=Math.max(1.3, ease-0.15); }
      else if(rating==='good'){ ivl = day; state='review'; reps++; ease = ease + 0.0; }
      else if(rating==='easy'){ ivl = 3*day; state='review'; reps++; ease = ease + 0.1; }
    } else { // review state (SM-2 like)
      if(rating==='again'){ lapses++; ivl = againIvl; state='relearning'; ease = Math.max(1.3, ease - 0.2); }
      else if(rating==='hard'){ ivl = Math.max(day, ivl * hardF); ease=Math.max(1.3, ease-0.15); }
      else if(rating==='good'){ ivl = Math.max(day, ivl * ease); }
      else if(rating==='easy'){ ivl = Math.max(day, ivl * ease * easyBonus); ease = ease + 0.05; }
    }

    const due = now + ivl;
    return { ...s, ease, ivl, reps, lapses, due, state };
  }

  /******************** STATE & DECK MANAGEMENT ********************/
  function getState(){ return store.read(STATE_KEY(currentUser().id), defaultState()); }
  function setState(next){ store.write(STATE_KEY(currentUser().id), next); }

  function upsertDeck(deck){
    const state = getState();
    const did = deck.id;
    const existing = state.decks[did];
    const cards = deck.cards.map(c=> ({ ...c, id: String(c.id) }));
    const srs = existing?.srs || {};
    for(const c of cards){ if(!srs[c.id]) srs[c.id] = srsInit(c); }
    state.decks[did] = { id:did, name: deck.name, cards, srs };
    setState(state);
    refreshDecks();
    buildTree(did);
  }

  function refreshDecks(){
    const state = getState();
    const wrap = $('#deckList');
    wrap.innerHTML = '';
    Object.values(state.decks).forEach(d=>{
      const dueCount = dueCards(d).length;
      const el = document.createElement('div');
      el.className = 'row';
      el.innerHTML = `<div class="pill">${d.name}</div><button data-open="${d.id}">Open</button><span class="muted">Due: ${dueCount}</span>`;
      el.querySelector('button').onclick = ()=> buildTree(d.id);
      wrap.appendChild(el);
    });
  }

  function dueCards(deck){
    const now = Date.now();
    return deck.cards.filter(c=> deck.srs[c.id] && deck.srs[c.id].due <= now);
  }

  /******************** TREE RENDERING ********************/
  function buildTree(deckId){
    const state = getState();
    const deck = state.decks[deckId] || Object.values(state.decks)[0];
    if(!deck){ $('#treeSvg').innerHTML = ''; $('#deckMeta').textContent = 'No deck loaded'; return; }
    state.currentDeck = deck.id; setState(state);
    $('#deckMeta').textContent = `${deck.name} • ${deck.cards.length} cards`;

    // Group cards by tag path
    const groups = {};
    for(const c of deck.cards){
      const path = tagsToPath(c.tags);
      const key = path.join(' / ');
      if(!groups[key]) groups[key] = { path, cards:[] };
      groups[key].cards.push(c);
    }
    const nodes = Object.values(groups).map((g,i)=>({ id: g.path.join('>')||`Group ${i+1}` , label: g.path[g.path.length-1]||'Misc', path:g.path, count:g.cards.length }));

    renderTree($('#treeSvg'), nodes);
  }

  function renderTree(svg, nodes){
    const style = $('#treeStyle').value; // flow | grid | radial
    svg.innerHTML = '';

    const W = svg.viewBox.baseVal.width || 1200;
    const H = svg.viewBox.baseVal.height || 800;
    const margin = 80;
    const r = 28; // node radius

    let positions = {};
    if(style==='grid'){
      const cols = Math.ceil(Math.sqrt(nodes.length));
      const rows = Math.ceil(nodes.length / cols);
      nodes.forEach((n, i)=>{
        const x = margin + (i % cols) * ((W-2*margin)/(cols-1||1));
        const y = margin + Math.floor(i/cols) * ((H-2*margin)/(rows-1||1));
        positions[n.id] = {x,y};
      });
    } else if(style==='radial'){
      const cx=W/2, cy=H/2, R=Math.min(W,H)/2 - margin;
      nodes.forEach((n,i)=>{
        const a = (i/nodes.length)*Math.PI*2 - Math.PI/2;
        positions[n.id] = { x: cx + R*Math.cos(a), y: cy + R*Math.sin(a) };
      });
    } else { // flow
      const cols = Math.ceil(Math.sqrt(nodes.length*1.3));
      nodes.forEach((n,i)=>{
        const row = Math.floor(i/cols);
        const col = i%cols;
        const x = margin + col * ((W-2*margin)/(cols-1||1));
        const y = margin + row * 120 + (col%2)*18; // playful stagger
        positions[n.id] = {x,y};
      });
    }

    // draw links (simple nearest‑neighbor chain to create a path)
    const gLinks = document.createElementNS('http://www.w3.org/2000/svg','g');
    gLinks.setAttribute('stroke', 'rgba(255,255,255,.08)');
    gLinks.setAttribute('stroke-width', '8');
    gLinks.setAttribute('fill', 'none');
    const sorted = nodes.slice().sort((a,b)=> a.path.length - b.path.length || a.label.localeCompare(b.label));
    for(let i=1;i<sorted.length;i++){
      const a = positions[sorted[i-1].id];
      const b = positions[sorted[i].id];
      const path = document.createElementNS('http://www.w3.org/2000/svg','path');
      const d = `M ${a.x} ${a.y} Q ${(a.x+b.x)/2} ${(a.y+b.y)/2 + 60} ${b.x} ${b.y}`; // curved
      path.setAttribute('d', d);
      path.setAttribute('stroke-linecap','round');
      path.setAttribute('opacity','0.9');
      gLinks.appendChild(path);
    }
    svg.appendChild(gLinks);

    // nodes
    const gNodes = document.createElementNS('http://www.w3.org/2000/svg','g');
    nodes.forEach(n=>{
      const p = positions[n.id];
      const g = document.createElementNS('http://www.w3.org/2000/svg','g');
      g.classList.add('node');
      g.setAttribute('transform', `translate(${p.x},${p.y})`);

      // node circle
      const c = document.createElementNS('http://www.w3.org/2000/svg','circle');
      c.setAttribute('r', 28);
      c.setAttribute('cx', 0); c.setAttribute('cy', 0);
      c.setAttribute('fill', `url(#grad-${n.path.length})`);
      c.setAttribute('stroke', '#223061');
      c.setAttribute('stroke-width', '2');

      // label
      const t = document.createElementNS('http://www.w3.org/2000/svg','text');
      t.textContent = n.label;
      t.setAttribute('class','node-label');
      t.setAttribute('x', 0); t.setAttribute('y', 50); t.setAttribute('text-anchor','middle');

      // count badge
      const badge = document.createElementNS('http://www.w3.org/2000/svg','text');
      badge.textContent = n.count;
      badge.setAttribute('x', 0); badge.setAttribute('y', 6); badge.setAttribute('text-anchor','middle');
      badge.setAttribute('font-weight','800');

      g.appendChild(c); g.appendChild(badge); g.appendChild(t);
      gNodes.appendChild(g);

      g.addEventListener('click', ()=> openReviewForPath(n.path));
    });
    svg.appendChild(gNodes);

    // gradients defs
    const defs = document.createElementNS('http://www.w3.org/2000/svg','defs');
    for(let i=1;i<6;i++){
      const grad = document.createElementNS('http://www.w3.org/2000/svg','radialGradient');
      grad.id = `grad-${i}`;
      grad.innerHTML = `
      <stop offset="20%" stop-color="${i%2? '#a78bfa':'#6ee7b7'}" stop-opacity="0.9"/>
      <stop offset="95%" stop-color="#0c1530" stop-opacity="1"/>`;
      defs.appendChild(grad);
    }
    svg.appendChild(defs);
  }

  async function openReviewForPath(path){
    const state = getState();
    const deck = state.decks[state.currentDeck];
    const key = path.join(' / ');
    const subset = deck.cards.filter(c=> (tagsToPath(c.tags).join(' / ') === key));
    startSession(deck, subset);
  }

  /******************** REVIEW SESSION ********************/
  let session = null;
  function startSession(deck, subset){
    const isLesson = subset.some(c=> deck.srs[c.id]?.state==='new');
    const due = dueCards({cards:subset, srs: deck.srs});
    const newOnes = subset.filter(c=> deck.srs[c.id]?.state==='new');
    const pool = (due.length>0 ? due : newOnes).slice(0, 20);
    if(!pool.length){ toast('Nothing due here — try another skill!'); return; }

    session = { deckId: deck.id, ids: pool.map(c=>c.id), idx:0, isLesson: !due.length, shownAns:false };
    $('#reviewType').textContent = session.isLesson ? 'Lesson' : 'Review';
    $('#reviewCounter').textContent = `1/${session.ids.length}`;
    $('#reviewModal').showModal();
    showCard();
  }

  function showCard(){
    const state = getState(); const deck = state.decks[session.deckId];
    const cid = session.ids[session.idx]; const card = deck.cards.find(c=> c.id===cid);
    $('#question').innerHTML = sanitize(card.front);
    $('#answer').innerHTML = sanitize(card.back);
    $('#answer').style.display = session.shownAns ? 'block' : 'none';
    $('#dueHint').textContent = '';
    $('#reviewCounter').textContent = `${session.idx+1}/${session.ids.length}`;
  }

  $('#showAnswer').onclick = () => { session.shownAns = true; $('#answer').style.display='block'; };
  $$('.srs-buttons button').forEach(b=> b.addEventListener('click', ()=> rate(b.dataset.rating)));

  function rate(rating){
    const state = getState(); const deck = state.decks[session.deckId];
    const cid = session.ids[session.idx];
    const s0 = deck.srs[cid] || srsInit({id:cid});
    const s1 = rateCard(s0, rating);
    deck.srs[cid] = s1; state.decks[deck.id] = deck; setState(state);

    // XP & streaks
    const xpGain = { again:2, hard:6, good:10, easy:12 }[rating] || 8;
    addXP(xpGain);

    // next
    session.idx++;
    session.shownAns=false;
    if(session.idx >= session.ids.length){
      $('#reviewModal').close();
      buildTree(deck.id);
      toast(`Great work! +${xpGain} XP`);
    } else {
      showCard();
    }
  }

  function addXP(x){
    const user = currentUser();
    const today = todayKey();
    const state = getState();
    state.xpByDay[today] = (state.xpByDay[today]||0) + x;
    setState(state);

    const total = (user.xp||0) + x; updateUser({ xp: total });
    // streak
    const last = user.lastActive ? new Date(user.lastActive) : null;
    const now = new Date();
    const daysApart = last ? Math.floor((now - new Date(fmtDate(last))) / (24*3600*1000)) : Infinity;
    let streak = user.streak||0;
    if(!last || daysApart > 1) streak = 1; else if(daysApart === 1) streak = streak + 1; // if same day, unchanged
    updateUser({ lastActive: now.toISOString(), streak });
    renderProfile(); renderLeaderboard();
  }

  /******************** UI RENDERERS ********************/
  function renderProfile(){
    const u = currentUser(); if(!u) return;
    $('#username').textContent = u.name;
    $('#avatar').textContent = u.avatar;
    $('#streak').textContent = u.streak||0;
    $('#xp').textContent = u.xp||0;

    const state = getState();
    const today = state.xpByDay[todayKey()] || 0;
    $('#dailyXp').textContent = today;
    $('#xpbarFill').style.width = `${clamp((today/50)*100, 0, 100)}%`;

    // review due across current deck
    const deck = state.decks[state.currentDeck || Object.keys(state.decks)[0]];
    const due = deck ? dueCards(deck).length : 0;
    $('#reviewDue').textContent = `Review Due (${due})`;
  }

  function renderLeaderboard(){
    const users = store.read(USERS_KEY, []).sort((a,b)=> (b.streak||0)-(a.streak||0) || (b.xp||0)-(a.xp||0));
    const tbody = $('#lb'); tbody.innerHTML='';
    users.forEach((u,i)=>{
      const tr = document.createElement('tr'); tr.innerHTML = `<td>#${i+1}</td><td>${u.name}</td><td>${u.streak||0}</td><td>${u.xp||0}</td>`; tbody.appendChild(tr);
    });
  }

  /******************** SANITIZER (very light) ********************/
  function sanitize(html){
    const tmp = document.createElement('div'); tmp.innerHTML = html;
    // remove scripts & inline events
    tmp.querySelectorAll('script, iframe, object, embed').forEach(x=>x.remove());
    tmp.querySelectorAll('*').forEach(el=>{ [...el.attributes].forEach(a=>{ if(a.name.startsWith('on')) el.removeAttribute(a.name); }); });
    return tmp.innerHTML;
  }

  /******************** EVENT WIRING ********************/
  $('#treeStyle').addEventListener('change', ()=>{ const s=getState(); s.theme=$('#treeStyle').value; setState(s); buildTree(s.currentDeck); });
  $('#closeModal').onclick = ()=> $('#reviewModal').close();
  $('#reviewDue').onclick = ()=>{ const s=getState(); const d=s.decks[s.currentDeck||Object.keys(s.decks)[0]]; if(!d) return toast('No deck'); const pool = dueCards(d).slice(0,40); if(!pool.length){ toast('Nothing due! Try a skill.'); return; } startSession(d, pool); };
  $('#newLesson').onclick = ()=>{ const s=getState(); const d=s.decks[s.currentDeck||Object.keys(s.decks)[0]]; if(!d) return toast('No deck'); const newOnes = d.cards.filter(c=> d.srs[c.id]?.state==='new'); startSession(d, newOnes); };
  $('#refreshLb').onclick = renderLeaderboard;
  $('#signOut').onclick = ()=>{ sessionStorage.removeItem('medu_current'); location.reload(); };

  $('#deckFile').addEventListener('change', async (e)=>{
    const file = e.target.files[0]; if(!file) return;
    try{
      const text = await file.text();
      const json = JSON.parse(text);
      const deck = await parseDeck(json);
      upsertDeck(deck);
      toast(`Imported ${deck.cards.length} cards from ${deck.name}`);
    }catch(err){
      console.error(err); toast('Import failed: '+err.message);
    }
  });

  $('#demoBtn').addEventListener('click', ()=> loadDemo());

  function loadDemo(){
    const demo = {
      name: 'Demo — Cardio & Embryology',
      cards: [
        { id: uid(), front: '<b>What closes the foramen ovale after birth?</b>', back:'Increased left atrial pressure causing septum primum to appose septum secundum.', tags:['Embryology::Heart development'] },
        { id: uid(), front: 'Define <i>preload</i>.', back:'End-diastolic ventricular wall stress/volume.', tags:['Cardiology::Physiology::Hemodynamics'] },
        { id: uid(), front: 'ECG: What does the P wave represent?', back:'Atrial depolarization.', tags:['Cardiology::ECG'] },
        { id: uid(), front: 'Embryologic origin of the aortic arch?', back:'Left fourth aortic arch → aorta.', tags:['Embryology::Aortic arches'] },
        { id: uid(), front: 'What is afterload?', back:'The load the ventricle must overcome to eject blood (approximate by MAP).', tags:['Cardiology::Physiology::Hemodynamics'] },
        { id: uid(), front: 'Teratogen: ACE inhibitors cause?', back:'Renal agenesis/oligohydramnios, fetal renal failure.', tags:['Embryology::Teratology'] },
        { id: uid(), front: 'Name the heart valve with 3 cusps that guards the pulmonary trunk.', back:'Pulmonary valve.', tags:['Cardiology::Anatomy'] },
        { id: uid(), front: 'Neural crest contributes to which great vessel structure?', back:'Aorticopulmonary septum.', tags:['Embryology::Heart development'] },
      ]
    };
    upsertDeck({ ...demo, id: 'demo-'+uid() });
  }

  /******************** INIT ********************/
  (function init(){
    const u = ensureUser(); renderProfile(); renderLeaderboard(); refreshDecks();
    const state = getState(); $('#treeStyle').value = state.theme || 'flow';
    // tiny confetti on first load
    setTimeout(()=> sparkles(document.body, 14), 600);
  })();

  /******************** FUN BITS ********************/
  function sparkles(el, n=10){
    for(let i=0;i<n;i++){
      const s = document.createElement('div');
      s.textContent = ['✨','🌟','💫','⭐'][i%4];
      s.style.position='fixed'; s.style.zIndex=99; s.style.pointerEvents='none';
      const x = Math.random()*window.innerWidth; const y = -20; const rot = (Math.random()*40-20);
      s.style.left=x+'px'; s.style.top=y+'px'; s.style.transform=`rotate(${rot}deg)`;
      document.body.appendChild(s);
      const dx = (Math.random()*2-1)*80; const dy = 120+Math.random()*60; const t = 900+Math.random()*600;
      s.animate([{transform:`translate(0,0) rotate(${rot}deg)`, opacity:1},{transform:`translate(${dx}px, ${dy}px) rotate(${rot*2}deg)`, opacity:0}], { duration:t, easing:'cubic-bezier(.2,.6,.25,1)' }).onfinish=()=> s.remove();
    }
  }
  </script>
</body>
</html>
