<template>
  <section class="big-message">
    <!-- Text first -->
    <h2>No Pressure, Just Me Being Real</h2>

    <p>
      I’m not expecting you to suddenly feel okay or respond differently. I just wanted you to know
      I’m here, I care about you, and I’m willing to take things one step at a time with you.
    </p>

    <!-- 🎁 Gift Box under the text -->
    <div class="gift-wrapper" v-if="!opened" @click="openGift">
      <div class="gift-box">
        <div class="lid"></div>
        <div class="box"></div>
      </div>
      <span class="gift-text">Tap to open 🎁</span>
    </div>

    <!-- 🎊 Confetti -->
    <canvas v-if="opened" ref="confettiCanvas" class="confetti-canvas"></canvas>

    <!-- 📜 Scroll -->
    <transition name="scroll">
      <div class="scroll" v-if="opened">
        <p class="surprise">A little surprise just for you 💖</p>
        <a href="/LoveSurprise.zip" download="LoveSurprise.zip"> Download your surprise 🎉 </a>
      </div>
    </transition>
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
  padding: 2rem;
  position: relative;
}

/* 🎁 Gift Box */
.gift-wrapper {
  cursor: pointer;
  margin: 6rem 0 1rem; /* increased top margin */
  perspective: 600px;
}

.gift-box {
  position: relative;
  width: 120px;
  height: 90px;
  margin: auto;
  transform-style: preserve-3d;
  transition: transform 0.6s ease;
}

.gift-box:hover {
  transform: rotateY(10deg) rotateX(5deg);
}

.box {
  width: 100%;
  height: 70px;
  background: linear-gradient(135deg, #e63946, #f77f00);
  border-radius: 10px 10px 12px 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
  position: relative;
  z-index: 1;
}

.lid {
  position: absolute;
  top: -25px;
  width: 100%;
  height: 30px;
  background: linear-gradient(135deg, #ffd700, #ffba00);
  border-radius: 10px 10px 0 0;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  transform-origin: bottom center;
  animation: lidOpen 0.8s forwards;
  z-index: 2;
}

.lid::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 6px;
  background: linear-gradient(90deg, #ffd700, #ffba00);
  transform: translateY(-50%);
  border-radius: 3px;
}
.lid::after {
  content: '';
  position: absolute;
  left: 50%;
  top: -5px;
  width: 15px;
  height: 15px;
  background: #ffd700;
  border-radius: 50%;
  transform: translateX(-50%);
}

/* lid opening animation */
@keyframes lidOpen {
  to {
    transform: rotateX(-60deg) translateY(-20px);
  }
}

/* 📜 Scroll */
.scroll {
  background: #fff5e6;
  padding: 2rem 1.5rem;
  width: 280px;
  margin: 1.5rem auto;
  border-radius: 16px;
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.25);
  border: 2px solid #d9c6a3;
  background-image: linear-gradient(120deg, #fff5e6 0%, #ffeac4 100%);
  background-size: cover;
  overflow: hidden;
  transform-origin: top center;
  font-family: 'Cursive', sans-serif;
  color: #5a4630;
  letter-spacing: 0.5px;
}

.scroll a {
  display: inline-block;
  margin-top: 1rem;
  color: #e63946;
  font-weight: bold;
  text-decoration: none;
}

.scroll-enter-active {
  animation: scrollOpen 0.8s ease-out forwards;
}

@keyframes scrollOpen {
  0% {
    transform: scaleY(0);
    opacity: 0;
  }
  60% {
    transform: scaleY(1.1);
    opacity: 1;
  }
  100% {
    transform: scaleY(1);
    opacity: 1;
  }
}

.gift-text {
  display: block;
  margin-top: 0.5rem;
  font-size: 0.95rem;
  color: #444;
  font-weight: 500;
}

/* Confetti canvas */
.confetti-canvas {
  position: fixed;
  top: 0;
  left: 0;
  pointer-events: none;
  width: 100%;
  height: 100%;
  z-index: 9999;
}

.surprise {
  display: block;
  margin-top: 0.5rem;
  font-size: 0.9rem;
  color: #141414;
}
</style>
