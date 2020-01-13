<template>
  <div>
    <p>---------------------------------</p>
    <h1> Au menu </h1>
    <div v-if="hasMenu">
      <div v-for="menu in item" :key="menu.nom">
        <p>---------------------------------</p>
        <h2>{{menu.nom}}</h2>
        <p> Prix : {{menu.prix}}€</p>
        <!-- <button v-on:click="afficheMenu()">Détails du menu</button> -->
        <h3> Les entrées : </h3>
        <p v-for="entree in menu.entrees" :key="entree"> {{entree}}</p>
        <h3> Les plats : </h3>
        <p v-for="plat in menu.plat" :key="plat"> {{plat}}</p>
        <h3> Les desserts : </h3>
        <p v-for="dessert in menu.dessert" :key="dessert"> {{dessert}}</p>
      </div>
    </div>
    <div v-else>
      <p>Menu indisponible pour ce restaurant</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {item: Array},

  data: function(){
    return {
      //detailMenu: false
      hasMenu: true
    }
  },
  updated(){
    this.hasMenu = this.verifMenu()
  },
  methods: {
    verifMenu(){
      if(this.item.length > 0){
        return true;
      } else {
        return false;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
