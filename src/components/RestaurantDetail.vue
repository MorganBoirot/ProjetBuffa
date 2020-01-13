<template>
  <div>
    <h1>Details du restaurant {{nom}}</h1>
    <p> Type : {{restaurant.cuisine}} </p>
    <p> Adresse : {{restaurant}} </p>
    <p> Longitude : {{lon}} - Latitute : {{lat}} </p>
    <p> Description du quartier : {{quartier}} </p>
    <img src="../assets/restaurant.jpg">
    <restaurant-carte v-model="carte"></restaurant-carte>
    <restaurant-menu v-model="menus"></restaurant-menu>
    <restaurant-evaluation v-model="evaluations"></restaurant-evaluation>
    <div class="restaurant-map">esfsef</div>
  </div>
</template>

<script>
import RestaurantCarte from './RestaurantCarte';
import RestaurantMenu from './RestaurantMenu';
import RestaurantEvaluation from './RestaurantEvaluation';
export default {
  name: "RestaurantDetail",
  props: {},
  components: {
    RestaurantCarte,
    RestaurantMenu,
    RestaurantEvaluation
  },
  computed: { // computed data, permet de définir des data "calculées"
    id() {
      // on y accèdera par {{id}} dans le template, et par this.id
      // dans le code
      return this.$route.params.id
    }
  },
  data:() => {
    return {
      restaurant: {},
      nom: "",
      cuisine: "",
      adr: "",
      lon: "",
      lat: "",
      quartier: "",
      apiURL: "http://localhost:8080/api/restaurants",
      carte: [{
        nom: "Entrées",
        plats: [{
          nom: "Salade de chèvres chauds",
          description: "Salade composée de salade et de fromage de chèvre chaud",
          img: "",
          prix: "12"},
          {
          nom: "Plateau de foie gras",
          description: "Plateau avec du foie gras lol",
          img: "",
          prix: "14"},
          {
          nom: "Salade de poulpe",
          description: "Salade avec du poulpe dedans",
          img:"",
          prix: "15"}]},
        {
        nom: "Plats",
        plats: [{
          nom: "Salade de chèvres chauds",
          description: "Salade composée de salade et de fromage de chèvre chaud",
          img: "",
          prix: "12"},
          {
          nom: "Plateau de foie gras",
          description: "Plateau avec du foie gras lol",
          img: "",
          prix: "14"},
          {
          nom: "Salade de poulpe",
          description: "Salade avec du poulpe dedans",
          img:"",
          prix: "15"}]},
        {
        nom: "Desserts",
        plats: [{
          nom: "Salade de chèvres chauds",
          description: "Salade composée de salade et de fromage de chèvre chaud",
          img: "",
          prix: "12"},
          {
          nom: "Plateau de foie gras",
          description: "Plateau avec du foie gras lol",
          img: "",
          prix: "14"},
          {
          nom: "Salade de poulpe",
          description: "Salade avec du poulpe dedans",
          img:"",
          prix: "15"}]
      }],
      menus: [{
        nom: "Menu Simple",
        prix: 20,
        entrees: ["Salade de chèvres chauds", "Plateau de foie gras", "Tapas"],
        plats: ["Filet mignon en croûte", "Dobe de sanglier", "Cuisse de canard"],
        desserts: ["Mousse au chocolat", "Glace", "Banane flambées"]
      },
      {
        nom: "Menu gastronomique",
        prix: 32,
        entrees: ["Salade de poulpe", "Tartare de saumon", "Caviar"],
        plats: ["Souris d'agneau", "Tartare de boeuf", "Gambas sautées"],
        desserts: ["Tiramisu", "Crème brulée", "Profiterols"]
      }],
      evaluations: [{
        nom: "Louis",
        grade: "5",
        commentaire:"OK"
      },
      {
        nom: "Max",
        grade: "3",
        commentaire:"BOF"
      }]
    };
  },
  /*mounted() {
    console.log("AVANT AFFICHAGE !");
    console.log("On va chercher les détails du restaurant id = " + this.$route.params.id)
    console.log("ID = " + this.id);
  },*/
  mounted() {
    this.getDataFromServer();
    console.log("ID = " + this.id);
  },
  methods: {
    async getDataFromServer() {
      
        let url = this.apiURL + "/" + this.id;

        try {
          let reponseJSON = await fetch(url, {
            method: "GET"
          });
          let reponseJS = await reponseJSON.json();
          this.restaurant = reponseJS.restaurant;
          this.nom = reponseJS.restaurant.name;
          console.log(reponseJS);
        } catch (err) {
          console.log("Erreur dans les fetchs GET " + err.msg);
        }
    },
    
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
