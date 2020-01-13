<template>
<div>
  <br/>
  <h1>Nombre de restaurants : {{nbRestaurants}}</h1><br/>
  <md-button class="md-raised md-primary" v-on:click="pagePrecedente()" v-bind:disabled="page==0">Précédent</md-button>
  <md-button class="md-raised md-primary" v-on:click="pageSuivante()" :disabled="page == nbPagesDeResultats">Suivant</md-button>
  <br/>
  <p>
    Nombre de restaurants par page :
    <vue-slider 
      ref="slider"
      type="range"
      min="2"
      max="100"
      value="10"
      v-on:change="getDataFromServer()"
      v-model="pagesize" 
    />
    {{pagesize}}
  </p><br/><br/>
  
  <div>
    <form v-if="isInModif" v-on:submit="modifierRestaurant" method="post">
      <h1>Modifier le restaurant {{nomModif}} :</h1>
      <div class="md-layout md-gutter">
        <div class="md-layout-item">
          <md-field>
            <label>Nom du restaurant</label>
            <md-input v-model="nom" required></md-input>
          </md-field>
        </div>
        <div class="md-layout-item">
          <md-field>
            <label>Type de cuisine</label>
            <md-input v-model="cuisine" required></md-input>
          </md-field>
        </div>
          <div class="md-layout-item">
            <md-button class="md-raised md-primary" type="submit">Modifier</md-button>
          </div>
      </div>
    </form>
    <form v-else v-on:submit="ajouterRestaurant" method="post">
      <h1>Ajouter un nouveau restaurant :</h1><br/>
      <div class="md-layout md-gutter">
        <div class="md-layout-item">
          <md-field>
            <label>Nom du restaurant</label>
            <md-input v-model="nom" required></md-input>
          </md-field>
        </div>
        <div class="md-layout-item">
          <md-field>
            <label>Type de cuisine</label>
            <md-input v-model="cuisine" required></md-input>
          </md-field>
        </div>
          <div class="md-layout-item">
            <md-button class="md-raised md-primary" type="submit">Ajouter</md-button>
          </div>
      </div>
    </form>
    
    <br/>
  </div>
        <md-table v-model="restaurants" md-sort="name" md-sort-order="asc" md-card md-fixed-header>
            <md-table-toolbar>
                <div class="md-toolbar-section-start">
                    <h1 class="md-title">Nom cherche</h1>
                </div>

                <md-field md-clearable class="md-toolbar-section-end">
                    <md-input placeholder="Rechercher par Nom" v-model="nomRecherche" @input="getDataFromServer()" />
                </md-field>
            </md-table-toolbar>

            <md-table-empty-state
        md-label="No users found"
        :md-description="`No user found for this '${nomRecherche}' query. Try a different search term or create a new user.`">
      </md-table-empty-state>

            

            <md-table-row slot="md-table-row" slot-scope="{ item }">
                <md-table-cell md-label="Name" md-sort-by="name">{{ item.name }}</md-table-cell>
                <md-table-cell md-label="Cuisine" md-sort-by="cuisine">{{ item.cuisine }}</md-table-cell>
                <md-table-cell md-label="Details"><router-link :to="'restaurant/'+item._id">Details</router-link></md-table-cell>
                <md-table-cell><button v-on:click="demandeModif(item._id, item.name, item.cuisine)">Modifier</button></md-table-cell>
                <md-table-cell><button v-on:click="supprimerRestaurant(item._id)">Supprimer</button></md-table-cell>
            </md-table-row>
        </md-table>
  </div>
</template>

<script>
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/antd.css'
export default {
  components: {
    VueSlider
  },
  name: "Restaurants",
  props: {},
  data: function() {
    return {
      restaurants: [],
      nbRestaurants: 0,
      nom: "",
      cuisine: "",
      page: 0,
      pagesize: 10,
      nomRecherche: "",
      nbPagesDeResultats: 0,
      apiURL: "http://localhost:8080/api/restaurants",
      isInModif: false,
      indexModif: 0,
      nomModif: ""
    };
  },
  mounted() {
    this.getDataFromServer();
  },
  methods: {
    async getDataFromServer() {
      // ici on fait un fetch pour récupérer des
      // restaurants sur le serveur.
      let url = this.apiURL +
                "?page=" +
                this.page +
                "&pagesize=" +
                this.pagesize +
                "&name=" +
                this.nomRecherche;
      try {
        let reponseJSON = await fetch(url);
        let reponseJS = await reponseJSON.json();
        this.restaurants = reponseJS.data;
        this.nbRestaurants = reponseJS.count;
        this.nbPagesDeResultats = Math.floor(this.nbRestaurants / this.pagesize);
      } catch(err) {
        console.log("Erreur dans les fetchs GET " + err.msg);
      }
    },
    async supprimerRestaurant(index) {
      let url = this.apiURL + "/" + index;
      try{
        let reponseJSON = await fetch(url, {
            method:"DELETE"
        });
        let reponseJS = await reponseJSON.json();
              console.log("Restaurant supprime : " + reponseJS.msg);
              this.getDataFromServer(); // on rafraichit l'affichage
      } catch(err) {
              console.log("Erreur dans le fetchs DELETE " + err.msg);
      }
  
    },
    async ajouterRestaurant(event) {
      // eviter le comportement par defaut
      event.preventDefault();
      let donneesFormulaire = new FormData();
      donneesFormulaire.append("nom", this.nom);
      donneesFormulaire.append("cuisine", this.cuisine);
      try{
        let reponseJSON = await fetch(this.apiURL, {
            method:"POST",
            body:donneesFormulaire
        });
        let reponseJS = await reponseJSON.json();
        console.log(reponseJS.msg);
        this.nom = "";
        this.cuisine = "";
        this.getDataFromServer(); // on rafraichit
      } catch(err) {
        console.log("Erreur dans le fetch POST " + err.msg);
      }
    },
    async modifierRestaurant(event) {
      event.preventDefault();
      let url = this.apiURL + "/" + this.indexModif;
      let donneesFormulaire = new FormData();
      donneesFormulaire.append("nom", this.nom);
      donneesFormulaire.append("cuisine", this.cuisine);
      try{
        let reponseJSON = await fetch(url, {
          method:"PUT",
          body:donneesFormulaire
        });
        let reponseJS = await reponseJSON.json();
        console.log(reponseJS.msg);
        this.isInModif = false;
        this.indexModif = 0;
        this.nom = "";
        this.cuisine = "";
        this.getDataFromServer();
      } catch(err) {
          console.log("Erreur dans le fetch PUT " + err.msg);
      }
    },
    demandeModif(index, name, cuisine) {
      this.isInModif  = true;
      this.indexModif = index;
      this.nom        = name;
      this.nomModif   = name;
      this.cuisine    = cuisine;
    },
    getColor(index) {
      return index % 2 ? "lightBlue" : "pink";
    },
    pageSuivante() {
      console.log("Page suivante");
      this.page++;
      this.getDataFromServer();
    },
    pagePrecedente() {
      console.log("Page precedente");
      this.page--;
      this.getDataFromServer();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
