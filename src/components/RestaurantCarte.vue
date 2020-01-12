<template>
  <div>
    <p>---------------------------------</p>
    <h1> La carte </h1>
    <div v-for="carte in cartes" :key="carte.nom">
      <p>---------------------------------</p>
      <h2> Les {{carte.nom}} </h2>
      <div v-for="plat in carte.plats" :key="plat.nom">
        <h3> {{plat.nom}} </h3>
        <p> {{plat.description}} </p>
        <img src="#"/>
        <p> Prix : {{plat.prix}}€</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['value'],

  data: function(){
    return {
      cartes: this.value
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
