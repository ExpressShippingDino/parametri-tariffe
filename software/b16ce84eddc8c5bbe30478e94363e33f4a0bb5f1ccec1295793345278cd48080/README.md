[combina pdf.html](https://github.com/user-attachments/files/31956270/combina.pdf.html)
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PDF — Unisci & Dividi</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.17.1/pdf-lib.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<style>
  :root{
    --ink:#1a1410; --paper:#f6f1e7; --paper-2:#efe7d6;
    --accent:#c2451f; --accent-deep:#9c3416;
    --line:#2c2218; --muted:#7a6f5f; --ok:#2f6b3a;
    --shadow:0 2px 0 var(--line);
  }
  *{box-sizing:border-box;margin:0;padding:0}
  body{
    font-family:'Iowan Old Style','Palatino Linotype',Palatino,Georgia,serif;
    background:var(--paper); color:var(--ink);
    -webkit-font-smoothing:antialiased; padding:32px 20px 80px;
    background-image:radial-gradient(var(--paper-2) 1px,transparent 1px);
    background-size:18px 18px;
  }
  .wrap{max-width:760px;margin:0 auto}
  header{border-bottom:3px solid var(--line);padding-bottom:18px;margin-bottom:28px}
  .eyebrow{font-family:'SF Mono',ui-monospace,Menlo,monospace;font-size:11px;letter-spacing:.22em;text-transform:uppercase;color:var(--accent-deep)}
  h1{font-size:clamp(34px,7vw,52px);line-height:.95;font-weight:700;letter-spacing:-.01em;margin-top:8px}
  h1 em{font-style:italic;color:var(--accent)}
  .sub{font-family:'SF Mono',ui-monospace,Menlo,monospace;font-size:12.5px;color:var(--muted);margin-top:12px}
  .tabs{display:flex;gap:0;margin-bottom:24px;border:2px solid var(--line)}
  .tab{flex:1;background:var(--paper);border:none;border-right:2px solid var(--line);
    padding:13px;font-family:inherit;font-size:17px;cursor:pointer;color:var(--ink);transition:.12s}
  .tab:last-child{border-right:none}
  .tab[aria-selected="true"]{background:var(--ink);color:var(--paper)}
  .tab:hover:not([aria-selected="true"]){background:var(--paper-2)}
  .panel{display:none}
  .panel.on{display:block}
  .drop{border:2px dashed var(--line);background:var(--paper-2);padding:34px 20px;text-align:center;
    cursor:pointer;transition:.12s}
  .drop:hover,.drop.hot{background:#e7dcc6;border-color:var(--accent)}
  .drop strong{display:block;font-size:19px;margin-bottom:4px}
  .drop span{font-family:'SF Mono',ui-monospace,monospace;font-size:12px;color:var(--muted)}
  ul.files{list-style:none;margin-top:18px}
  ul.files li{display:flex;align-items:center;gap:12px;background:var(--paper);
    border:2px solid var(--line);padding:11px 14px;margin-bottom:9px;box-shadow:var(--shadow)}
  .idx{font-family:'SF Mono',monospace;font-size:13px;color:var(--paper);background:var(--accent);
    width:26px;height:26px;display:grid;place-items:center;flex:none}
  .fname{flex:1;font-size:15px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .meta{font-family:'SF Mono',monospace;font-size:11px;color:var(--muted)}
  .mini{background:none;border:1.5px solid var(--line);font-family:inherit;cursor:pointer;
    padding:3px 8px;font-size:13px;line-height:1}
  .mini:hover{background:var(--ink);color:var(--paper)}
  .mini:disabled{opacity:.25;cursor:default}
  .field{margin-top:18px}
  label.lbl{display:block;font-family:'SF Mono',monospace;font-size:11px;letter-spacing:.12em;
    text-transform:uppercase;color:var(--accent-deep);margin-bottom:7px}
  input[type=text]{width:100%;border:2px solid var(--line);background:var(--paper);
    padding:11px 13px;font-family:'SF Mono',monospace;font-size:14px;color:var(--ink)}
  input[type=text]:focus{outline:none;border-color:var(--accent)}
  select{width:100%;border:2px solid var(--line);background:var(--paper);
    padding:11px 13px;font-family:'SF Mono',monospace;font-size:13px;color:var(--ink);cursor:pointer}
  select:focus{outline:none;border-color:var(--accent)}
  .hint{font-family:'SF Mono',monospace;font-size:11.5px;color:var(--muted);margin-top:6px}
  .go{margin-top:24px;width:100%;background:var(--accent);color:#fff;border:2px solid var(--line);
    padding:15px;font-family:inherit;font-size:19px;cursor:pointer;box-shadow:var(--shadow);transition:.1s}
  .go:hover:not(:disabled){background:var(--accent-deep)}
  .go:active:not(:disabled){transform:translateY(2px);box-shadow:none}
  .go:disabled{background:var(--paper-2);color:var(--muted);cursor:default}
  .out{margin-top:20px}
  .out a{display:flex;align-items:center;gap:10px;text-decoration:none;color:var(--ink);
    background:#eef5e8;border:2px solid var(--ok);padding:12px 14px;margin-bottom:8px;
    box-shadow:0 2px 0 var(--ok);font-size:15px}
  .out a .dl{margin-left:auto;font-family:'SF Mono',monospace;font-size:12px;color:var(--ok)}
  .err{color:var(--accent-deep);font-family:'SF Mono',monospace;font-size:13px;margin-top:14px}
  .note{margin-top:30px;border-top:1.5px solid var(--line);padding-top:14px;
    font-family:'SF Mono',monospace;font-size:11.5px;color:var(--muted);line-height:1.7}
  .note b{color:var(--ok)}

  /* ---- preview grid ---- */
  .prev-bar{display:flex;align-items:center;gap:10px;flex-wrap:wrap;margin-top:20px}
  .prev-bar .lbl{margin:0}
  .prev-bar .count{font-family:'SF Mono',monospace;font-size:12px;color:var(--accent-deep);margin-left:auto}
  .prev-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(96px,1fr));gap:10px;margin-top:12px}
  .thumb{border:2px solid var(--line);background:var(--paper);cursor:pointer;position:relative;
    padding:6px;text-align:center;transition:.1s;user-select:none}
  .thumb:hover{background:var(--paper-2)}
  .thumb canvas{width:100%;height:auto;display:block;background:#fff;border:1px solid var(--muted)}
  .thumb .pg{font-family:'SF Mono',monospace;font-size:11px;color:var(--muted);margin-top:5px}
  .thumb.sel{border-color:var(--accent);background:#fbe9df;box-shadow:var(--shadow)}
  .thumb.sel .pg{color:var(--accent-deep);font-weight:700}
  .thumb .tick{position:absolute;top:4px;right:4px;width:20px;height:20px;background:var(--accent);
    color:#fff;font-size:12px;display:none;place-items:center;border:1.5px solid var(--line)}
  .thumb.sel .tick{display:grid}
  .loading{font-family:'SF Mono',monospace;font-size:12px;color:var(--muted);margin-top:14px}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="eyebrow">PDF Toolkit · local only</div>
    <h1>Unisci <em>&</em> Dividi PDF — ESC</h1>
    <p class="sub">Tutto avviene nel tuo browser. Nessun file lascia il dispositivo.</p>
  </header>

  <div class="tabs" role="tablist">
    <button class="tab" id="t-merge" role="tab" aria-selected="true" onclick="switchTab('merge')">Unisci</button>
    <button class="tab" id="t-split" role="tab" aria-selected="false" onclick="switchTab('split')">Dividi</button>
  </div>

  <!-- MERGE -->
  <section class="panel on" id="p-merge">
    <div class="drop" id="drop-merge">
      <strong>Trascina qui i PDF</strong>
      <span>o clicca per sceglierli — l'ordine è quello di caricamento</span>
      <input type="file" id="in-merge" accept="application/pdf" multiple hidden>
    </div>
    <ul class="files" id="list-merge"></ul>
    <button class="go" id="go-merge" disabled onclick="doMerge()">Unisci in un PDF</button>
    <div class="out" id="out-merge"></div>
    <div class="err" id="err-merge"></div>
  </section>

  <!-- SPLIT -->
  <section class="panel" id="p-split">
    <div class="drop" id="drop-split">
      <strong>Trascina qui un PDF</strong>
      <span>o clicca per sceglierlo</span>
      <input type="file" id="in-split" accept="application/pdf" hidden>
    </div>
    <ul class="files" id="list-split"></ul>

    <!-- anteprima pagine -->
    <div id="prev-wrap" style="display:none">
      <div class="prev-bar">
        <span class="lbl">Anteprima — clicca le pagine da estrarre</span>
        <button class="mini" onclick="selectAll()">Tutte</button>
        <button class="mini" onclick="selectNone()">Nessuna</button>
        <span class="count" id="sel-count">0 selezionate</span>
      </div>
      <div class="prev-grid" id="prev-grid"></div>
      <div class="loading" id="prev-loading"></div>
    </div>

    <div class="field">
      <label class="lbl" for="ranges">Pagine da estrarre</label>
      <input type="text" id="ranges" placeholder="es. 1-3, 5, 8-10" oninput="rangesToSel()">
      <p class="hint" id="ranges-hint">Clicca le pagine sopra, oppure scrivi qui gli intervalli. Vuoto = una pagina per file.</p>
    </div>
    <div class="field">
      <label class="lbl" for="split-mode">Come salvare</label>
      <select id="split-mode">
        <option value="combined">Un unico PDF con tutte le pagine selezionate</option>
        <option value="per-range">Un file per intervallo (es. 1-3 → un file, 5 → un file)</option>
        <option value="per-page">Un file separato per ogni pagina</option>
      </select>
    </div>
    <button class="go" id="go-split" disabled onclick="doSplit()">Dividi PDF</button>
    <div class="out" id="out-split"></div>
    <div class="err" id="err-split"></div>
  </section>

  <p class="note">
    <b>Privacy:</b> l'elaborazione è 100% client-side. Funziona anche offline una volta caricata la pagina.<br>
    <b>Unisci:</b> riordina i file con ↑ ↓ prima di procedere.<br>
    <b>Dividi:</b> scegli le pagine dall'anteprima o scrivi gli intervalli, es. <b>1-3, 5, 8-10</b>.
  </p>
</div>

<script>
const { PDFDocument } = PDFLib;
if(window.pdfjsLib) pdfjsLib.GlobalWorkerOptions.workerSrc='https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';
let mergeFiles = [];
let splitFile = null;
let splitTotal = 0;
let selected = new Set();   // page numbers 1-based

function switchTab(t){
  document.getElementById('t-merge').setAttribute('aria-selected', t==='merge');
  document.getElementById('t-split').setAttribute('aria-selected', t==='split');
  document.getElementById('p-merge').classList.toggle('on', t==='merge');
  document.getElementById('p-split').classList.toggle('on', t==='split');
}
const fmt = b => b<1024?b+' B':b<1048576?(b/1024).toFixed(0)+' KB':(b/1048576).toFixed(1)+' MB';

/* ---- drag & drop wiring ---- */
function wireDrop(dropId, inputId, onFiles, multiple){
  const d=document.getElementById(dropId), inp=document.getElementById(inputId);
  d.onclick=()=>inp.click();
  inp.onchange=()=>{ if(inp.files.length) onFiles([...inp.files]); inp.value=''; };
  ['dragover','dragenter'].forEach(e=>d.addEventListener(e,ev=>{ev.preventDefault();d.classList.add('hot');}));
  ['dragleave','drop'].forEach(e=>d.addEventListener(e,ev=>{ev.preventDefault();d.classList.remove('hot');}));
  d.addEventListener('drop',ev=>{
    const fs=[...ev.dataTransfer.files].filter(f=>f.type==='application/pdf');
    if(fs.length) onFiles(multiple?fs:[fs[0]]);
  });
}

/* ---- MERGE ---- */
wireDrop('drop-merge','in-merge', fs=>{ mergeFiles.push(...fs); renderMerge(); }, true);
function renderMerge(){
  const ul=document.getElementById('list-merge'); ul.innerHTML='';
  mergeFiles.forEach((f,i)=>{
    const li=document.createElement('li');
    li.innerHTML=`<span class="idx">${i+1}</span>
      <span class="fname">${f.name}</span>
      <span class="meta">${fmt(f.size)}</span>
      <button class="mini" ${i===0?'disabled':''} onclick="moveMerge(${i},-1)">↑</button>
      <button class="mini" ${i===mergeFiles.length-1?'disabled':''} onclick="moveMerge(${i},1)">↓</button>
      <button class="mini" onclick="rmMerge(${i})">✕</button>`;
    ul.appendChild(li);
  });
  document.getElementById('go-merge').disabled = mergeFiles.length<2;
  document.getElementById('out-merge').innerHTML='';
  document.getElementById('err-merge').textContent='';
}
function moveMerge(i,d){ const j=i+d; [mergeFiles[i],mergeFiles[j]]=[mergeFiles[j],mergeFiles[i]]; renderMerge(); }
function rmMerge(i){ mergeFiles.splice(i,1); renderMerge(); }

async function doMerge(){
  const btn=document.getElementById('go-merge'), err=document.getElementById('err-merge');
  err.textContent=''; btn.disabled=true; btn.textContent='Unione in corso…';
  try{
    const out=await PDFDocument.create();
    for(const f of mergeFiles){
      const src=await PDFDocument.load(await f.arrayBuffer());
      const pages=await out.copyPages(src, src.getPageIndices());
      pages.forEach(p=>out.addPage(p));
    }
    const bytes=await out.save();
    showLinks('out-merge',[{name:'documento-unito.pdf',bytes,pages:out.getPageCount()}]);
  }catch(e){ err.textContent='Errore: '+e.message+' — il file potrebbe essere protetto o danneggiato.'; }
  btn.disabled=false; btn.textContent='Unisci in un PDF';
}

/* ---- SPLIT ---- */
wireDrop('drop-split','in-split', fs=>{ splitFile=fs[0]; selected=new Set(); renderSplit(); }, false);

async function renderSplit(){
  const ul=document.getElementById('list-split'); ul.innerHTML='';
  document.getElementById('out-split').innerHTML='';
  document.getElementById('err-split').textContent='';
  document.getElementById('ranges').value='';
  const wrap=document.getElementById('prev-wrap'), grid=document.getElementById('prev-grid');
  grid.innerHTML=''; wrap.style.display='none';
  if(!splitFile){ document.getElementById('go-split').disabled=true; return; }

  let pages='?';
  try{ pages=(await PDFDocument.load(await splitFile.arrayBuffer())).getPageCount(); }catch{}
  splitTotal = typeof pages==='number'?pages:0;

  const li=document.createElement('li');
  li.innerHTML=`<span class="idx">1</span>
    <span class="fname">${splitFile.name}</span>
    <span class="meta">${pages} pag · ${fmt(splitFile.size)}</span>
    <button class="mini" onclick="splitFile=null;selected=new Set();renderSplit()">✕</button>`;
  ul.appendChild(li);
  document.getElementById('ranges-hint').textContent=
    `${pages} pagine totali. Clicca le pagine da estrarre, oppure scrivi gli intervalli. Vuoto = una pagina per file.`;
  document.getElementById('go-split').disabled=false;
  updateCount();
  renderThumbs();
}

async function renderThumbs(){
  const wrap=document.getElementById('prev-wrap'), grid=document.getElementById('prev-grid');
  const loading=document.getElementById('prev-loading');
  if(!window.pdfjsLib){ return; }   // niente anteprima se la lib non è disponibile
  wrap.style.display='block';
  loading.textContent='Genero anteprime…';
  try{
    const data=await splitFile.arrayBuffer();
    const pdf=await pdfjsLib.getDocument({data}).promise;
    for(let n=1;n<=pdf.numPages;n++){
      const cell=document.createElement('div');
      cell.className='thumb'; cell.dataset.pg=n;
      cell.onclick=()=>toggle(n);
      cell.innerHTML=`<div class="tick">✓</div><canvas></canvas><div class="pg">pag ${n}</div>`;
      grid.appendChild(cell);
      const page=await pdf.getPage(n);
      const vp0=page.getViewport({scale:1});
      const scale=Math.min(0.35, 150/vp0.width);
      const vp=page.getViewport({scale});
      const canvas=cell.querySelector('canvas');
      canvas.width=vp.width; canvas.height=vp.height;
      await page.render({canvasContext:canvas.getContext('2d'),viewport:vp}).promise;
    }
    loading.textContent='';
    syncThumbs();
  }catch(e){ loading.textContent='Anteprima non disponibile per questo file.'; }
}

function toggle(n){
  if(selected.has(n)) selected.delete(n); else selected.add(n);
  syncThumbs(); selToRanges(); updateCount();
}
function selectAll(){ selected=new Set(); for(let i=1;i<=splitTotal;i++)selected.add(i); syncThumbs(); selToRanges(); updateCount(); }
function selectNone(){ selected=new Set(); syncThumbs(); selToRanges(); updateCount(); }

function syncThumbs(){
  document.querySelectorAll('#prev-grid .thumb').forEach(c=>{
    c.classList.toggle('sel', selected.has(+c.dataset.pg));
  });
}
function updateCount(){
  document.getElementById('sel-count').textContent =
    selected.size ? `${selected.size} selezionate` : 'nessuna → una pagina per file';
}

/* selezione -> testo intervalli compatti */
function selToRanges(){
  const arr=[...selected].sort((a,b)=>a-b);
  const parts=[]; let i=0;
  while(i<arr.length){ let j=i; while(j+1<arr.length && arr[j+1]===arr[j]+1) j++;
    parts.push(arr[i]===arr[j]?`${arr[i]}`:`${arr[i]}-${arr[j]}`); i=j+1; }
  document.getElementById('ranges').value=parts.join(', ');
}
/* testo intervalli -> selezione (mentre scrivi) */
function rangesToSel(){
  const raw=document.getElementById('ranges').value;
  const s=new Set();
  try{ for(const j of parseRanges(raw, splitTotal||1e9)) j.idx.forEach(i=>s.add(i+1)); }catch{ return; }
  selected=s; syncThumbs(); updateCount();
}

function parseRanges(str,max){
  const out=[];
  for(let part of str.split(',')){
    part=part.trim(); if(!part) continue;
    const m=part.match(/^(\d+)\s*-\s*(\d+)$/);
    if(m){
      let a=+m[1],b=+m[2]; if(a>b)[a,b]=[b,a];
      const idx=[]; for(let p=a;p<=b;p++){ if(p<1||p>max) throw Error(`pagina ${p} fuori intervallo (1-${max})`); idx.push(p-1);}
      out.push({label:`pag-${a}-${b}`,idx});
    }else if(/^\d+$/.test(part)){
      const p=+part; if(p<1||p>max) throw Error(`pagina ${p} fuori intervallo (1-${max})`);
      out.push({label:`pag-${p}`,idx:[p-1]});
    }else throw Error(`"${part}" non è un intervallo valido`);
  }
  return out;
}

async function doSplit(){
  const btn=document.getElementById('go-split'), err=document.getElementById('err-split');
  err.textContent=''; btn.disabled=true; btn.textContent='Divisione in corso…';
  try{
    const src=await PDFDocument.load(await splitFile.arrayBuffer());
    const total=src.getPageCount();
    const raw=document.getElementById('ranges').value.trim();
    const mode=document.getElementById('split-mode').value;
    let jobs;
    if(!raw){
      // niente selezione: una pagina per file
      jobs = src.getPageIndices().map(i=>({label:`pag-${i+1}`,idx:[i]}));
    }else if(mode==='combined'){
      // tutte le pagine selezionate in un unico PDF (ordine di pagina)
      const all=[...new Set(parseRanges(raw,total).flatMap(j=>j.idx))].sort((a,b)=>a-b);
      const label = all.length===1 ? `pag-${all[0]+1}`
                  : `selezione-${all[0]+1}-${all[all.length-1]+1}`;
      jobs=[{label,idx:all}];
    }else if(mode==='per-page'){
      jobs = parseRanges(raw,total).flatMap(j=>j.idx.map(i=>({label:`pag-${i+1}`,idx:[i]})));
    }else{ // per-range
      jobs = parseRanges(raw,total);
    }
    const base=splitFile.name.replace(/\.pdf$/i,'');
    const results=[];
    for(const j of jobs){
      const doc=await PDFDocument.create();
      const pgs=await doc.copyPages(src,j.idx);
      pgs.forEach(p=>doc.addPage(p));
      results.push({name:`${base}_${j.label}.pdf`,bytes:await doc.save(),pages:j.idx.length});
    }
    showLinks('out-split',results);
  }catch(e){ err.textContent='Errore: '+e.message; }
  btn.disabled=false; btn.textContent='Dividi PDF';
}

/* ---- output links ---- */
function showLinks(id,items){
  const box=document.getElementById(id); box.innerHTML='';
  items.forEach(it=>{
    const url=URL.createObjectURL(new Blob([it.bytes],{type:'application/pdf'}));
    const a=document.createElement('a');
    a.href=url; a.download=it.name;
    a.innerHTML=`📄 ${it.name} <span class="dl">${it.pages} pag · scarica ↓</span>`;
    box.appendChild(a);
  });
}
</script>
</body>
</html>
