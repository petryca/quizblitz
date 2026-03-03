<template>
  <div class="start-screen">
    <div class="bg-grid" aria-hidden="true"></div>
    <div class="bg-glow" aria-hidden="true"></div>

    <main class="content">
      <div class="logo-wrap">
        <span class="logo-icon" aria-hidden="true">⚡</span>
        <h1 class="heading">QuizBlitz</h1>
      </div>
      <p class="tagline">Test your knowledge. Beat the clock. Own the leaderboard.</p>
      <button class="play-btn" @click="handlePlay">
        <span class="play-btn__label">Play</span>
        <span class="play-btn__arrow" aria-hidden="true">→</span>
      </button>
    </main>
  </div>
</template>

<script>
export default {
  name: 'StartScreen',
  emits: ['start'],
  methods: {
    handlePlay() {
      this.$emit('start');
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@400;500&display=swap');

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.start-screen {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: #0a0a0f;
  overflow: hidden;
  font-family: 'DM Sans', sans-serif;
}

/* ── Background grid ── */
.bg-grid {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(255 220 50 / 0.06) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255 220 50 / 0.06) 1px, transparent 1px);
  background-size: 48px 48px;
}

/* ── Radial glow ── */
.bg-glow {
  position: absolute;
  inset: 0;
  background: radial-gradient(ellipse 70% 60% at 50% 50%, rgba(255 200 0 / 0.12) 0%, transparent 70%);
  pointer-events: none;
}

/* ── Main content ── */
.content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
  text-align: center;
  padding: 2rem;
  animation: fade-up 0.7s cubic-bezier(0.22, 1, 0.36, 1) both;
}

@keyframes fade-up {
  from { opacity: 0; transform: translateY(32px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ── Logo row ── */
.logo-wrap {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.logo-icon {
  font-size: 3.5rem;
  line-height: 1;
  filter: drop-shadow(0 0 18px rgba(255 200 0 / 0.7));
  animation: pulse-glow 2.4s ease-in-out infinite;
}

@keyframes pulse-glow {
  0%, 100% { filter: drop-shadow(0 0 14px rgba(255 200 0 / 0.6)); }
  50%       { filter: drop-shadow(0 0 28px rgba(255 200 0 / 0.95)); }
}

.heading {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(4rem, 14vw, 8rem);
  letter-spacing: 0.04em;
  line-height: 1;
  color: #ffd200;
  text-shadow:
    0 0 40px rgba(255 210 0 / 0.4),
    4px 4px 0 #3a2d00,
    8px 8px 0 rgba(0 0 0 / 0.4);
}

/* ── Tagline ── */
.tagline {
  font-size: clamp(0.9rem, 2.5vw, 1.15rem);
  font-weight: 400;
  color: rgba(255 255 255 / 0.55);
  letter-spacing: 0.02em;
  max-width: 34ch;
  animation: fade-up 0.7s 0.15s cubic-bezier(0.22, 1, 0.36, 1) both;
}

/* ── Play button ── */
.play-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
  margin-top: 0.5rem;
  padding: 0.9rem 2.6rem;
  background: #ffd200;
  color: #0a0a0f;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-family: 'DM Sans', sans-serif;
  font-size: 1.1rem;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  box-shadow:
    0 0 0 0 rgba(255 210 0 / 0.5),
    0 8px 24px rgba(0 0 0 / 0.4);
  transition:
    transform 0.18s ease,
    box-shadow 0.18s ease,
    background 0.18s ease;
  animation: fade-up 0.7s 0.28s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.play-btn:hover {
  background: #ffe44d;
  transform: translateY(-3px) scale(1.03);
  box-shadow:
    0 0 0 6px rgba(255 210 0 / 0.18),
    0 14px 32px rgba(0 0 0 / 0.45);
}

.play-btn:active {
  transform: translateY(0) scale(0.98);
  box-shadow: 0 4px 12px rgba(0 0 0 / 0.35);
}

.play-btn__arrow {
  font-size: 1.25rem;
  transition: transform 0.18s ease;
}

.play-btn:hover .play-btn__arrow {
  transform: translateX(4px);
}
</style>