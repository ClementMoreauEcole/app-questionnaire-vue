<template>
  <div class="Questionnaire">
    <div>
      <div>
        <input v-if="!voirQuestion" v-model="nom" type="text" placeholder="Entrez votre nom">
        <input v-if="!voirQuestion" v-model="prenom" type="text" placeholder="Entrez votre prénom"> 
        <input v-if="!voirQuestion" v-model="societe" type="text" placeholder="Entrez votre société"> 
        <button v-if="!voirQuestion" v-on:click="dsplQuestion()">Aller aux questions !</button>
      </div>
      
      <question v-if="!voirMesResultat && voirQuestion" :question="questions[currentQuestion]"></question>

      <div v-if="voirMesResultat && voirQuestion">
        <h1>Nom : {{nom}}, Prenom: {{prenom}}, Societe: {{societe}}</h1>
        <h1>Mes résultats</h1>
       
        <div v-for="question in questions">
          <h1>Question {{question.id}}</h1>
          <h2>{{question.intitule}}</h2>
          <div v-for="lechoix in question.choix">
            <p>{{lechoix.libelle}} </p>
          </div>
          <h2>Vous avez choisi : {{question.reponse}}</h2>
          
          <h2 v-if="question.etatReponse" style="color: green;">Bonne réponse !</h2>

          
        </div>
         <h2>Vous avez {{nbBonneReponse}} bonne réponse</h2>
      </div>


      <button v-if="!derniereQuestion() && !voirMesResultat && voirQuestion" v-on:click="questSuivante()">Suivante</button>
      <button v-if="!premiereQuestion() && !voirMesResultat && voirQuestion" v-on:click="questionPrecedente()">Precedente</button>
      <button v-if="derniereQuestion() && voirQuestion" v-on:click="finQuestionnaire()">Mes résultats</button>
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
      nom: null,
      prenom: null,
      societe: null,
      voirQuestion: false,
      currentQuestion: 0,
      voirMesResultat: false,
      nbBonneReponse: 0,
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
          bonneReponse: ["Macdo 🍔","Burger King 👑"],
          etatReponse: false
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
          bonneReponse: ["Fortnite 👶"],
          etatReponse: false
        }
      ]
    };
  },
  methods: {
    questSuivante() {
      return this.currentQuestion++
    },
    questionPrecedente() {
      return this.currentQuestion--
    },
    derniereQuestion() {
      if (this.currentQuestion < this.questions.length - 1) {
        return false
      } else {
        return true
      }
    },
    premiereQuestion() {
      if (this.currentQuestion == 0) {
        return true
      } else {
        return false
      }
    },
    finQuestionnaire() {
      console.log("fin questionaire")
      for(var i = 0; i<this.questions.length; i++){
          var nbReponse = this.questions[i].reponse.length
          console.log("nbReponse : " + nbReponse)
          console.log("Nb reponse attendue : " + this.questions[i].bonneReponse.length)
          var nbReponseBonne = 0
          if(nbReponse == this.questions[i].bonneReponse.length){
            for( var j = 0; j<this.questions[i].bonneReponse.length; j++){
              console.log("meme nombre")
              console.log("Bonne reponse : " +this.questions[i].bonneReponse[j])
              for( var h = 0; h<this.questions[i].reponse.length; h++){
                console.log("boucle h")
                console.log("Reponse : " +this.questions[i].reponse[h])
                if(this.questions[i].reponse[j] == this.questions[i].bonneReponse[h]){
                  console.log("compteur ++ ")
                  nbReponseBonne++;
                }
              }
            }
            if(nbReponse == nbReponseBonne){
            console.log("on passe")
            this.questions[i].etatReponse = true
            this.nbBonneReponse++
          }
          
          }
          
          
          
        }
      
      this.voirMesResultat = true
    },

    dsplQuestion(){
      this.voirQuestion = true
    },
  }
};
</script>
