<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>autopass</title>
  <link rel="icon" href="https://photosaya.io/images/2024/07/09/LOGO.png" type="image/png"/>

  <style>
    :root {
      --bg: rgba(255, 255, 255, 0.95);
      --text: #000;
      --box: #fff;
    }

    body.dark-mode {
      --bg: rgba(0, 0, 0, 0.8);
      --text: #eee;
      --box: #1e1e1e;
    }

    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-image: url('https://imagme.com/images/2025/02/21/photo_2025-02-21_01-31-20.jpeg');
      background-size: cover;
      background-position: center;
      background-attachment: fixed;
      background-repeat: no-repeat;
      color: var(--text);
      transition: 0.3s ease-in-out;
      text-align: center;
      overflow-x: hidden;
    }

    canvas#snow {
      position: fixed;
      top: 0;
      left: 0;
      pointer-events: none;
      z-index: 1;
    }

    .judul-gif {
      margin-top: 30px;
      margin-bottom: 10px;
      position: relative;
      z-index: 2;
    }

    .judul-gif img {
      max-width: 300px;
      height: auto;
      filter: drop-shadow(2px 4px 6px rgba(0, 0, 0, 0.5));
    }

    .login-form {
      background-color: var(--box);
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
      display: inline-block;
      width: 300px;
      margin-top: 20px;
      position: relative;
      z-index: 2;
    }

    .input-field {
      margin: 10px 0;
      padding: 10px;
      width: 100%;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 5px;
      background-color: var(--bg);
      color: var(--text);
    }

    /* 🎯 Tombol Neon Glow Style */
    .btn-neon {
      position: relative;
      display: inline-block;
      padding: 12px 30px;
      margin: 8px;
      font-size: 16px;
      font-weight: bold;
      color: #fffb00;
      text-decoration: none;
      background: linear-gradient(0deg, #000, #272727);
      border-radius: 10px;
      text-align: center;
      z-index: 2;
      cursor: pointer;
    }

    .btn-neon:before,
    .btn-neon:after {
      content: '';
      position: absolute;
      right: -3px;
      bottom: -3px;
      background: linear-gradient(45deg, #fb0000, #ff8d00, #490000, #c01d1d, #ffe200, #fb0000, #ffa700, #490000, #c01d1d, #efff00);
      background-size: 400%;
      width: calc(100% + 6px);
      height: calc(100% + 6px);
      z-index: -1;
      animation: steam 5s linear infinite;
      border-radius: 10px;
    }

    .btn-neon:after {
      filter: blur(30px);
    }

    @keyframes steam {
      0% { background-position: 0 0; }
      50% { background-position: 400% 0; }
      100% { background-position: 0 0; }
    }

    label {
      display: block;
      font-weight: bold;
      margin-bottom: 5px;
    }

    audio {
      display: none;
    }

    .btn-wrap {
      margin-top: 20px;
      z-index: 3;
      position: relative;
    }
  </style>
</head>
<body>

  <!-- ❄️ SALJU -->
  <canvas id="snow"></canvas>

  <!-- 🎵 BACKSOUND -->
  <audio id="backsound" autoplay loop>
    <source src="https://cdn.pixabay.com/download/audio/2022/12/07/audio_dcef74f48d.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>

  <!-- 🎬 JUDUL GIF -->
  <div class="judul-gif">
    <img src="https://imagme.com/images/2024/11/11/gif-toto12.gif" alt="Judul GIF">
  </div>

  <!-- 🌗 TOMBOL TEMA + MUTE -->
  <div class="btn-wrap">
    <button class="btn-neon" onclick="toggleTheme()">🌗 Ganti Tema</button>
    <button class="btn-neon mute-btn" onclick="toggleMute()">🔊 Mute</button>
  </div>

  <!-- 📋 FORM -->
  <div class="login-form">
    <label for="full-text">Silahkan di Login Ya bosku Dengan</label>
    <textarea id="full-text" class="input-field" rows="8" readonly>
Silahkan di Login Ya bosku Dengan

ID : 
password:

Dan ubah password bosku sesuai dengan keinginan bosku
Jangan beritahu password bosku kepada orang lain untuk keamanan akun bosku 🙂

Link login : https://toto12oslo.net
    </textarea><br />
    <button class="btn-neon copy-btn" onclick="copyAndChangePassword()">📋 Copy Cok!</button>
  </div>

  <!-- ✨ SCRIPT: SALJU -->
  <script>
    const canvas = document.getElementById("snow");
    const ctx = canvas.getContext("2d");
    let w = window.innerWidth;
    let h = window.innerHeight;
    canvas.width = w;
    canvas.height = h;

    const maxFlakes = 100;
    const flakes = [];

    function Flake() {
      this.x = Math.random() * w;
      this.y = Math.random() * h;
      this.radius = Math.random() * 3 + 1;
      this.speed = Math.random() * 1 + 0.5;
      this.wind = Math.random() * 1 - 0.5;

      this.update = function () {
        this.y += this.speed;
        this.x += this.wind;

        if (this.y > h) {
          this.y = 0;
          this.x = Math.random() * w;
        }
        if (this.x > w || this.x < 0) {
          this.x = Math.random() * w;
        }
      };

      this.draw = function () {
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
        ctx.fillStyle = "rgba(255, 255, 255, 0.8)";
        ctx.fill();
      };
    }

    function createFlakes() {
      for (let i = 0; i < maxFlakes; i++) {
        flakes.push(new Flake());
      }
    }

    function animateFlakes() {
      ctx.clearRect(0, 0, w, h);
      for (let flake of flakes) {
        flake.update();
        flake.draw();
      }
      requestAnimationFrame(animateFlakes);
    }

    window.addEventListener("resize", () => {
      w = window.innerWidth;
      h = window.innerHeight;
      canvas.width = w;
      canvas.height = h;
    });

    createFlakes();
    animateFlakes();
  </script>

  <!-- 📋 SCRIPT: PASSWORD + SALIN -->
  <script>
    const passwords = ["bunga123", "kucing456", "apel789", "matahari22"];
    const prefixList = ["gacor", "jitu", "bola", "maxwin", "bisa", "pasti", "hoki", "menang", "emas", "super", "juara", "naga", "maju", "king", "bintang"];
    for (let prefix of prefixList) {
      for (let i = 1; i <= 999; i++) {
        passwords.push(`${prefix}${i.toString().padStart(3, '0')}`);
      }
    }

    const filteredPasswords = passwords.filter((pw) => pw.length >= 6);

    function copyAndChangePassword() {
      const password = filteredPasswords[Math.floor(Math.random() * filteredPasswords.length)];
      const newText = `Silahkan di Login Ya bosku Dengan

ID : 
password: ${password}

Dan ubah password bosku sesuai dengan keinginan bosku
Jangan beritahu password bosku kepada orang lain untuk keamanan akun bosku 🙂

Link login :https://toto12oslo.net/`;

      const copyText = document.getElementById("full-text");
      copyText.value = newText;
      copyText.select();
      document.execCommand("copy");
    }

    function toggleTheme() {
      document.body.classList.toggle("dark-mode");
    }

    function toggleMute() {
      const audio = document.getElementById("backsound");
      const muteBtn = document.querySelector(".mute-btn");
      if (audio.muted) {
        audio.muted = false;
        muteBtn.textContent = "🔊 Mute";
      } else {
        audio.muted = true;
        muteBtn.textContent = "🔇 Unmute";
      }
    }

    document.addEventListener("DOMContentLoaded", function () {
      const backsound = document.getElementById("backsound");
      backsound.volume = 0.5;
      document.body.addEventListener("click", () => {
        if (backsound.paused) backsound.play();
      }, { once: true });
    });
  </script>
</body>
</html>
