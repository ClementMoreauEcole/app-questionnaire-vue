<template>
  <!-- Le corps du composant qui sera affiché a la place des balises du meme nom -->
  <div class="Question">
    <h1>Question {{question.id}}</h1>
    <h2>{{question.intitule}} </h2>
      <div>
        <div class="leChoix" v-for="unchoix in question.choix">
            <input v-model="question.reponse" type="checkbox" v-bind:value="unchoix.libelle">
            <label  for="scales">{{unchoix.libelle}}</label>

           
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Question',
  props:["question"],
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
.listeChoix{
  display: flex;
  flex-direction: row;

}
.leChoix{
  align-items: center;
}
</style>
