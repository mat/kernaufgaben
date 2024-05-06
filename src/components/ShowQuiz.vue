<template>
  <div>
    <div v-if="quizStarted && !quizFinished">
      <p class="progress-text">{{ progressText }}</p>
      <p class="question-text">{{ currentQuestion }}</p>
      <form @submit.prevent="submitAnswer">
        <input v-model="userAnswer" v-focus inputmode="numeric" ref="answerInput">
        <button type="submit">weiter</button>
      </form>
    </div>
    <div v-if="quizFinished">
      <p class="progress-text">{{ progressText }}</p>
      <p>Deine Punkte: {{ score }} / 10</p>
      <ul>
        <li v-for="(result, index) in results" :key="index"
          :class="{ correct: result.correctAnswer === parseInt(result.userAnswer) }">
          {{ result.question }} = {{ result.correctAnswer }} (Deine Antwort: {{ result.userAnswer }})
        </li>
      </ul>
      <button @click="startNewQuiz">Neues Quiz</button>
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
      questions: this.generateQuestions(MULTIPLICATION_GROUPS_TEXT),
      results: []
    }
  },
  computed: {
    progressText() {
      return this.quizFinished ? 'Geschafft!' : `Aufgabe ${this.questionIndex + 1} von 10`;
    }
  },
  mounted() {
    this.startQuiz();
  },
  methods: {
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
    startQuiz() {
      this.quizStarted = true;
      this.nextQuestion();
    },
    startNewQuiz() {
      this.quizStarted = false;
      this.quizFinished = false;
      this.score = 0;
      this.questionIndex = 0;
      this.results = []; // Clear the results array
      this.questions = this.generateQuestions(MULTIPLICATION_GROUPS_TEXT);
      this.startQuiz();
    },
    generateQuestions(text) {
      const groups = text.trim().split('\n\n');
      const exercises = groups.flatMap(group => {
        const lines = group.trim().split('\n').slice(1);
        return lines.map(line => this.parseMultiplicationGroup(line.trim()));
      });

      // Use the shuffleArray helper function
      this.shuffleArray(exercises);

      // Select the first 10 exercises
      return exercises.slice(0, 10);
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
      // If the input field is blank, return immediately
      if (this.userAnswer.trim() === '') {
        return;
      }

      const correctAnswer = this.questions[this.questionIndex].result;
      if (parseInt(this.userAnswer) === correctAnswer) {
        this.score++;
      }

      // Store the result
      this.results.push({
        question: `${this.questions[this.questionIndex].num1} · ${this.questions[this.questionIndex].num2}`,
        correctAnswer,
        userAnswer: this.userAnswer
      });

      this.userAnswer = '';
      if (!this.quizFinished) {
        this.questionIndex++;
        this.nextQuestion();
        this.$refs.answerInput.focus();
      }
    }
  }
}
</script>
<style scoped>
.question-text {
  font-size: 10vw;
  /* Adjust as needed */
}

.input-field,
.submit-button {
  font-size: 5vw;
  /* Adjust as needed */
  padding: 1vw;
  width: 90%;
  /* Adjust as needed */
  margin: 1vw auto;
  display: block;
}

@media (min-width: 600px) {
  .question-text {
    font-size: 5vw;
    /* Adjust as needed */
  }

  .input-field,
  .submit-button {
    font-size: 2vw;
    /* Adjust as needed */
  }
}

@media (min-width: 900px) {
  .question-text {
    font-size: 3vw;
    /* Adjust as needed */
  }

  .input-field,
  .submit-button {
    font-size: 1vw;
    /* Adjust as needed */
  }
}

ul {
  list-style-type: none;
  /* Remove bullet points */
  padding: 0;
}

li.correct {
  color: green;
  /* Highlight correct answers */
}
</style>