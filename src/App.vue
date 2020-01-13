<template>
  <div id="app">
    <md-tabs md-sync-route>
    <md-tab id="tab-home" md-label="Home" to="/" exact></md-tab>
      <md-tab id="tab-restaurants" md-label="Restaurants" to="/restaurants"></md-tab>
    </md-tabs>
     <router-view></router-view>
  
  </div>
  
</template>

<script>

export default {
  name: "app",
  components: {
  },
  data: function() {
    return {
      nom: "buffa"
    };
  },
  mounted()  {
    console.log("avant affichage");
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
