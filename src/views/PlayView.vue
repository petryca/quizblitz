<template>
  <div class="play-view">
    <div class="bg-grid" aria-hidden="true"></div>
    <div class="bg-glow" aria-hidden="true"></div>

    <!-- GAME -->
    <template v-if="gameState === 'playing'">
      <div class="game-ui">

        <!-- HUD -->
        <header class="hud">
          <div class="hud__brand">⚡ QuizBlitz</div>
          <div class="hud__center">
            <div class="progress-bar">
              <div
                class="progress-bar__fill"
                :style="{ width: progressPercent + '%' }"
              ></div>
            </div>
            <span class="hud__count">{{ currentIndex + 1 }} / {{ questions.length }}</span>
          </div>
          <div class="hud__score">
            <span class="hud__score-label">Score</span>
            <span class="hud__score-value">{{ score }}</span>
          </div>
        </header>

        <!-- Question -->
        <transition name="slide" mode="out-in">
          <QuestionCard
            :key="currentIndex"
            :question="currentQuestion"
            @answer="handleAnswer"
          />
        </transition>

        <!-- Toast feedback -->
        <transition name="toast">
          <div
            v-if="toastVisible"
            class="toast"
            :class="toastCorrect ? 'toast--correct' : 'toast--wrong'"
          >
            {{ toastCorrect ? '✓ Correct!' : '✗ Wrong!' }}
          </div>
        </transition>

      </div>
    </template>

    <!-- END -->
    <ScoreBoard
      v-else-if="gameState === 'end'"
      :score="score"
      @restart="goHome"
    />
  </div>
</template>

<script>
import QuestionCard from '@/components/QuestionCard.vue';
import ScoreBoard   from '@/components/ScoreBoard.vue';

export default {
  name: 'PlayView',

  components: { QuestionCard, ScoreBoard },

  data() {
    return {
      gameState:    'playing',  // 'playing' | 'end'
      currentIndex: 0,
      score:        0,
      toastVisible: false,
      toastCorrect: false,

      // ── Fill in your questions here ──
      questions: [
        {
          question: 'What is the chemical symbol for gold?',
          answers: ['Go', 'Gd', 'Au', 'Ag'],
          correct: 2,
        },
        {
          question: 'How many bones are in the adult human body?',
          answers: ['186', '206', '216', '226'],
          correct: 1,
        },
        {
          question: 'Which planet has the most moons?',
          answers: ['Jupiter', 'Saturn', 'Uranus', 'Neptune'],
          correct: 1,
        },
        {
          question: 'What is the speed of light in a vacuum (km/s)?',
          answers: ['199,792', '299,792', '399,792', '499,792'],
          correct: 1,
        },
        {
          question: 'In which year did the Berlin Wall fall?',
          answers: ['1987', '1988', '1989', '1991'],
          correct: 2,
        },
        {
          question: 'Which element has the atomic number 1?',
          answers: ['Helium', 'Hydrogen', 'Lithium', 'Carbon'],
          correct: 1,
        },
        {
          question: 'What is the largest ocean on Earth?',
          answers: ['Atlantic', 'Indian', 'Arctic', 'Pacific'],
          correct: 3,
        },
        {
          question: 'Who painted the Sistine Chapel ceiling?',
          answers: ['Leonardo da Vinci', 'Raphael', 'Michelangelo', 'Donatello'],
          correct: 2,
        },
        {
          question: 'What is the square root of 144?',
          answers: ['11', '12', '13', '14'],
          correct: 1,
        },
        {
          question: 'Which country has the longest coastline in the world?',
          answers: ['Russia', 'Australia', 'Norway', 'Canada'],
          correct: 3,
        }
      ],
    };
  },

  computed: {
    currentQuestion() {
      return this.questions[this.currentIndex];
    },

    progressPercent() {
      return (this.currentIndex / this.questions.length) * 100;
    },
  },

  mounted() {
    this.startGame();
  },

  methods: {
    startGame() {
      this.currentIndex = 0;
      this.score        = 0;
      this.gameState    = 'playing';
    },

    handleAnswer(isCorrect) {
      if (isCorrect) this.score++;

      this.toastCorrect = isCorrect;
      this.toastVisible = true;
      setTimeout(() => { this.toastVisible = false; }, 900);

      if (this.currentIndex < this.questions.length - 1) {
        this.currentIndex++;
      } else {
        setTimeout(() => { this.gameState = 'end'; }, 400);
      }
    },

    goHome() {
      this.$router.push({ name: 'home' });
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@400;500&display=swap');

.play-view {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #0a0a0f;
  overflow: hidden;
  font-family: 'DM Sans', sans-serif;
}

/* ── Background ── */
.bg-grid {
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(255 220 50 / 0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255 220 50 / 0.05) 1px, transparent 1px);
  background-size: 48px 48px;
  pointer-events: none;
  z-index: 0;
}

.bg-glow {
  position: fixed;
  inset: 0;
  background: radial-gradient(ellipse 70% 60% at 50% 50%, rgba(255 200 0 / 0.08) 0%, transparent 70%);
  pointer-events: none;
  z-index: 0;
}

/* ── Game UI ── */
.game-ui {
  position: relative;
  z-index: 1;
  width: 100%;
  max-width: 680px;
  padding: 1.5rem 1rem 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

/* ── HUD ── */
.hud {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 0.75rem 1.25rem;
  background: rgba(255 255 255 / 0.04);
  border: 1px solid rgba(255 255 255 / 0.08);
  border-radius: 6px;
  backdrop-filter: blur(8px);
}

.hud__brand {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 1.3rem;
  letter-spacing: 0.06em;
  color: #ffd200;
  white-space: nowrap;
}

.hud__center {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
}

.progress-bar {
  width: 100%;
  max-width: 260px;
  height: 4px;
  background: rgba(255 255 255 / 0.1);
  border-radius: 99px;
  overflow: hidden;
}

.progress-bar__fill {
  height: 100%;
  background: #ffd200;
  border-radius: 99px;
  transition: width 0.4s cubic-bezier(0.22, 1, 0.36, 1);
  box-shadow: 0 0 8px rgba(255 210 0 / 0.6);
}

.hud__count {
  font-size: 0.72rem;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: rgba(255 255 255 / 0.35);
}

.hud__score {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  white-space: nowrap;
}

.hud__score-label {
  font-size: 0.65rem;
  font-weight: 500;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(255 255 255 / 0.35);
}

.hud__score-value {
  font-family: 'Bebas Neue', sans-serif;
  font-size: 1.6rem;
  line-height: 1;
  color: #ffd200;
}

/* ── Question slide transition ── */
.slide-enter-active,
.slide-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.slide-enter-from { opacity: 0; transform: translateX(32px); }
.slide-leave-to   { opacity: 0; transform: translateX(-32px); }

/* ── Toast ── */
.toast {
  position: fixed;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  padding: 0.65rem 1.6rem;
  border-radius: 99px;
  font-size: 0.95rem;
  font-weight: 500;
  letter-spacing: 0.04em;
  pointer-events: none;
  z-index: 100;
}

.toast--correct {
  background: rgba(50 220 120 / 0.2);
  border: 1px solid rgba(50 220 120 / 0.6);
  color: #32dc78;
  box-shadow: 0 0 24px rgba(50 220 120 / 0.2);
}

.toast--wrong {
  background: rgba(255 80 80 / 0.2);
  border: 1px solid rgba(255 80 80 / 0.5);
  color: #ff5050;
  box-shadow: 0 0 24px rgba(255 80 80 / 0.15);
}

.toast-enter-active,
.toast-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.toast-enter-from,
.toast-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(10px);
}
</style>