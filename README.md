<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Gatito Interactivo</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      overflow: hidden;
      background: black;
      font-family: Arial, sans-serif;
    }

    /* Fondo de estrellas animadas */
    .stars, .stars::after, .stars::before {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 200%;
      height: 200%;
      background: transparent url("https://www.transparenttextures.com/patterns/stardust.png") repeat;
      animation: animStar 60s linear infinite;
    }

    .stars::after { animation-duration: 120s; opacity: 0.5; }
    .stars::before { animation-duration: 180s; opacity: 0.3; }

    @keyframes animStar {
      from { transform: translateY(0); }
      to { transform: translateY(-1000px); }
    }

    .cat-container {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 2;
      cursor: pointer;
    }

    .cat {
      width: 200px;
      transition: transform 0.3s ease;
    }

    .message {
      position: absolute;
      top: 75%;
      left: 50%;
      transform: translateX(-50%);
      font-size: 26px;
      color: white;
      display: none;
      text-shadow: 0px 0px 8px #ff66cc;
      z-index: 3;
    }

    .confetti {
      position: absolute;
      font-size: 24px;
      animation: floatUp 2s ease forwards;
      z-index: 3;
    }

    @keyframes floatUp {
      0% { transform: translateY(0) scale(1); opacity: 1; }
      100% { transform: translateY(-150px) scale(1.5); opacity: 0; }
    }
  </style>
</head>
<body>
  <div class="stars"></div>
  <div class="cat-container" id="catBox">
    <img src="https://i.postimg.cc/yddtxzmj/IMG-3480.png" class="cat" id="catImg">
  </div>
  <div class="message" id="msg">te quiero mucho bobita</div>

  <script>
    const cat = document.getElementById("catImg");
    const msg = document.getElementById("msg");
    const catBox = document.getElementById("catBox");

    let totalClicks = 0;
    const moveClicks = 3;
    const explodeClick = 7;

    cat.addEventListener("click", () => {
      totalClicks++;

      if (totalClicks <= moveClicks) {
        moveCat();
      } else if (totalClicks < explodeClick) {
        const growCount = totalClicks - moveClicks;
        cat.style.transform = `scale(${1 + 0.2 * growCount})`;
      } else {
        explodeCat();
      }
    });

    function moveCat() {
      const maxX = window.innerWidth - cat.offsetWidth;
      const maxY = window.innerHeight - cat.offsetHeight;
      const rect = catBox.getBoundingClientRect();
      let newX = rect.left + (Math.random() * 200 - 100);
      let newY = rect.top + (Math.random() * 200 - 100);

      newX = Math.max(0, Math.min(newX, maxX));
      newY = Math.max(0, Math.min(newY, maxY));

      catBox.style.left = `${newX + cat.offsetWidth/2}px`;
      catBox.style.top = `${newY + cat.offsetHeight/2}px`;
    }

    function explodeCat() {
      cat.style.display = "none";
      msg.style.display = "block";
      createConfetti();
    }

    function createConfetti() {
      const rect = catBox.getBoundingClientRect();
      const xCenter = rect.left + rect.width / 2;
      const yCenter = rect.top + rect.height / 2;

      for (let i = 0; i < 30; i++) {
        const confetti = document.createElement("div");
        confetti.classList.add("confetti");
        confetti.innerHTML = "😊";
        document.body.appendChild(confetti);

        confetti.style.left = `${xCenter + (Math.random() * 100 - 50)}px`;
        confetti.style.top = `${yCenter + (Math.random() * 100 - 50)}px`;

        setTimeout(() => {
          confetti.remove();
        }, 2000);
      }
    }
  </script>
</body>
</html>
