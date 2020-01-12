<template>
  <div>
    <p>---------------------------------</p>
    <h1> Au menu </h1>
    <div v-for="menu in menus" :key="menu.nom">
      <p>---------------------------------</p>
      <h2>{{menu.nom}}</h2>
      <p> Prix : {{menu.prix}}€</p>
      <!-- <button v-on:click="afficheMenu()">Détails du menu</button> -->
      <h3> Les entrées : </h3>
      <p v-for="entree in menu.entrees" :key="entree"> {{entree}}</p>
      <h3> Les plats : </h3>
      <p v-for="plat in menu.plats" :key="plat"> {{plat}}</p>
      <h3> Les desserts : </h3>
      <p v-for="dessert in menu.desserts" :key="dessert"> {{dessert}}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['value'],

  data: function(){
    return {
      menus: this.value,
      //detailMenu: false
    }
  }//,
  // methods: {
  //   afficheMenu() {
  //     if (this.detailMenu) {
  //       this.detailMenu = false;
  //     } else {
  //       this.detailMenu = true;
  //     }
  //   }
  // }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
