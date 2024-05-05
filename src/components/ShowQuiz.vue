<template>
  <div>
    <button @click="startQuiz">Start Quiz</button>
    <div v-if="quizStarted">
      <p>{{ currentQuestion }}</p>
      <input v-model="userAnswer">
      <button @click="submitAnswer">Submit</button>
    </div>
    <div v-if="quizFinished">
      <p>Deine Punkte: {{ score }} / 10. Glückwunsch!</p>
    </div>
  </div>
</template>

<script>
import { MULTIPLICATION_GROUPS_TEXT } from './constants.js';

export default {
  data() {
    return {
      quizStarted: false,
      quizFinished: false,
      currentQuestion: '',
      userAnswer: '',
      score: 0,
      questionIndex: 0,
      questions: this.generateQuestions(MULTIPLICATION_GROUPS_TEXT)
    }
  },
  methods: {
    startQuiz() {
      this.quizStarted = true;
      this.nextQuestion();
    },
    generateQuestions(text) {
      const groups = text.trim().split('\n\n');
      const exercises = groups.flatMap(group => {
        const lines = group.trim().split('\n').slice(1);
        return lines.map(line => this.parseMultiplicationGroup(line.trim()));
      });
      // Randomly select 10 exercises
      return exercises.sort(() => Math.random() - 0.5).slice(0, 10);
    },
    parseMultiplicationGroup(line) {
      const parts = line.split(' = ');
      const result = parseInt(parts[1], 10);

      const operands = parts[0].split(' x ');
      const num1 = parseInt(operands[0], 10);
      const num2 = parseInt(operands[1], 10);

      return { num1, num2, result };
    },
    nextQuestion() {
      if (this.questionIndex < 10) {
        this.currentQuestion = `${this.questions[this.questionIndex].num1} · ${this.questions[this.questionIndex].num2} = ?`;
      } else {
        this.quizFinished = true;
      }
    },
    submitAnswer() {
      if (parseInt(this.userAnswer) === this.questions[this.questionIndex].result) {
        this.score++;
      }
      this.userAnswer = '';
      this.questionIndex++;
      this.nextQuestion();
    }
  }
}
</script>