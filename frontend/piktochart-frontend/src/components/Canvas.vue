<template>
  <div class="canvas">
    <DraggableItem
      v-for="item in items"
      :key="item.id"
      :item="item"
      @update="update"
      @delete="deleteItem"
    />
  </div>
</template>

<script>
import DraggableItem from "./DraggableItem.vue";

export default {
  props: ["items"],
  components: { DraggableItem },
  methods: {
    update(item) {
      this.$emit("update-item", item);
    },
    deleteItem(id) {
      this.$emit("delete-item", id);
    },
  },
};
</script>

<style scoped>
.canvas {
  flex: 1;
  position: relative;
  height: 100vh;
  background: #f5f5f5;
}
</style>
