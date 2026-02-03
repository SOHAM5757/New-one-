<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Will You Be My Valentine?</title>
  <style>
    :root{
      --bg1:#ffdde1;
      --bg2:#ee9ca7;
      --card:#ffffffcc;
      --text:#1f1f1f;
      --accent:#ff4d6d;
      --accent2:#ff85a1;
      --shadow: 0 18px 45px rgba(0,0,0,.18);
    }

    *{ box-sizing:border-box; }
    body{
      margin:0;
      min-height:100vh;
      display:grid;
      place-items:center;
      font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color:var(--text);
      background: radial-gradient(circle at 20% 20%, var(--bg1), transparent 55%),
                  radial-gradient(circle at 80% 30%, #ffd6e0, transparent 45%),
                  linear-gradient(135deg, var(--bg1), var(--bg2));
      overflow:hidden;
    }

    /* floating hearts */
    .hearts{
      position:fixed;
      inset:0;
      pointer-events:none;
      overflow:hidden;
    }
    .heart{
      position:absolute;
      width:18px;
      height:18px;
      background: var(--accent);
      transform: rotate(45deg);
      opacity:.25;
      animation: floatUp linear infinite;
      filter: drop-shadow(0 6px 10px rgba(0,0,0,.15));
    }
    .heart::before,
    .heart::after{
      content:"";
      position:absolute;
      width:18px;
      height:18px;
      border-radius:50%;
      background:inherit;
    }
    .heart::before{ left:-9px; top:0; }
    .heart::after{ left:0; top:-9px; }

    @keyframes floatUp{
      from{ transform: translateY(110vh) rotate(45deg); }
      to  { transform: translateY(-20vh) rotate(45deg); }
    }

    .card{
      width:min(92vw, 520px);
      background:var(--card);
      backdrop-filter: blur(10px);
      border:1px solid rgba(255,255,255,.55);
      border-radius:22px;
      box-shadow:var(--shadow);
      padding:28px 22px;
      text-align:center;
      position:relative;
    }

    .badge{
      display:inline-flex;
      align-items:center;
      gap:10px;
      padding:10px 14px;
      border-radius:999px;
      background: linear-gradient(90deg, #fff, #ffe6ee);
      border:1px solid rgba(255,77,109,.25);
      font-weight:700;
      letter-spacing:.2px;
    }
    .badge .mini-heart{
      width:14px;height:14px;
      background:var(--accent);
      transform: rotate(45deg);
      border-radius:2px;
      position:relative;
    }
    .badge .mini-heart::before,
    .badge .mini-heart::after{
      content:"";
      position:absolute;
      width:14px;height:14px;
      border-radius:50%;
      background:inherit;
    }
    .badge .mini-heart::before{ left:-7px; top:0; }
    .badge .mini-heart::after{ left:0; top:-7px; }

    h1{
      margin:16px 0 8px;
      font-size: clamp(28px, 4vw, 40px);
      line-height:1.1;
    }
    p{
      margin:0 auto 20px;
      max-width:42ch;
      font-size:16px;
      opacity:.85;
    }

    .buttons{
      display:flex;
      gap:14px;
      justify-content:center;
      flex-wrap:wrap;
      margin-top:12px;
    }

    button{
      border:none;
      cursor:pointer;
      padding:12px 18px;
      border-radius:14px;
      font-weight:800;
      font-size:16px;
      transition: transform .15s ease, box-shadow .15s ease;
      min-width:140px;
    }

    .yes{
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      color:white;
      box-shadow: 0 14px 30px rgba(255,77,109,.35);
    }
    .yes:hover{ transform: translateY(-2px) scale(1.02); }

    .no{
      background: #ffffff;
      color:#333;
      border:1px solid rgba(0,0,0,.08);
      box-shadow: 0 10px 20px rgba(0,0,0,.08);
      position:relative;
    }
    .no:hover{ transform: translateY(-2px); }

    .result{
      margin-top:18px;
      font-weight:800;
      color: #b6002f;
      min-height: 28px;
    }

    .footer{
      margin-top:10px;
      font-size:13px;
      opacity:.7;
    }
  </style>
</head>
<body>
  <div class="hearts" aria-hidden="true" id="hearts"></div>

  <main class="card">
    <div class="badge">
      <span class="mini-heart"></span>
      Valentine Proposal
    </div>

    <h1>Will you be my Valentine? 💘</h1>
    <p>I promise snacks, good vibes, and zero drama (okay… minimal 😄)</p>

    <div class="buttons">
      <button class="yes" id="yesBtn">Yes! 💖</button>
      <button class="no" id="noBtn">No 🙃</button>
    </div>

    <div class="result" id="result"></div>
    <div class="footer">Tip: try clicking “No” 😉</div>
  </main>

  <script>
    // Make floating hearts
    const heartsWrap = document.getElementById("hearts");
    const HEARTS = 18;

    for (let i = 0; i < HEARTS; i++) {
      const h = document.createElement("div");
      h.className = "heart";
      const left = Math.random() * 100;
      const duration = 6 + Math.random() * 8;
      const delay = Math.random() * 6;
      const size = 10 + Math.random() * 18;

      h.style.left = left + "vw";
      h.style.animationDuration = duration + "s";
      h.style.animationDelay = delay + "s";
      h.style.width = size + "px";
      h.style.height = size + "px";
      h.style.opacity = (0.15 + Math.random() * 0.25).toFixed(2);

      heartsWrap.appendChild(h);
    }

    // Buttons
    const yesBtn = document.getElementById("yesBtn");
    const noBtn = document.getElementById("noBtn");
    const result = document.getElementById("result");

    yesBtn.addEventListener("click", () => {
      result.textContent = "YAY!! 💞 I knew you’d say yes 😭✨";
    });

    // "No" runs away
    function moveNoButton() {
      const pad = 12;
      const btnRect = noBtn.getBoundingClientRect();

      const maxX = window.innerWidth - btnRect.width - pad;
      const maxY = window.innerHeight - btnRect.height - pad;

      const x = Math.max(pad, Math.random() * maxX);
      const y = Math.max(pad, Math.random() * maxY);

      noBtn.style.position = "fixed";
      noBtn.style.left = x + "px";
      noBtn.style.top = y + "px";
    }

    noBtn.addEventListener("mouseenter", moveNoButton);
    noBtn.addEventListener("click", () => {
      moveNoButton();
      result.textContent = "Hmm… suspicious 🤨 Try again 😌";
    });
  </script>
</body>
</html>
