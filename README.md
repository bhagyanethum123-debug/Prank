<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>System Scan</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      background: #000;
      overflow: hidden;
      font-family: monospace;
    }
    #matrix {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      z-index: 0;
      opacity: 0.35;
    }
    #output {
      position: relative;
      z-index: 1;
      color: #00ff00;
      padding: 15px;
      font-size: 13px;
      line-height: 1.7;
      white-space: pre-wrap;
      text-shadow: 0 0 5px #00ff00;
    }
    .warn { color: #ff3333; text-shadow: 0 0 8px #ff0000; font-weight: bold; }
    .kidding {
      color: #00ff88;
      font-weight: bold;
      font-size: 22px;
      text-shadow: 0 0 15px #00ff88;
      animation: pop 0.6s ease;
    }
    @keyframes pop {
      0% { transform: scale(0.5); opacity: 0; }
      100% { transform: scale(1); opacity: 1; }
    }
    .blink { animation: blink 0.8s infinite; }
    @keyframes blink { 50% { opacity: 0; } }
    .loading { color: #ffaa00; text-shadow: 0 0 8px #ffaa00; font-weight: bold; }
    .ip { color: #00ffff; text-shadow: 0 0 10px #00ffff; font-weight: bold; }
  </style>
</head>
<body>
  <canvas id="matrix"></canvas>
  <div id="output"></div>

  <script>
    // Matrix rain
    const canvas = document.getElementById("matrix");
    const ctx = canvas.getContext("2d");
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    // Japanese අකුරු අයින් කරලා — English + numbers + symbols විතරයි
    const chars = "01ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz#$%&*+=<>{}[]";
    const fontSize = 16;
    const columns = Math.floor(canvas.width / fontSize);
    const drops = Array(columns).fill(1);

    function drawMatrix() {
      ctx.fillStyle = "rgba(0,0,0,0.08)";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = "#00ff00";
      ctx.font = fontSize + "px monospace";
      for (let i = 0; i < drops.length; i++) {
        const text = chars[Math.floor(Math.random() * chars.length)];
        ctx.fillText(text, i * fontSize, drops[i] * fontSize);
        if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) drops[i] = 0;
        drops[i]++;
      }
    }
    setInterval(drawMatrix, 50);

    window.addEventListener("resize", () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });

    // Fake IP generator
    function fakeIP() {
      return `${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}.${Math.floor(Math.random()*255)}`;
    }

    const lines = [
      "> Initializing system scan...",
      "> Target device detected.",
      "> Target name: JANITH",
      "> Target IP: 192.168.1.104",
      "> Public IP: " + fakeIP(),
      "> Establishing connection...",
      "> Port 443 open.",
      "> Port 8080 open.",
      "> Bypassing firewall...",
      "> Firewall bypassed.",
      "> Escalating privileges...",
      "> Root access attempt...",
      "> Root access GRANTED.",
      "> Accessing device memory...",
      "> Reading contacts... 142 found",
      "> Reading messages... 3,847 found",
      "> Reading photos... 2,103 found",
      "> Reading gallery metadata...",
      "> Extracting location history...",
      "> Extracting saved passwords...",
      "> Extracting bank app data...",
      "> Cloning device ID...",
      "",
      "> LOADING DATA... ▓▓▓░░░░░░░ 30%",
      "> LOADING DATA... ▓▓▓▓▓▓░░░░ 60%",
      "> LOADING DATA... ▓▓▓▓▓▓▓▓▓░ 90%",
      "> LOADING DATA... ▓▓▓▓▓▓▓▓▓▓ 100%",
      "",
      "> Uploading contacts... 100%",
      "> Uploading messages... 100%",
      "> Uploading photos... 100%",
      "> Uploading passwords... 100%",
      "> Data transfer complete.",
      "> Server IP: " + fakeIP(),
      "> Connection secured with AES-256.",
      "",
      "⚠️ WARNING: DEVICE COMPROMISED ⚠️",
      "⚠️ ALL DATA HAS BEEN COPIED ⚠️",
      "⚠️ DO NOT TURN OFF THE DEVICE ⚠️",
      "⚠️ CONTACTING YOUR NETWORK PROVIDER ⚠️",
      "⚠️ REPORTING TO AUTHORITIES ⚠️",
      "",
      "█████████████████████ 100%",
      "",
      "...",
      "...",
      "",
      "😂 JUST KIDDING! 😂",
      "මේක නිකම් සෙල්ලමක්! ඔබේ phone එකට කිසිම දෙයක් වුනේ නැහැ.",
      "බය නොවෙන්න — මේක යාළුවෙක් එවපු නිකම් විහිළුවක් 😄"
    ];

    let i = 0;
    const output = document.getElementById("output");
    const totalTime = 5 * 60 * 1000;
    const delay = totalTime / lines.length;

    function typeNext() {
      if (i < lines.length) {
        const line = document.createElement("div");
        const txt = lines[i];

        if (txt.includes("WARNING") || txt.includes("COMPROMISED") || txt.includes("COPIED")) {
          line.className = "warn";
        }
        if (txt.includes("KIDDING") || txt.includes("සෙල්ලමක්")) {
          line.className = "kidding";
        }
        if (txt.includes("REPORTING") || txt.includes("AUTHORITIES")) {
          line.className = "warn blink";
        }
        if (txt.includes("LOADING DATA")) {
          line.className = "loading";
        }
        if (txt.includes("IP:")) {
          line.className = "ip";
        }

        line.textContent = txt;
        output.appendChild(line);
        window.scrollTo(0, document.body.scrollHeight);
        i++;

        const extra = Math.random() * 2500;
        setTimeout(typeNext, delay + extra);
      }
    }

    typeNext();
  </script>
</body>
</html>
