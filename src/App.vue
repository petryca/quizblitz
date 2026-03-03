<template>
  <div style="min-height: 100vh; background: #0a0a0f; display: flex; align-items: center; justify-content: center;">
    <div style="width: 100%; max-width: 640px;">
      <p style="color: rgba(255,255,255,0.4); font-family: sans-serif; text-align: center; margin-bottom: 1rem;">
        Last answer: <strong style="color: #ffd200;">{{ lastResult }}</strong>
      </p>
      <QuestionCard :question="testQuestion" @answer="handleAnswer" />
    </div>
  </div>
</template>

<script>
import QuestionCard from './components/QuestionCard.vue';

export default {
  name: 'App',
  components: { QuestionCard },
  data() {
    return {
      lastResult: '—',
      testQuestion: {
        question: 'Which planet is known as the Red Planet?',
        answers: ['Venus', 'Mars', 'Jupiter', 'Saturn'],
        correct: 1,
      },
    };
  },
  methods: {
    handleAnswer(isCorrect) {
      this.lastResult = isCorrect ? '✅ Correct' : '❌ Wrong';
    },
  },
};
</script>