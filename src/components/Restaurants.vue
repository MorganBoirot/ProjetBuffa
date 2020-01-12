<template>
<div>
  <p>
    Rechercher par nom:
    <input type="text" v-model="nomRecherche" v-on:input="getDataFromServer()" />
  </p>
  <p>
    Nombre de restaurants par page :
    <input
      type="range"
      min="2"
      max="100"
      value="10"
      v-on:input="getDataFromServer()"
      v-model="pagesize"
    />
    {{pagesize}}
  </p>
  <h1>Nombre de restaurants : {{nbRestaurants}}</h1>
  <button v-on:click="pagePrecedente()" v-bind:disabled="page==0">Précédent</button>
  <button v-on:click="pageSuivante()" :disabled="page == nbPagesDeResultats">Suivant</button>
 
  <H1>TABLE VUE-MATERIAL</H1>
  <div>
    <form v-if="isInModif" v-on:submit="modifierRestaurant" method="post">
      <h1>Modifier le restaurant {{nomModif}} :</h1>
      Nom du restaurant:<input type="text" v-model="nom" required>
      Type de cuisine:<input type="text" v-model="cuisine" required>
      <button type="submit">Modifier</button>
    </form>
    <form v-else v-on:submit="ajouterRestaurant" method="post">
      <h1>Ajouter un nouveau restaurant :</h1>
      Nom du restaurant:<input type="text" v-model="nom" required>
      Type de cuisine:<input type="text" v-model="cuisine" required>
      <button type="submit">Ajouter</button>
    </form>
  </div>
        <md-table v-model="restaurants" md-sort="name" md-sort-order="asc" md-card md-fixed-header>
            <md-table-toolbar>
                <div class="md-toolbar-section-start">
                    <h1 class="md-title">Nom cherche</h1>
                </div>

                <md-field md-clearable class="md-toolbar-section-end">
                    <md-input placeholder="Search by name..." v-model="nomRecherche" @input="getDataFromServer()" />
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
export default {
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
    console.log("AVANT AFFICHAGE !");
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
