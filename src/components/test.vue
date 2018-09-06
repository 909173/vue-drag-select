<template>
  <draggable-area
  >
    <template>
      
    </template>
  </draggable-area>
</template>


<script lang="ts">
import Vue from 'vue'
import draggableAreaVue from '@/draggable-area/draggableArea.vue';
export default Vue.extend({
  name: "test",
  components:{
    "daggable-area": draggableAreaVue
  },
  data() {
    return {
      testJson: [
        {
          id: 0,
          name: "jhon"
        },
        {
          id: 1,
          name: "json"
        },
        {
          id: 2,
          name: "Kres"
        }
      ]
    }
  }  
})
</script>


<style scoped>

</style>
