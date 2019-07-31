    <template>
      <div id="app">
        <div class="results" v-if="results">
          <div>
              <p>Nous te recommandons les carrières suivantes :</p>
          </div>

          <div class="carrieres">
              <div class="carriere" v-for="carriere in selectedCareers" v-bind:key="carriere.id">
                  <div class="carriere-header">
                      <img v-bind:src="carriere.image"/>
                      <h2>{{ carriere.nom }}</h2>
                  </div>
                  <div class="carriere-info">
                      <p>{{ trimText(carriere.description) }}</p>
                      <a v-bind:href="carriere.url_regle">Lien vers les règles</a>
                  </div>
              </div>
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
            <div v-for="answer in answers" class="reponse" @click="answerQuestion(answer)" v-bind:key="answer.id">
              {{ answer.intitule }}
            </div>
          </div>
        </div>
      </div>
    </template>

    <script>
    import questions_json from './data/questions.json';
    import carrieres_json from './data/carrieres.json';
    export default {
      name: 'app',

      data() {
        return {
          questions: questions_json,
          carrieres: carrieres_json,
          answers: [],
          currentQuestion: false,
          selectedAnswer: false,
          selectedCareers: [],
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

        fetchCareers() {
            let carrieres = this.carrieres.filter(
                function(c) {
                    return this.indexOf(c.id) > 0;
                },
                this.selectedAnswer.carrieres
            );
            return carrieres;
        },

        answerQuestion(answer) {
          this.selectedAnswer = answer;
          //La réponse choisie possède une "question_suivante", on la charge.
          if(this.selectedAnswer.hasOwnProperty('question_suivante')) {
            this.showQuestion(this.selectedAnswer.question_suivante);
          } else {
              //Pas de question_suivante on a donc les résultats finaux.
            this.showResults();
          }
        },

        showResults() {
          this.results = true;
          this.selectedCareers = this.fetchCareers();
        },

        reset() {
          this.answers = [];
          this.currentQuestion = false;
          this.selectedAnswer = false;
          this.results = false;
          this.selectedCareers = [];
        },

          trimText(text) {
            return text.substring(0, 255) + "...";
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
      margin-top:50px;
      padding: 0.46em 1.6em;
      border: 0.1em solid #8EF9F3;
      background-color: #593C8F;
      color: #8EF9F3;
      cursor: pointer;
    }

    .carrieres {
      display: flex;
      justify-content: space-around;
      flex-wrap: wrap;
    }

    .carriere {
      background-color: #171738;
      width: 300px;
      display: inline-block;
      margin: auto;
      border-radius: 5px;
      position: relative;
      text-align: center;
      box-shadow: -1px 15px 30px -12px black;
      color: #8ef9f3;
      height:420px;
    }

    .carriere-header {
      max-height: 190px;
      text-align: center;
    }

    .carriere-header .carriere-image {
      position: relative;
      height: 130px;
      margin-bottom: 35px;
      overflow:hidden;
    }

    .carriere-header h2 {
      font-size: 26px;
      font-weight: 900;
      margin-bottom: 5px;
    }

    .carriere-info {
      padding: 20px;
      margin-bottom: 10px;  
    }

    .carriere-info p {
      text-align: left;
    }

    .carriere-info a {
      display: inline-block;
      margin-right: 3px;
      width: 100%;
      text-align: right;
      color: #db5461;
      text-underline: none;
      cursor:pointer;
    }

    .carriere-info a:hover {
      color: #8EF9F3;
    }
  </style>
