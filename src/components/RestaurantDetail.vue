<template>
  <div>
    <br/><br/>
    <h1>Details du restaurant {{nom}}</h1>
    <br/>
    <p> Type : {{cuisine}} </p>
    <p> Adresse : {{addr}} </p>
    <br/>
    <img id="img1" :src="imgUrl"/>
    <restaurant-carte :item="carte"></restaurant-carte>
    <restaurant-menu :item="menus"></restaurant-menu>
    <restaurant-evaluation :item="evaluations"></restaurant-evaluation>
    <restaurant-map :item="coord"></restaurant-map>
    <div class="restaurant-map"></div>
  </div>
</template>

<script>
import RestaurantCarte from './RestaurantCarte';
import RestaurantMenu from './RestaurantMenu';
import RestaurantEvaluation from './RestaurantEvaluation';
import RestaurantMap from './RestaurantMap';
export default {
  name: "RestaurantDetail",
  props: {},
  components: {
    RestaurantCarte,
    RestaurantMenu,
    RestaurantEvaluation,
    RestaurantMap
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
      coord: [],
      imgUrl: "",
      lon: "",
      lat: "",
      quartier: "",
      apiURL: "http://localhost:8080/api/restaurants",
      carte: [],
      menus: [],
      Img:["/img/restaurant1.5ba12bcf.jpg",
      "/img/restaurant0.3d4e99aa.jpg",
      "/img/restaurant2.56064ddf.jpg",
      "/img/restaurant3.9982a728.jpg"],
      evaluations: []
    };
  },
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
          this.coord = reponseJS.restaurant.address.coord;
          
        } catch (err) {
          console.log("Erreur dans les fetchs GET " + err.msg);
        }
          let i = this.getRandomInt();
          this.imgUrl=this.Img[i];
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
