<template>
  <div class="Questionnaire">
    <h1>Question {{questionnaire.questions[questionnaire.currentQuestion].id}}</h1>
    <h2>{{questionnaire.questions[questionnaire.currentQuestion].intitule}} </h2>
      <div>
        <div class="leChoix" v-for="unchoix in questionnaire.questions[questionnaire.currentQuestion].choix">
            <input v-model="questionnaire.questions[questionnaire.currentQuestion].reponse" type="checkbox" v-bind:value="unchoix.libelle">
            <label  for="scales">{{unchoix.libelle}}</label>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Questionnairea',
  props:["questionnaireb"],
  data () {
    return {
      questionnaire: this.questionnaireb
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
