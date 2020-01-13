<template>
  <div>
    <p>---------------------------------</p>
    <h1> La carte </h1>
    <div v-if="item">  
      <div v-for="carte in item" :key="carte.nom">
        <p>---------------------------------</p>
        <h2> Les {{carte.nom}} </h2>
        <div v-for="plat in carte.plats" :key="plat.nom">
          <h3> {{plat.nom}} </h3>
          <p> {{plat.desription}} </p>
          <!-- <img src="#"/> -->
          <p> Prix : {{plat.prix}}€</p>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Carte indisponible pour ce restaurant</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {item: Array
  },

  data: function(){
    return {
    }
  },
  mounted(){
    
  },
  methods: {
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
