<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>MadderZx-ative-key</title>
  <style>
    :root{
      --bg-overlay: rgba(0,0,10,0.55);
      --card-bg: rgba(255,255,255,0.06);
      --accent: #1e90ff;
      --glass: rgba(255,255,255,0.06);
      --glass-border: rgba(255,255,255,0.08);
      --text: #e6eef8;
      --muted: #bfcfe6;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }

    html,body{
      height:100%;
      margin:0;
      color:var(--text);
      background:linear-gradient(var(--bg-overlay), var(--bg-overlay)), url('https://images.unsplash.com/photo-1446776811953-b23d57bd21aa?auto=format&fit=crop&w=1650&q=80') center/cover fixed no-repeat;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
    }

    .wrap{
      min-height:100vh;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:2rem;
      box-sizing:border-box;
    }

    .panel{
      width:100%;
      max-width:760px;
      background:var(--card-bg);
      border:1px solid var(--glass-border);
      border-radius:14px;
      padding:28px;
      box-shadow:0 8px 30px rgba(0,0,0,0.5);
      backdrop-filter: blur(6px) saturate(120%);
      position:relative;
      overflow:hidden;
    }

    /* Logo circle with a number */
    .logo{
      position:absolute;
      left:20px;
      top:20px;
      width:56px;
      height:56px;
      border-radius:50%;
      background:linear-gradient(135deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02));
      border:1px solid rgba(255,255,255,0.08);
      display:flex;
      align-items:center;
      justify-content:center;
      font-weight:700;
      font-size:22px;
      color:var(--accent);
    }

    .creator{
      position:absolute;
      right:18px;
      top:16px;
      font-size:13px;
      color:var(--muted);
    }

    .title{
      text-align:center;
      margin:26px 0 8px;
    }
    .title h1{
      margin:0;
      font-size:36px;
      color:var(--accent);
      letter-spacing:0.6px;
    }
    .title p{
      margin:6px 0 0;
      color:var(--muted);
      font-size:14px;
    }

    .card{
      margin-top:18px;
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:10px;
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
        <p>Unique site key generator — copy or wait for auto-regenerate every 3 hours</p>
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
            <button id="regenBtn" class="btn-ghost" title="Regenerate Now">Regenerate Now</button>
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
      const regenBtn = document.getElementById('regenBtn');
      const toast = document.getElementById('toast');

      function randStr(len=6){
        const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz23456789';
        let out = '';
        for(let i=0;i<len;i++) out += chars.charAt(Math.floor(Math.random()*chars.length));
        return out;
      }

      function formatKey(){
        // prefix + random part
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
          // create initial key (randomized)
          const initial = formatKey();
          localStorage.setItem(STORAGE_KEY, initial);
          localStorage.setItem(STORAGE_TS, String(Date.now()));
        }
        render();
      })();

      // auto-check every second for countdown and expiration
      setInterval(updateCountdown, 1000);

      // ensure auto-regenerate at exact expiry if tab open
      setInterval(()=>{
        const ts = Number(localStorage.getItem(STORAGE_TS) || 0);
        if(ts && (Date.now() - ts) >= EXPIRE_MS){
          regenerate();
        }
      }, 10 * 1000); // check every 10s

      // Also set a strict timer to regenerate at expiry if tab stays
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
          }, delay + 200); // small buffer
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

      regenBtn.addEventListener('click', ()=>{
        regenerate();
        showToast('Regenerated');
      });

      // Listen for storage events so multiple tabs stay in sync
      window.addEventListener('storage', (ev)=>{
        if(ev.key === STORAGE_KEY || ev.key === STORAGE_TS){
          render();
        }
      });

    })();
  </script>
</body>
</html># ModderZx-key
