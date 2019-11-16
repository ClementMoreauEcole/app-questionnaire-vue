<template>
  <div class="Questionnaire">
    <div>
      <div>
        <!-- Les 3 balises input pour entrer le nom/prénom/societe -->
        <input v-if="!voirQuestion" v-model="nom" type="text" placeholder="Entrez votre nom">
        <input v-if="!voirQuestion" v-model="prenom" type="text" placeholder="Entrez votre prénom"> 
        <input v-if="!voirQuestion" v-model="societe" type="text" placeholder="Entrez votre société"> 
        <button v-if="!voirQuestion" v-on:click="dsplQuestion()">Aller aux questions !</button>
      </div>
      <!-- Le composant question qui affiche les questions -->
      <question v-if="!voirMesResultat && voirQuestion" :question="questions[currentQuestion]"></question>
      
      <!-- 
        Partie résultat
        Affiche le nom/Prenom/societe
       -->
      <div v-if="voirMesResultat && voirQuestion">
        <h1>Nom : {{nom}}, Prenom: {{prenom}}, Societe: {{societe}}</h1>
        <h1>Mes résultats</h1>
       <!-- Boucle sur chaque questions afin d'y afficher les données souhaitées -->
        <div v-for="question in questions">
          <h1>Question {{question.id}}</h1>
          <h2>{{question.intitule}}</h2>
          <div v-for="lechoix in question.choix">
            <p>{{lechoix.libelle}} </p>
          </div>
          <h2>Vous avez choisi : {{question.reponse}}</h2>
          <!-- Affiche un message en vert si il s'agit d'une bonne réponse -->
          <h2 v-if="question.etatReponse" style="color: green;">Bonne réponse !</h2>

          
        </div>
        <!-- Affiche le nombre de bonne réponse -->
         <h2>Vous avez {{nbBonneReponse}} bonne réponse</h2>
      </div>

      <!-- Les bouton permettant le changement de question ainsi que l'affichage des réponses -->
      <button v-if="!derniereQuestion() && !voirMesResultat && voirQuestion" v-on:click="questSuivante()">Suivante</button>
      <button v-if="!premiereQuestion() && !voirMesResultat && voirQuestion" v-on:click="questionPrecedente()">Precedente</button>
      <button v-if="derniereQuestion() && voirQuestion" v-on:click="finQuestionnaire()">Mes résultats</button>
    </div>
  </div>
</template>

<script>
import question from "../components/Question.vue";
import pouchdb from "pouchdb";

export default {
  name: "Questionnaire",
  components: {
    question
  },
  data() {
    return {
      // Scruture des données utilisées dans le code ( je n'ai pas eux le temps de faire un fichier json séparer)
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
    // Permet de passer a la question suivante
    questSuivante() {
      return this.currentQuestion++
    },
    // Permet de revenir a la question precedente 
    questionPrecedente() {
      return this.currentQuestion--
    },
    // Permet de savoir si la question courrante est bien la dernière question
    derniereQuestion() {
      if (this.currentQuestion < this.questions.length - 1) {
        return false
      } else {
        return true
      }
    },
    // Permet de savoir si la question courrante est bien la premiere question
    premiereQuestion() {
      if (this.currentQuestion == 0) {
        return true
      } else {
        return false
      }
    },
    // Permet de calculer le nombre de bonne réponses et d'afficher la page des résultat
    finQuestionnaire() {
      var db = new pouchdb('dbVueJS')

      for(var i = 0; i<this.questions.length; i++){
        // Recupere le nombre de réponses de l'utilisateur
          var nbReponse = this.questions[i].reponse.length
          var nbReponseBonne = 0
          // Effectue une premiere vérification afin de voir si le nombre de réponses de l'utilisateur est bien le meme que le nombre de réponses attendue
          // Permet de minimiser le traitement en cas de mauvaise réponses
          if(nbReponse == this.questions[i].bonneReponse.length){
            for( var j = 0; j<this.questions[i].bonneReponse.length; j++){
              for( var h = 0; h<this.questions[i].reponse.length; h++){
                // Pour chaque réponses de l'utilisateur on vérifie que la réponse est bien présente dans les réponses attendues
                if(this.questions[i].reponse[j] == this.questions[i].bonneReponse[h]){
                  // Incrémente la variable des bonnes réponses de l'utilisateur
                  nbReponseBonne++;
                }
              }
            }
            // Vérifie que le nombre de bonne réponse de l'utilisateur est le meme que le nombre de bonne réponses attendue
            // Permet d'incrémenter le compteur de bonne réponse et de passer l'état de la question a vrai ( ce qui permet de facilement identifier les bonnes réponses)
            if(nbReponse == nbReponseBonne){
            this.questions[i].etatReponse = true
            this.nbBonneReponse++
          }
          
          }
          
          
          
        }
      // Permet l'affichage de la grille des réponses
      this.voirMesResultat = true
      // Envoie en bdd
      db.put({
            _id: this.nom+this.prenom+this.societe+(Math.random()*100000),
            societe: this.societe,
            nom: this.nom,
            prenom: this.prenom,
            reponses: this.questions
            });
    },
    // Permet d'afficher les questions 
    dsplQuestion(){
      this.voirQuestion = true
    },
  }
};
</script>
