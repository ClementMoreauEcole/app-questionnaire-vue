<template>
  <div class="Questionnaire">
    <div>
      <question v-if="!voirMesResultat" :question="questions[currentQuestion]"></question>
      <div v-if="voirMesResultat">
        <h1>Mes résultats</h1>
        <div v-for="question in questions">
          <h1>Question {{question.id}}</h1>
          <h2>{{question.intitule}}</h2>
          <div v-for="lechoix in question.choix">
            <p>{{lechoix.libelle}} </p>
          </div>
          <h2>Vous avez choisi : {{question.reponse}}</h2>
        </div>
      </div>
      <button v-if="!derniereQuestion() && !voirMesResultat" v-on:click="questSuivante()">Suivante</button>
      <button v-if="!premiereQuestion() && !voirMesResultat" v-on:click="questionPrecedente()">Precedente</button>
      <button v-if="derniereQuestion()" v-on:click="finQuestionnaire()">Mes résultats</button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import question from "../components/Question.vue";

export default {
  name: "Questionnaire",
  components: {
    question
  },
  data() {
    return {
      currentQuestion: 0,
      voirMesResultat: false,
      questions: [
        {
          id: "1",
          intitule: "Ou mangez vous le plus souvent ?",
          choix: [
            {
              id: 1,
              libelle: "Macdo 🍔"
            },
            {
              id: 2,
              libelle: "KFC 🐔"
            },
            {
              id: 3,
              libelle: "Burger King 👑"
            }
          ],
          reponse: [],
          bonneReponse: []
        },
        {
          id: "2",
          intitule: "Quel est votre jeux favoris ?",
          choix: [
            {
              id: 1,
              libelle: "Apex Legends 🐱‍🏍"
            },
            {
              id: 2,
              libelle: "Fortnite 👶"
            }
          ],
          reponse: [],
          bonneReponse: []
        }
      ]
    };
  },
  methods: {
    questSuivante() {
      return this.currentQuestion++;
    },
    questionPrecedente() {
      return this.currentQuestion--;
    },
    derniereQuestion() {
      if (this.currentQuestion < this.questions.length - 1) {
        return false;
      } else {
        return true;
      }
    },
    premiereQuestion() {
      if (this.currentQuestion == 0) {
        return true;
      } else {
        return false;
      }
    },
    finQuestionnaire() {
      this.voirMesResultat = true;
    }
  }
};
</script>
