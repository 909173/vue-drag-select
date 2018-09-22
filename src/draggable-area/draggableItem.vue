<template>
  <div>
    <div v-for="(item, index) in text" :key="index">
      {{item}}
    </div>
  </div>  
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "draggableItem",
  props: {
    text: Array as () => string[],
    keyString: String
  }
});
</script>
