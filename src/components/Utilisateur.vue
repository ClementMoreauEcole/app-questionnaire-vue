<template>
  <div class="Utilisateur">
    <h1>{{ nom }}</h1>
    <h1>{{ prenom }}</h1>
    <h1>{{ societe }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Utilisateur',
  props: {
    nom: String,
    prenom: String,
    societe: String
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
