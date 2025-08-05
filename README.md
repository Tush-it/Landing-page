<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Code With @code_buddy15</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    :root {
      --bg-color: #0e0e0e;
      --text-color: white;
      --card-bg: rgba(255, 255, 255, 0.05);
      --btn-bg: white;
      --btn-color: #111;
    }

    body.light {
      --bg-color: #f3f3f3;
      --text-color: #111;
      --card-bg: rgba(0, 0, 0, 0.05);
      --btn-bg: #111;
      --btn-color: white;
    }

    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      background: var(--bg-color);
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      overflow-x: hidden;
      color: var(--text-color);
      transition: background 0.5s, color 0.5s;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      z-index: 0;
    }

    .container {
      position: relative;
      z-index: 2;
      max-width: 600px;
      margin: auto;
      top: 50%;
      transform: translateY(-50%);
      background: var(--card-bg);
      backdrop-filter: blur(10px);
      padding: 50px;
      border-radius: 20px;
      text-align: center;
      box-shadow: 0 0 20px rgba(255,255,255,0.05);
    }

    .emoji {
      font-size: 2.5rem;
      margin-bottom: 20px;
    }

    h1 {
      font-size: 1.8em;
      margin-bottom: 10px;
      white-space: nowrap;
      border-right: 2px solid var(--text-color);
      width: fit-content;
      margin-left: auto;
      margin-right: auto;
      overflow: hidden;
      animation: typing 3.5s steps(40, end), blink-caret 0.75s step-end infinite;
    }

    @keyframes typing {
      from { width: 0 }
      to { width: 100% }
    }

    @keyframes blink-caret {
      from, to { border-color: transparent }
      50% { border-color: var(--text-color) }
    }

    p {
      font-size: 1.2em;
      margin: 20px 0 30px;
    }

    .button {
      background: var(--btn-bg);
      color: var(--btn-color);
      padding: 15px 30px;
      border-radius: 30px;
      text-decoration: none;
      font-weight: bold;
      margin: 10px;
      display: inline-block;
      transition: 0.3s ease;
    }

    .button:hover {
      background: #ddd;
      transform: scale(1.05);
    }

    #muteButton, #themeToggle {
      position: absolute;
      top: 20px;
      z-index: 3;
      background: rgba(255,255,255,0.15);
      color: white;
      border: none;
      padding: 10px 15px;
      border-radius: 8px;
      cursor: pointer;
    }

    #muteButton { right: 20px; }
    #themeToggle { left: 20px; }

    .sparkle {
      position: absolute;
      font-size: 1.2em;
      animation: sparkle-fall 6s linear infinite;
      opacity: 0.7;
      pointer-events: none;
    }

    @keyframes sparkle-fall {
      0% {
        transform: translateY(-20px) scale(1);
        opacity: 1;
      }
      100% {
        transform: translateY(100vh) scale(0.5);
        opacity: 0;
      }
    }

    .loader {
      position: fixed;
      z-index: 99;
      background: var(--bg-color);
      color: var(--text-color);
      font-size: 1.5rem;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: opacity 1s ease;
    }

    .hidden {
      opacity: 0;
      pointer-events: none;
    }
  </style>
</head>
<body>

<div class="loader" id="loader">🚀 Loading Your Coding World...</div>

<canvas id="particles"></canvas>

<button id="themeToggle">🌗 Theme</button>
<button id="muteButton">🔇 Mute</button>
<audio id="bgMusic" autoplay loop>
  <source src="https://cdn.pixabay.com/download/audio/2023/06/07/audio_d7de86cc8a.mp3?filename=young-and-beautiful-instrumental-145709.mp3" type="audio/mpeg">
</audio>

<div class="container" id="mainContent">
  <div class="emoji">🧑‍💻</div>
  <h1>Explore Coding World With Me</h1>
  <p>@code_buddy15</p>
  <a class="button" href="mailto:codebuddy15@gmail.com">📧 Contact</a>
  <a class="button" href="https://instagram.com/code_buddy15" target="_blank">📸 Instagram</a>
  <a class="button" href="https://www.youtube.com/@code_buddy15" target="_blank">🎥 YouTube</a>
  <a class="button" href="#" onclick="alert('GEH 💀💀💀')">GEH 💀</a>
</div>

<script>
  // Sparkles
  const body = document.body;
  for (let i = 0; i < 60; i++) {
    const sparkle = document.createElement('div');
    sparkle.classList.add('sparkle');
    sparkle.textContent = '🧑‍💻';
    sparkle.style.left = Math.random() * 100 + 'vw';
    sparkle.style.top = '-' + (Math.random() * 100) + 'px';
    sparkle.style.animationDelay = Math.random() * 5 + 's';
    sparkle.style.animationDuration = 4 + Math.random() * 4 + 's';
    body.appendChild(sparkle);
  }

  // Music toggle
  const bgMusic = document.getElementById("bgMusic");
  const muteBtn = document.getElementById("muteButton");
  let isMuted = false;
  muteBtn.addEventListener("click", () => {
    isMuted = !isMuted;
    bgMusic.muted = isMuted;
    muteBtn.textContent = isMuted ? "🔊 Unmute" : "🔇 Mute";
  });

  // Theme toggle
  const themeBtn = document.getElementById("themeToggle");
  themeBtn.addEventListener("click", () => {
    document.body.classList.toggle("light");
  });

  // Canvas particles
  const canvas = document.getElementById('particles');
  const ctx = canvas.getContext('2d');
  let particles = [];

  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  class Particle {
    constructor() {
      this.x = Math.random() * canvas.width;
      this.y = Math.random() * canvas.height;
      this.radius = Math.random() * 2 + 1;
      this.dx = (Math.random() - 0.5) * 0.5;
      this.dy = (Math.random() - 0.5) * 0.5;
    }
    draw() {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
      ctx.fillStyle = 'rgba(255, 255, 255, 0.5)';
      ctx.fill();
    }
    update() {
      this.x += this.dx;
      this.y += this.dy;
      if (this.x < 0 || this.x > canvas.width) this.dx *= -1;
      if (this.y < 0 || this.y > canvas.height) this.dy *= -1;
      this.draw();
    }
  }

  function initParticles() {
    particles = [];
    for (let i = 0; i < 100; i++) {
      particles.push(new Particle());
    }
  }

  function animateParticles() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    particles.forEach(p => p.update());
    requestAnimationFrame(animateParticles);
  }

  window.addEventListener('resize', () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    initParticles();
  });

  initParticles();
  animateParticles();

  // Animated loader
  window.onload = () => {
    setTimeout(() => {
      document.getElementById("loader").classList.add("hidden");
    }, 1500);
  };
</script>

</body>
</html>
"""

