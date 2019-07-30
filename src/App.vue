<template>
  <div id="app">
    <div class="results" v-if="results">
      <div>
        <p>Résultats:</p>
      </div>

      <div class="carrieres">
        <p>Nous te recommandons les carrières suivantes :</p>

        <ul>
            <li v-for="carriere in selectedAnswer.carrieres">{{ carriere }}</li>
        </ul>
      </div>

      <div class="restart">
        <button class="btn" @click="init">
          Recommencer
        </button>
      </div>
    </div>
    <div v-else>
      <div class="question">
        <p>{{ currentQuestion.intitule }}</p>
      </div>

      <div class="reponses">
        <div v-for="answer in answers" class="reponse" @click="answerQuestion(answer)">
          {{ answer.intitule }}
        </div>
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
      selectedAnswer: false,
      results: false
    }
  },

  created() {
    // On démarre l'app avec la première question.
    this.init();
  },

  methods: {
    init() {
      this.reset();
      this.showQuestion(0);
    },

    showQuestion(id) {
      this.currentQuestion = this.questions.filter(q => q.id === id)[0]; //peut-être un meilleur moyen de récupérer la question ?
      this.answers = this.currentQuestion.reponses;
    },

    answerQuestion(answer) {
      this.selectedAnswer = answer;
      if(this.selectedAnswer.hasOwnProperty('question_suivante')) {
        this.showQuestion(this.selectedAnswer.question_suivante);
      } else {
        this.showResults();
      }
    },

    showResults() {
      this.results = true;
    },

    reset() {
      this.answers = [];
      this.currentQuestion = false;
      this.selectedAnswer = false;
      this.results = false;
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
  height: 100%;
}

  .question {
    display: flex;
    align-items: center;
    text-align: center;
  }

  .question p {
    margin: auto;
    font-size: xx-large;
    padding-bottom: 6rem;
  }

  .reponses {
    display: flex;
    justify-content: space-around;
  }

  .reponse {
    width: 400px;
    height: 250px;
    background-color: #171738;
    color: #8ef9f3;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    transition: background-color 1s;
    font-size: x-large;
    border-radius: 25px;
  }

  .reponse:hover {
    background-color: #db5461;
    cursor: pointer;
  }

  .btn {
    display: inline-block;
    padding: 0.46em 1.6em;
    border: 0.1em solid #8EF9F3;
    background-color: #593C8F;
    color: #8EF9F3;
  }
</style>
