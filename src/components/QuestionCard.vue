<template>
  <div class="question-card">
    <div class="question-card__header">
      <p class="question-text">{{ question.question }}</p>
    </div>

    <div class="answers-grid">
      <button
        v-for="(answer, index) in question.answers"
        :key="index"
        class="answer-btn"
        :class="getAnswerClass(index)"
        :disabled="answered"
        @click="handleAnswer(index)"
      >
        <span class="answer-btn__letter" aria-hidden="true">{{ letters[index] }}</span>
        <span class="answer-btn__text">{{ answer }}</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionCard',

  props: {
    question: {
      type: Object,
      required: true,
    },
  },

  emits: ['answer'],

  data() {
    return {
      answered: false,
      selectedIndex: null,
      letters: ['A', 'B', 'C', 'D'],
    };
  },

  methods: {
    handleAnswer(index) {
      if (this.answered) return;

      this.answered = true;
      this.selectedIndex = index;

      const isCorrect = index === this.question.correct;

      setTimeout(() => {
        this.$emit('answer', isCorrect);
        this.answered = false;
        this.selectedIndex = null;
      }, 1000);
    },

    getAnswerClass(index) {
      if (this.selectedIndex === null) return '';

      if (index === this.question.correct) return 'answer-btn--correct';
      if (index === this.selectedIndex)    return 'answer-btn--wrong';

      return '';
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

.question-card {
  display: flex;
  flex-direction: column;
  gap: 1.75rem;
  width: 100%;
  max-width: 640px;
  margin: 0 auto;
  padding: 2rem;
  font-family: 'DM Sans', sans-serif;
}

/* ── Question text ── */
.question-card__header {
  padding: 1.75rem 2rem;
  border-radius: 6px;
  background: rgba(255 255 255 / 0.05);
  border: 1px solid rgba(255 255 255 / 0.1);
  animation: fade-up 0.5s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.question-text {
  font-size: clamp(1rem, 3vw, 1.2rem);
  font-weight: 500;
  line-height: 1.6;
  color: #fff;
  letter-spacing: 0.01em;
}

/* ── Answers grid ── */
.answers-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
}

@media (max-width: 480px) {
  .answers-grid {
    grid-template-columns: 1fr;
  }
}

/* ── Answer button ── */
.answer-btn {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.9rem 1.1rem;
  background: rgba(255 255 255 / 0.04);
  border: 1px solid rgba(255 255 255 / 0.12);
  border-radius: 5px;
  cursor: pointer;
  text-align: left;
  color: rgba(255 255 255 / 0.85);
  font-family: 'DM Sans', sans-serif;
  font-size: 0.95rem;
  font-weight: 400;
  line-height: 1.4;
  transition: background 0.15s ease, border-color 0.15s ease, transform 0.15s ease;
  animation: fade-up 0.5s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.answer-btn:nth-child(1) { animation-delay: 0.06s; }
.answer-btn:nth-child(2) { animation-delay: 0.12s; }
.answer-btn:nth-child(3) { animation-delay: 0.18s; }
.answer-btn:nth-child(4) { animation-delay: 0.24s; }

.answer-btn:not(:disabled):hover {
  background: rgba(255 210 0 / 0.1);
  border-color: rgba(255 210 0 / 0.45);
  transform: translateY(-2px);
}

.answer-btn:not(:disabled):active {
  transform: translateY(0);
}

.answer-btn:disabled {
  cursor: not-allowed;
  opacity: 0.7;
}

/* ── Letter badge ── */
.answer-btn__letter {
  flex-shrink: 0;
  width: 1.7rem;
  height: 1.7rem;
  display: grid;
  place-items: center;
  border-radius: 3px;
  background: rgba(255 255 255 / 0.08);
  font-family: 'Bebas Neue', sans-serif;
  font-size: 1rem;
  letter-spacing: 0.05em;
  color: rgba(255 255 255 / 0.5);
  transition: background 0.15s ease, color 0.15s ease;
}

.answer-btn__text {
  flex: 1;
}

/* ── Correct state ── */
.answer-btn--correct {
  background: rgba(50 220 120 / 0.18) !important;
  border-color: rgba(50 220 120 / 0.7) !important;
  color: #32dc78 !important;
  box-shadow: 0 0 20px rgba(50 220 120 / 0.2);
  animation: pop 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.answer-btn--correct .answer-btn__letter {
  background: rgba(50 220 120 / 0.3);
  color: #32dc78;
}

/* ── Wrong state ── */
.answer-btn--wrong {
  background: rgba(255 80 80 / 0.18) !important;
  border-color: rgba(255 80 80 / 0.7) !important;
  color: #ff5050 !important;
  box-shadow: 0 0 20px rgba(255 80 80 / 0.2);
  animation: shake 0.4s ease;
}

.answer-btn--wrong .answer-btn__letter {
  background: rgba(255 80 80 / 0.3);
  color: #ff5050;
}

/* ── Animations ── */
@keyframes fade-up {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes pop {
  0%   { transform: scale(1); }
  50%  { transform: scale(1.04); }
  100% { transform: scale(1); }
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25%       { transform: translateX(-6px); }
  75%       { transform: translateX(6px); }
}
</style>