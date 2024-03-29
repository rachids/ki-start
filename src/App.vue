<template>
    <div id="app">
        <div class="results" v-if="results">
            <div class="header">
                <p>Nous vous recommandons les carrières suivantes :</p>
            </div>

            <div class="carrieres">
                <div class="carriere" v-for="carriere in selectedCareers" v-bind:key="carriere.id">
                    <div class="carriere-image">
                        <img v-bind:src="carriere.image"/>
                    </div>
                    <div class="carriere-info">
                        <h2>{{ carriere.nom }}</h2>
                        <p>{{ trimText(carriere.description) }}</p>
                        <a v-bind:href="carriere.url_regle">Lien vers les règles</a>
                    </div>
                </div>
            </div>

            <div class="retour">
                <button class="btn" @click="goBack(currentQuestion.id)">
                    Retour
                </button>
                <button class="btn" @click="init">
                    Recommencer
                </button>
            </div>
        </div>
        <div class="questions" v-else>
            <div class="question">
                <p>{{ currentQuestion.intitule }}</p>
            </div>

            <div class="reponses">
                <div v-for="answer in answers" class="reponse" @click="answerQuestion(answer)" v-bind:key="answer.id">
                    {{ answer.intitule }}
                </div>
            </div>

            <div class="retour" v-if="previousQuestion">
                <button class="btn" @click="goBack(previousQuestion)">
                    Retour
                </button>
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

        computed: {
            previousQuestion() {
                if(this.currentQuestion.hasOwnProperty("question_precedente")) {
                    return this.currentQuestion.question_precedente;
                }

                return false;
            },
        },

        methods: {
            init() {
                this.reset();
                this.showQuestion(1);
            },

            showQuestion(id) {
                this.currentQuestion = this.questions.filter(q => q.id === id)[0]; //peut-être un meilleur moyen de récupérer la question ?
                this.answers = this.currentQuestion.reponses;
            },

            fetchCareers() {
                return this.carrieres.filter(
                    function(c) {
                        return this.indexOf(c.id) > 0;
                    },
                    this.selectedAnswer.carrieres
                );
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

            goBack(question) {
                this.results = false;
                this.showQuestion(question);
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

    .questions {
        display:flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: space-around;
        height:100%;
    }

    .question {
        flex: 1 0 100%;
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
        width: 100%;
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

    .retour {
        align-self: flex-end;
        margin-top: 30px;
    }

    .btn {
        display: inline-block;
        padding: 1em 2em;
        border: 0.1em solid #8EF9F3;
        background-color: #593C8F;
        color: #8EF9F3;
        transition: background-color 1s;
        cursor: pointer;
        font-size: 0.95em;
    }

    .btn:hover {
        background-color: #db5461;
    }

    /*Résultats*/
    .header {
        font-size: xx-large;
    }

    .carrieres {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
    }

    .carriere {
        margin: 5px;
        width: 400px;
        height: 40vh;
        background-color: #171738;
        color: #8ef9f3;
        display: flex;
        border-radius: 25px;
    }

    .carriere-image {
        width: 120px;
        height: 100%;
        justify-self: center;
    }

    .carriere-image img {
        display: block;
        width: 120px;
        height: 160px;
    }

    .carriere .carriere-info{
        flex-grow: 5;
        height: 100%;
        padding-left: 5px;
    }

    .carriere-info h2 {

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
    }

    .carriere-info a:hover {
        color: #8EF9F3;
    }
</style>
