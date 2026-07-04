<!doctype html>
<html lang="hi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>MadderZx — Copy Key</title>
  <style>
    :root{
      --bg-url: url('REPLACE_WITH_ANIME_IMAGE_URL'); /* replace with your anime image URL */
      --overlay: linear-gradient(rgba(0,0,0,0.46), rgba(0,0,0,0.46));
      --accent:#00b4ff; --accent-dark:#0072d6;
      --muted:#c7d7ee; --text:#eaf6ff;
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    html,body{ height:100%; margin:0; }
    body{
      background: var(--overlay), var(--bg-url) center/cover fixed no-repeat;
      color:var(--text);
      display:flex; align-items:center; justify-content:center;
      padding:24px; box-sizing:border-box;
      -webkit-font-smoothing:antialiased;
    }

    .card{
      width:100%; max-width:520px;
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:12px; padding:28px; text-align:center;
      box-shadow:0 18px 60px rgba(0,0,0,0.55); border:1px solid rgba(255,255,255,0.06);
      position:relative;
    }

    .logo{ position:absolute; left:18px; top:18px; width:52px; height:52px; border-radius:50%;
      display:flex; align-items:center; justify-content:center; font-weight:800; color:var(--accent);
      background:rgba(255,255,255,0.03); border:1px solid rgba(255,255,255,0.05); }

    h1{ margin:0 0 8px 0; font-size:32px; color:var(--accent); }
    p.lead{ margin:0 0 20px 0; color:var(--muted); }

    .key{
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", monospace;
      background:linear-gradient(90deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      padding:14px;border-radius:10px;font-size:18px;color:var(--text); word-break:break-all;
      border:1px solid rgba(255,255,255,0.03); margin-bottom:12px;
    }

    .btn{ padding:12px 18px;border-radius:12px;border:none;cursor:pointer;font-weight:800;font-size:16px; }
    .btn-primary{ background: linear-gradient(90deg,var(--accent),var(--accent-dark)); color:white; box-shadow:0 12px 30px rgba(0,123,255,0.18); }
    .btn-ghost{ background:transparent; border:1px solid rgba(255,255,255,0.06); color:var(--muted); padding:10px 12px; cursor:default; border-radius:10px; }

    .small{ color:var(--muted); font-size:13px; margin-top:8px; }

    .expired-note{ font-weight:700; color:#ffd1d1; margin-bottom:12px; }

    /* modal */
    .modal-back{ position:fixed; inset:0; display:none; align-items:center; justify-content:center; background:rgba(0,0,0,0.5); z-index:999; }
    .modal{ width:92%; max-width:420px; background:#07101a; border-radius:12px; padding:18px; border:1px solid rgba(255,255,255,0.06); text-align:center; }
    .modal h3{ margin:6px 0 10px 0; color:var(--accent); }
    .modal p{ color:var(--muted); margin:0 0 14px 0; white-space:pre-wrap; }
    .modal .row{ display:flex; gap:8px; justify-content:center; margin-top:8px; flex-wrap:wrap; }
    .disabled{ opacity:0.45; pointer-events:none; }

    @media(max-width:520px){ h1{ font-size:24px } .logo{ left:10px; top:10px; width:44px; height:44px } }
  </style>
</head>
<body>
  <div class="logo" aria-hidden="true">1</div>

  <div class="card" role="main" aria-labelledby="page-title">
    <h1 id="page-title">MadderZx Key</h1>
    <p class="lead">Page pe pehli baar aane par key milegi — reload/return par key expire ho jayegi.</p>

    <div id="activeView">
      <div id="keyValue" class="key" aria-live="polite">--</div>
      <button id="copyBtn" class="btn btn-primary" aria-label="Copy Key">Copy Key</button>
      <div class="small">Key reload/return par expire ho jayegi.</div>
    </div>

    <div id="expiredView" style="display:none;">
      <div class="expired-note">Key expired</div>
      <div class="small">Ab "Get Key" se choose karo kya karna hai (current page replace hoga).</div>
      <div style="margin-top:12px;">
        <button id="getBtn" class="btn btn-primary">Get Key</button>
      </div>
      <div style="margin-top:10px;"><button id="closeExpired" class="btn-ghost">Close</button></div>
    </div>

    <div style="margin-top:14px" class="small">creator by @OpMmadhav</div>
  </div>

  <!-- Modal for choices -->
  <div id="modalBack" class="modal-back" role="dialog" aria-modal="true" aria-hidden="true">
    <div class="modal" role="document">
      <h3>Choose action</h3>
      <p id="modalDesc">Remove this page and open the original site (if available) or open the target link.</p>
      <div class="row">
        <button id="openOriginBtn" class="btn btn-primary">Open original page</button>
        <button id="openTargetBtn" class="btn btn-primary">Open target link</button>
      </div>
      <div style="margin-top:10px;">
        <button id="modalCancel" class="btn-ghost">Cancel</button>
      </div>
    </div>
  </div>

  <script>
    (function(){
      const TARGET = 'https://earnlinks.in/ys8Sk';
      const S_KEY = 'mz_key';
      const S_TS  = 'mz_ts';
      const S_ORG = 'mz_origin';
      const S_LOADED = 'mz_loaded';

      const keyEl = document.getElementById('keyValue');
      const copyBtn = document.getElementById('copyBtn');
      const activeView = document.getElementById('activeView');
      const expiredView = document.getElementById('expiredView');
      const getBtn = document.getElementById('getBtn');
      const closeExpired = document.getElementById('closeExpired');

      const modalBack = document.getElementById('modalBack');
      const openOriginBtn = document.getElementById('openOriginBtn');
      const openTargetBtn = document.getElementById('openTargetBtn');
      const modalCancel = document.getElementById('modalCancel');
      const modalDesc = document.getElementById('modalDesc');

      function randStr(len=6){
        const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz23456789';
        let s=''; for(let i=0;i<len;i++) s+=chars.charAt(Math.floor(Math.random()*chars.length)); return s;
      }
      function mkKey(){ return 'madderZx-free-' + randStr(6); }

      function showActive(key){
        keyEl.textContent = key;
        activeView.style.display = '';
        expiredView.style.display = 'none';
      }
      function showExpired(){
        activeView.style.display = 'none';
        expiredView.style.display = '';
      }

      // Init: if first load in this tab -> generate key and save origin; otherwise expire
      (function init(){
        const loaded = sessionStorage.getItem(S_LOADED);
        if(!loaded){
          const key = mkKey();
          sessionStorage.setItem(S_KEY, key);
          sessionStorage.setItem(S_TS, String(Date.now()));
          const origin = document.referrer || '';
          sessionStorage.setItem(S_ORG, origin);
          sessionStorage.setItem(S_LOADED, '1');
          showActive(key);
        } else {
          // reload or return -> expire
          sessionStorage.removeItem(S_KEY);
          sessionStorage.removeItem(S_TS);
          showExpired();
        }
      })();

      // Copy action
      copyBtn.addEventListener('click', async ()=>{
        const key = sessionStorage.getItem(S_KEY) || '';
        try{
          await navigator.clipboard.writeText(key);
          copyBtn.textContent = 'Copied!';
          setTimeout(()=> copyBtn.textContent = 'Copy Key', 1400);
        }catch(e){
          copyBtn.textContent = 'Copy failed';
          setTimeout(()=> copyBtn.textContent = 'Copy Key', 1400);
        }
      });

      // expired view: Get Key -> show modal choices
      getBtn.addEventListener('click', ()=>{
        const origin = sessionStorage.getItem(S_ORG) || '';
        if(!origin){
          openOriginBtn.classList.add('disabled');
          modalDesc.textContent = 'Original page not known. Aap seedha target link open kar sakte hain.';
        } else {
          openOriginBtn.classList.remove('disabled');
          modalDesc.textContent = `Original page detected:\n${origin}\n\nChoose action: replace current page with original, or replace with target link.`;
        }
        modalBack.style.display = 'flex';
        modalBack.setAttribute('aria-hidden','false');
      });

      // modal cancel
      modalCancel.addEventListener('click', closeModal);
      closeExpired.addEventListener('click', ()=> {
        // just close expired view, show nothing else (keeps page as is)
        // We show expiredView by default; here just do nothing (or hide modal if open)
      });

      function closeModal(){
        modalBack.style.display = 'none';
        modalBack.setAttribute('aria-hidden','true');
      }

      // Open original page (replace current)
      openOriginBtn.addEventListener('click', ()=>{
        const origin = sessionStorage.getItem(S_ORG) || '';
        closeModal();
        try{ sessionStorage.clear(); }catch(e){}
        if(origin){
          location.replace(origin); // replaces current history entry
        } else {
          location.replace(TARGET);
        }
      });

      // Open target (replace current)
      openTargetBtn.addEventListener('click', ()=>{
        closeModal();
        try{ sessionStorage.clear(); }catch(e){}
        location.replace(TARGET);
      });

      // close modal on Escape
      window.addEventListener('keydown', (e)=>{
        if(e.key === 'Escape' && modalBack.style.display === 'flex') closeModal();
      });

    })();
  </script>
</body>
</html>      border-radius:10px;
      padding:18px;
      display:flex;
      align-items:center;
      gap:12px;
      border:1px solid rgba(255,255,255,0.04);
    }

    .keybox{
      flex:1;
      display:flex;
      flex-direction:column;
      gap:8px;
    }

    .key{
      font-family:monospace;
      font-size:20px;
      background:rgba(0,0,0,0.25);
      padding:10px 12px;
      border-radius:8px;
      color:var(--text);
      user-select:text;
      word-break:break-all;
    }

    .controls{
      display:flex;
      gap:8px;
      align-items:center;
    }

    button{
      background:linear-gradient(180deg,var(--accent), #1266d0);
      color:white;
      border:none;
      padding:10px 12px;
      border-radius:8px;
      cursor:pointer;
      font-weight:600;
      box-shadow:0 6px 18px rgba(30,144,255,0.15);
    }

    .btn-ghost{
      background:transparent;
      border:1px solid rgba(255,255,255,0.06);
      color:var(--muted);
      box-shadow:none;
      font-weight:600;
    }

    .muted{
      color:var(--muted);
      font-size:13px;
    }

    .status{
      text-align:right;
      font-size:13px;
      color:var(--muted);
    }

    .toast{
      position:fixed;
      left:50%;
      transform:translateX(-50%);
      bottom:28px;
      background:rgba(0,0,0,0.6);
      color:white;
      padding:8px 14px;
      border-radius:999px;
      font-weight:600;
      display:none;
      z-index:999;
    }

    footer{
      margin-top:16px;
      text-align:center;
      color:var(--muted);
      font-size:13px;
    }

    @media (max-width:520px){
      .panel{ padding:18px; }
      .title h1{ font-size:26px; }
      .logo{ left:12px; top:12px; width:48px; height:48px; font-size:18px; }
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="panel" role="main" aria-labelledby="main-title">
      <div class="logo" aria-hidden="true">1</div>
      <div class="creator">creator by @OpMmadhav</div>

      <div class="title">
        <h1 id="main-title">MadderZx Key</h1>
        <p>Unique site key generator — copy. Key will auto-regenerate every 3 hours.</p>
      </div>

      <div class="card" aria-live="polite">
        <div class="keybox">
          <div class="muted">Your Key</div>
          <div id="key" class="key" title="Your MadderZx key">--</div>
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <div class="muted" id="generatedAt">Generated: --</div>
            <div class="status" id="countdown">Next in: --</div>
          </div>
        </div>

        <div style="display:flex; flex-direction:column; align-items:flex-end;">
          <div class="controls">
            <button id="copyBtn" title="Copy Key">Copy</button>
          </div>
        </div>
      </div>

      <footer>Made with 🌙 — MadderZx-ative-key</footer>
    </div>
  </div>

  <div class="toast" id="toast">Copied!</div>

  <script>
    (function(){
      const STORAGE_KEY = 'madderzx_key';
      const STORAGE_TS  = 'madderzx_ts';
      const EXPIRE_MS = 3 * 60 * 60 * 1000; // 3 hours

      const keyEl = document.getElementById('key');
      const genEl = document.getElementById('generatedAt');
      const countdownEl = document.getElementById('countdown');
      const copyBtn = document.getElementById('copyBtn');
      const toast = document.getElementById('toast');

      function randStr(len=6){
        const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz23456789';
        let out = '';
        for(let i=0;i<len;i++) out += chars.charAt(Math.floor(Math.random()*chars.length));
        return out;
      }

      function formatKey(){
        return 'madderZx-free-' + randStr(6);
      }

      function showToast(msg='Copied!'){
        toast.textContent = msg;
        toast.style.display = 'block';
        toast.style.opacity = '1';
        clearTimeout(toast._t);
        toast._t = setTimeout(()=> toast.style.display = 'none', 1600);
      }

      function setKey(k){
        const now = Date.now();
        localStorage.setItem(STORAGE_KEY, k);
        localStorage.setItem(STORAGE_TS, String(now));
        render();
      }

      function regenerate(){
        const newKey = formatKey();
        setKey(newKey);
      }

      function render(){
        const k = localStorage.getItem(STORAGE_KEY);
        const ts = Number(localStorage.getItem(STORAGE_TS) || 0);
        if(k){
          keyEl.textContent = k;
          genEl.textContent = 'Generated: ' + (ts ? new Date(ts).toLocaleString() : '--');
        } else {
          keyEl.textContent = '--';
          genEl.textContent = 'Generated: --';
        }
        updateCountdown();
      }

      function updateCountdown(){
        const ts = Number(localStorage.getItem(STORAGE_TS) || 0);
        if(!ts){
          countdownEl.textContent = 'Next in: --';
          return;
        }
        const expireAt = ts + EXPIRE_MS;
        const now = Date.now();
        const rem = expireAt - now;
        if(rem <= 0){
          // expired -> regenerate automatically
          regenerate();
          return;
        }
        const hrs = Math.floor(rem / (1000*60*60));
        const mins = Math.floor((rem % (1000*60*60)) / (1000*60));
        const secs = Math.floor((rem % (1000*60)) / 1000);
        countdownEl.textContent = `Next in: ${String(hrs).padStart(2,'0')}:${String(mins).padStart(2,'0')}:${String(secs).padStart(2,'0')}`;
      }

      // On load: ensure a key exists; if not create initial one
      (function init(){
        const existing = localStorage.getItem(STORAGE_KEY);
        const ts = Number(localStorage.getItem(STORAGE_TS) || 0);
        if(!existing || !ts || (Date.now() - ts) >= EXPIRE_MS){
          const initial = formatKey();
          localStorage.setItem(STORAGE_KEY, initial);
          localStorage.setItem(STORAGE_TS, String(Date.now()));
        }
        render();
      })();

      // auto-check every second for countdown and expiration
      setInterval(updateCountdown, 1000);

      // check every 10s for expiry and regenerate if needed (keeps tabs in sync)
      setInterval(()=>{
        const ts = Number(localStorage.getItem(STORAGE_TS) || 0);
        if(ts && (Date.now() - ts) >= EXPIRE_MS){
          regenerate();
        }
      }, 10 * 1000);

      // strict schedule to regenerate exactly at expiry if tab remains open
      (function scheduleStrict(){
        const ts = Number(localStorage.getItem(STORAGE_TS) || 0);
        if(!ts) return;
        const expireAt = ts + EXPIRE_MS;
        const delay = expireAt - Date.now();
        if(delay <= 0){
          regenerate();
        } else {
          setTimeout(()=>{
            regenerate();
            scheduleStrict(); // schedule next
          }, delay + 200);
        }
      })();

      copyBtn.addEventListener('click', async ()=>{
        const k = localStorage.getItem(STORAGE_KEY) || '';
        try{
          await navigator.clipboard.writeText(k);
          showToast('Copied!');
        }catch(e){
          showToast('Copy failed');
        }
      });

      // Listen for storage events so multiple tabs stay in sync
      window.addEventListener('storage', (ev)=>{
        if(ev.key === STORAGE_KEY || ev.key === STORAGE_TS){
          render();
        }
      });

    })();
 
