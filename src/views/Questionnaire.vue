<template>
  <div class="Questionnaire">
    <h1>La page du questionnaire</h1>
    <div>
      <question :question="questions[currentQuestion]"></question>
      <button v-if="!derniereQuestion()" v-on:click="questSuivante()">Suivante</button>
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
      questions: [
        {
          id: "1",
          intitule: "test",
          choix: [
            {
              id: 1,
              libelle: "Oui"
              },
            {
              id: 2,
              libelle: "Non"
            },
            {
              id: 3,
              libelle: "Peut-etre 🤷‍♂️"
            }
          ],
          reponse: []
        },
        {
          id: "2",
          intitule: "test2",
          choix: [{
              id: 1,
              libelle: "Oui"
              },
            {
              id: 2,
              libelle: "Non"
            }],
          reponse: []
        }
      ]
    };
  },
  methods: {
    questSuivante() {
      return this.currentQuestion++;
    },

    derniereQuestion(){
      if(this.currentQuestion < this.questions.length-1 ){
        return false
      }else{
        return true
      }
    }
  },
};
</script>
