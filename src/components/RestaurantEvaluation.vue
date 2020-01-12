<template>
  <div>
    <p>---------------------------------</p>
    <h1> Notes et commentaires </h1>
    <div v-for="evaluation in evaluations" :key="evaluation.nom">
      <p>---------------------------------</p>
      <p> Utilisateur : {{evaluation.nom}}</p>
      <p> Note : {{evaluation.grade}}/5</p>
      <p> Commentaire : {{evaluation.commentaire}}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['value'],

  data: function(){
    return {
      evaluations: this.value
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
