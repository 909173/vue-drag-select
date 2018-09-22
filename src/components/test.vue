<template>
  <draggable-area selector-class="item">
    <template slot-scope="{ selectedItems }">
      <draggable-item
        v-for="item in testJson" :key="item.id"
        :text="[item.id, item.name]"
        :keyString="item.id.toString()"
        :class="getClasses(item, selectedItems)"
      >        
      </draggable-item>
    </template>
  </draggable-area>
</template>


<script lang="ts">
import Vue from "vue";
import draggableAreaVue from "@/draggable-area/draggableArea.vue";
import draggableItemVue from "@/draggable-area/draggableItem.vue";
export default Vue.extend({
  name: "test",
  components: {
    "draggable-area": draggableAreaVue,
    "draggable-item": draggableItemVue
  },
  computed: {
    getClasses() {
      return (item: { id: number; name: string }, selectedItems: any[]) => {
        console.log(item, selectedItems);
        const isActive = !!selectedItems.find(
          selectedItem => selectedItem.keyString === item.id.toString()
        );
        return {
          item: true,
          active: isActive
        };
      };
    }
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
    };
  }
});
</script>


<style scoped>
/* Basic reset */
*,
*:before,
*:after {
  box-sizing: inherit;
}
html {
  box-sizing: border-box;
  user-select: none;
}
html,
body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
}
body {
  font: 16px / 1.5 "Helvetica Neue", sans-serif;
  padding: 5%;
}
/* Custom styling */
.item {
  display: inline-flex;
  min-width: 80px;
  height: 100px;
  margin-right: 10px;
  margin-bottom: 10px;
  background-color: #ddd;
  justify-content: center;
  align-items: center;
  text-transform: uppercase;
  letter-spacing: 3px;
  font-size: 10px;
}
.item.active {
  background-color: rgb(0, 162, 255);
  color: #fff;
}
</style>
