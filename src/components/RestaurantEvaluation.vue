<template>
  <div>
    <p>---------------------------------</p>
    <h1> Notes et commentaires </h1>
    <div v-if="hasEval">
      <div v-for="evaluation in item" :key="evaluation.date"> 
        <p>---------------------------------</p>
        <p> Date : {{evaluation.date}}</p>
        <p> Grade : {{evaluation.grade}}</p>
        <p> Note : {{evaluation.score}}</p>
      </div>
    </div>
    <div v-else>
      <p>Aucune note disponible</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    item: Array
  },

  data: function(){
    return {
      evaluations: this.value,
      hasEval: true
    }
  },
  updated(){
    this.hasEval = this.verifNote();
  },
  methods: {
    verifNote(){
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
