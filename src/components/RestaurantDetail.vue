<template>
  <div>
    <h1>Details du restaurant {{nom}}</h1>
    <p> Type : {{cuisine}} </p>
    <p> Adresse : {{addr}} </p>
    <img src="../assets/restaurant2.jpg"/>
    <restaurant-carte :item="carte"></restaurant-carte>
    <restaurant-menu :item="menus"></restaurant-menu>
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
      addr: "",
      imgUrl: "",
      lon: "",
      lat: "",
      quartier: "",
      apiURL: "http://localhost:8080/api/restaurants",
      carte: [],
      menus: [],
      evaluations: []
    };
  },
  /*mounted() {
    console.log("AVANT AFFICHAGE !");
    console.log("On va chercher les détails du restaurant id = " + this.$route.params.id)
    console.log("ID = " + this.id);
  },*/
  mounted() {
    this.getDataFromServer();
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
          this.cuisine = reponseJS.restaurant.cuisine;
          this.addr = reponseJS.restaurant.address.building + " " + reponseJS.restaurant.address.street + ", " + reponseJS.restaurant.address.zipcode + " " + reponseJS.restaurant.borough; 
          this.carte = reponseJS.restaurant.carte;
          this.menus = reponseJS.restaurant.menu;
          this.evaluations = reponseJS.restaurant.grades;
        } catch (err) {
          console.log("Erreur dans les fetchs GET " + err.msg);
        }
         let i = this.getRandomInt();
        this.imgUrl = "../assets/restaurant" + i + ".jpg";
    },
    getRandomInt() {
           return Math.floor(Math.random() * Math.floor(3));
      },    
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
