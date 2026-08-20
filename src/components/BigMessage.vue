<template>
  <section class="big-message">
    <!-- Text first -->
    <h2>No Pressure, Just Me Being Real</h2>

    <p>
      I’m not expecting you to suddenly feel okay or respond differently. I just wanted you to know
      I’m here, I care about you, and I’m willing to take things one step at a time with you.
    </p>

<!-- Guy with glasses blowing a kiss -->
<div class="kiss-guy">
  <div class="guy-face">
    <div class="hair"></div>

    <div class="glasses">
      <span></span>
      <span></span>
    </div>

    <div class="eyes">
      <span>•</span>
      <span>•</span>
    </div>

    <div class="mouth">😘</div>
  </div>

  <div class="guy-body">
    <div class="arm left-arm">👋</div>
    <div class="arm right-arm">💗</div>
  </div>

  <div class="kiss">💋</div>
</div>
  </section>
</template>

<script setup>
import { ref, nextTick } from 'vue'

const opened = ref(false)
const confettiCanvas = ref(null)

const openGift = () => {
  opened.value = true
  nextTick(() => launchConfetti())
}

const launchConfetti = () => {
  const canvas = confettiCanvas.value
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  const ctx = canvas.getContext('2d')

  const confettis = []
  const colors = ['#ff595e', '#ffca3a', '#8ac926', '#1982c4', '#6a4c93']

  for (let i = 0; i < 150; i++) {
    confettis.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height - canvas.height,
      r: Math.random() * 6 + 4,
      color: colors[Math.floor(Math.random() * colors.length)],
      speed: Math.random() * 3 + 2,
      tilt: Math.random() * 10 - 5,
      tiltSpeed: Math.random() * 0.1 + 0.05,
    })
  }

  const draw = () => {
    ctx.clearRect(0, 0, canvas.width, canvas.height)
    confettis.forEach((c) => {
      ctx.beginPath()
      ctx.fillStyle = c.color
      ctx.save()
      ctx.translate(c.x, c.y)
      ctx.rotate((c.tilt * Math.PI) / 180)
      ctx.fillRect(0, 0, c.r, c.r * 0.4)
      ctx.restore()
      c.y += c.speed
      c.tilt += c.tiltSpeed * 5
      if (c.y > canvas.height) {
        c.y = -10
        c.x = Math.random() * canvas.width
      }
    })
    requestAnimationFrame(draw)
  }
  draw()
}
</script>

<style scoped>
.big-message {
  text-align: center;
  padding: 1.25rem 1rem;
  position: relative;
}

/* 💋 Responsive guy with glasses */
.kiss-guy {
  position: relative;
  width: min(180px, 55vw);
  height: min(190px, 58vw);
  margin: 2rem auto 0;
  animation: guyFloat 3s ease-in-out infinite;
}

.guy-face {
  position: relative;
  width: 60%;
  aspect-ratio: 1;
  margin: auto;
  background: #ffd1b3;
  border-radius: 50%;
  border: 3px solid #333;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  z-index: 2;
}

.hair {
  position: absolute;
  top: -6%;
  left: 7%;
  width: 86%;
  height: 35%;
  background: #222;
  border-radius: 50% 50% 25% 25%;
  transform: rotate(-3deg);
}

.glasses {
  position: absolute;
  top: 39%;
  left: 8%;
  width: 84%;
  display: flex;
  justify-content: space-between;
  z-index: 3;
}

.glasses span {
  width: 38%;
  aspect-ratio: 1.4;
  border: 3px solid #222;
  border-radius: 7px;
  background: rgba(255, 255, 255, 0.25);
}

.glasses::after {
  content: '';
  position: absolute;
  top: 42%;
  left: 40%;
  width: 20%;
  height: 3px;
  background: #222;
}

.eyes {
  position: absolute;
  top: 47%;
  left: 25%;
  width: 50%;
  display: flex;
  justify-content: space-between;
  font-size: clamp(10px, 3vw, 16px);
  z-index: 2;
}

.mouth {
  position: absolute;
  bottom: 7%;
  left: 36%;
  font-size: clamp(18px, 7vw, 28px);
  animation: kissFace 1.5s ease-in-out infinite;
}

.guy-body {
  position: absolute;
  bottom: 0;
  left: 20%;
  width: 60%;
  height: 42%;
  background: #4f46e5;
  border-radius: 45px 45px 15px 15px;
  border: 3px solid #333;
}

.arm {
  position: absolute;
  font-size: clamp(20px, 8vw, 32px);
}

.left-arm {
  left: -27%;
  top: 12%;
  transform: rotate(-25deg);
}

.right-arm {
  right: -27%;
  top: 12%;
  transform: rotate(20deg);
}

.kiss {
  position: absolute;
  top: 18%;
  right: 3%;
  font-size: clamp(20px, 7vw, 30px);
  animation: kissFly 2s ease-in-out infinite;
}

/* Floating animation */
@keyframes guyFloat {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-7px);
  }
}

@keyframes kissFace {
  0%,
  100% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.12);
  }
}

@keyframes kissFly {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.5);
  }

  30% {
    opacity: 1;
  }

  70% {
    opacity: 1;
    transform: translate(25px, -12px) scale(1.15);
  }

  100% {
    opacity: 0;
    transform: translate(50px, -20px) scale(0.8);
  }
}

/* 📱 Mobile */
@media (max-width: 600px) {
  .big-message {
    padding: 1.25rem 1rem;
    overflow-x: hidden;
  }

  .big-message h2 {
    font-size: 1.45rem;
    line-height: 1.3;
    margin-bottom: 1rem;
  }

  .big-message p {
    font-size: 0.95rem;
    line-height: 1.6;
    margin: 0 auto;
    max-width: 95%;
  }

  .kiss-guy {
    width: 145px;
    height: 155px;
    margin-top: 1.5rem;
  }
}

/* 📱 Very small phones */
@media (max-width: 380px) {
  .big-message {
    padding: 1rem 0.75rem;
  }

  .big-message h2 {
    font-size: 1.25rem;
  }

  .big-message p {
    font-size: 0.88rem;
  }

  .kiss-guy {
    width: 125px;
    height: 140px;
    margin-top: 1.25rem;
  }
}
</style>
