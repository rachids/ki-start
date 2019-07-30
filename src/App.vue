<template>
  <div id="app">
    <p class="question">{{ currentQuestion.intitule }}</p>

    <div>
      <div v-for="answer in answers">
        {{ answer.intitule }}
      </div>
    </div>
  </div>
</template>

<script>
import questions_json from './data/questions.json';

export default {
  name: 'app',

  data() {
    return {
      questions: questions_json,
      answers: [],
      currentQuestion: false,
      selectedAnswer: false
    }
  },

  created() {
    this.showQuestion(0);
  },

  methods: {
    showQuestion(id) {
      this.currentQuestion = this.questions.filter(q => q.id === id)[0];
      this.answers = this.currentQuestion.reponses;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
