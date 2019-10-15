<template>
  <div class="home">
    <input v-model="nom" placeholder="Entrez votre nom" >
    <input v-model="prenom" placeholder="Entrez votre prénom" >
    <input  v-model="societe" placeholder="Entrez votre société">
    <Utilisateur :nom="nom" :prenom="prenom"  :societe="societe"/>

  </div>
</template>

<script>
// @ is an alias to /src
import Utilisateur from '@/components/Utilisateur.vue'

export default {
  name: 'home',
  components: {
    Utilisateur
  },
  data() {
    return{
      nom: null,
      prenom: null,
      societe: null
    }
    
  }
};

</script>
