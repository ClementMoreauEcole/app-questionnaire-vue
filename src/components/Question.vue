<template>
  <div class="Question">
    <h1>{{question.id}} - {{question.intitule}} </h1>
   <div v-for=" unchoix in question.choix">
    <input type="checkbox">
    <label for="scales">{{unchoix.libelle}}</label>
  </div>
    {{question}}
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
</style>
