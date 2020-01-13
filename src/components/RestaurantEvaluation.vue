<template>
  <div>
    <p>---------------------------------</p>
    <h1> Notes et commentaires </h1>
    <div v-for="evaluation in item" :key="evaluation">
      <p>---------------------------------</p>
      <p> Date : {{evaluation.date}}</p>
      <p> Grade : {{evaluation.grade}}</p>
      <p> Note : {{evaluation.score}}</p>
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
      evaluations: this.value
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
