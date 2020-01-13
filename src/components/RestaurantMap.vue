<template>
  <div>
    <p>---------------------------------</p>
    <p> Latitude : {{item[1]}} Longitude : {{item[0]}} </p>
    <GmapMap
  :center="{lat:item[1], lng:item[0]}"
  :zoom="7"
  map-type-id="terrain"
  style="width: 500px; height: 300px"
>
</GmapMap>
  </div>
</template>

<script>
export default {
  props: {item: Array
  },

  data: function(){
    return {
    }
  },
  updated(){
    
  },
  methods: {
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
