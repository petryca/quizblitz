<template>
  <div class="scoreboard">
    <div class="bg-grid" aria-hidden="true"></div>

    <main class="content">
      <div class="game-over-wrap">
        <span class="skull" aria-hidden="true">💀</span>
        <h1 class="heading">Game Over</h1>
      </div>

      <div class="score-card" :class="scoreClass">
        <span class="score-label">Your Score</span>
        <div class="score-display">
          <span class="score-value">{{ score }}</span>
          <span class="score-denom">/10</span>
        </div>
        <span class="score-verdict">{{ verdict }}</span>
      </div>

      <button class="restart-btn" @click="handleRestart">
        <span class="restart-btn__label">Play Again</span>
        <span class="restart-btn__icon" aria-hidden="true">↺</span>
      </button>
    </main>
  </div>
</template>

<script>
export default {
  name: 'ScoreBoard',

  props: {
    score: {
      type: Number,
      required: true,
    },
  },

  emits: ['restart'],

  computed: {
    scoreClass() {
      if (this.score >= 8) return 'score-card--great';
      if (this.score >= 5) return 'score-card--ok';
      return 'score-card--poor';
    },

    verdict() {
      if (this.score === 10) return 'Perfect. Unstoppable. ⚡';
      if (this.score >= 8)  return 'Outstanding effort!';
      if (this.score >= 5)  return 'Not bad — keep pushing.';
      if (this.score >= 3)  return 'Room to grow. Try again!';
      return 'Rough round. Bounce back!';
    },
  },

  methods: {
    handleRestart() {
      this.$emit('restart');
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

.scoreboard {
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
    linear-gradient(rgba(255 220 50 / 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255 220 50 / 0.05) 1px, transparent 1px);
  background-size: 48px 48px;
}

/* ── Content ── */
.content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  padding: 2rem;
  text-align: center;
  animation: fade-up 0.65s cubic-bezier(0.22, 1, 0.36, 1) both;
}

@keyframes fade-up {
  from { opacity: 0; transform: translateY(28px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ── Game Over heading ── */
.game-over-wrap {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.skull {
  font-size: 2.8rem;
  line-height: 1;
  animation: shake 2.8s ease-in-out infinite;
}

@keyframes shake {
  0%, 90%, 100% { transform: rotate(0deg); }
  92%            { transform: rotate(-8deg); }
  96%            { transform: rotate(8deg); }
}

.heading {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3.5rem, 12vw, 6.5rem);
  letter-spacing: 0.05em;
  line-height: 1;
  color: #fff;
  text-shadow:
    4px 4px 0 rgba(0 0 0 / 0.5),
    0 0 40px rgba(255 255 255 / 0.08);
}

/* ── Score card ── */
.score-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  padding: 2rem 3.5rem;
  border-radius: 6px;
  border: 1px solid rgba(255 255 255 / 0.08);
  background: rgba(255 255 255 / 0.04);
  backdrop-filter: blur(8px);
  animation: fade-up 0.65s 0.12s cubic-bezier(0.22, 1, 0.36, 1) both;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.score-card--great {
  border-color: rgba(50 220 120 / 0.45);
  box-shadow: 0 0 40px rgba(50 220 120 / 0.12);
}

.score-card--ok {
  border-color: rgba(255 210 0 / 0.45);
  box-shadow: 0 0 40px rgba(255 210 0 / 0.1);
}

.score-card--poor {
  border-color: rgba(255 80 80 / 0.4);
  box-shadow: 0 0 40px rgba(255 80 80 / 0.1);
}

.score-label {
  font-size: 0.75rem;
  font-weight: 500;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(255 255 255 / 0.4);
}

.score-display {
  display: flex;
  align-items: baseline;
  gap: 0.2rem;
}

.score-value {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(5rem, 18vw, 8rem);
  line-height: 1;
  color: #ffd200;
  text-shadow: 0 0 30px rgba(255 210 0 / 0.45);
}

/* colour-code the number by performance tier */
.score-card--great .score-value { color: #32dc78; text-shadow: 0 0 30px rgba(50 220 120 / 0.5); }
.score-card--poor  .score-value { color: #ff5050; text-shadow: 0 0 30px rgba(255 80 80 / 0.45); }

.score-denom {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(2rem, 6vw, 3rem);
  color: rgba(255 255 255 / 0.3);
}

.score-verdict {
  font-size: 0.95rem;
  font-weight: 400;
  color: rgba(255 255 255 / 0.55);
  letter-spacing: 0.02em;
}

/* ── Play Again button ── */
.restart-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.55rem;
  padding: 0.9rem 2.6rem;
  background: #ffd200;
  color: #0a0a0f;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-family: 'DM Sans', sans-serif;
  font-size: 1.05rem;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  box-shadow: 0 8px 24px rgba(0 0 0 / 0.4);
  transition: transform 0.18s ease, box-shadow 0.18s ease, background 0.18s ease;
  animation: fade-up 0.65s 0.24s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.restart-btn:hover {
  background: #ffe44d;
  transform: translateY(-3px) scale(1.03);
  box-shadow:
    0 0 0 6px rgba(255 210 0 / 0.18),
    0 14px 32px rgba(0 0 0 / 0.45);
}

.restart-btn:active {
  transform: translateY(0) scale(0.97);
  box-shadow: 0 4px 12px rgba(0 0 0 / 0.35);
}

.restart-btn__icon {
  font-size: 1.2rem;
  transition: transform 0.35s ease;
}

.restart-btn:hover .restart-btn__icon {
  transform: rotate(-45deg);
}
</style>