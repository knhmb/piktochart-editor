<template>
  <div class="app">
    <Sidebar
      :images="imageList"
      @add-image="addImage"
      @add-text="addText"
      @refresh-images="fetchImages"
    />
    <Canvas
      :items="canvasItems"
      @update-item="updateItem"
      @delete-item="deleteItem"
    />
  </div>
</template>

<script>
import Sidebar from "./components/Sidebar.vue";
import Canvas from "./components/Canvas.vue";

export default {
  components: { Sidebar, Canvas },
  data() {
    return {
      imageList: [],
      canvasItems: [],
    };
  },
  methods: {
    async fetchImages() {
      const res = await fetch("http://localhost:8000/images");
      this.imageList = await res.json();
    },
    addImage(src) {
      this.canvasItems.push({
        id: crypto.randomUUID(),
        type: "image",
        src,
        x: 50,
        y: 50,
      });
    },
    addText() {
      this.canvasItems.push({
        id: crypto.randomUUID(),
        type: "text",
        text: "New Text",
        x: 50,
        y: 50,
      });
    },
    updateItem(updatedItem) {
      const idx = this.canvasItems.findIndex((i) => i.id === updatedItem.id);
      if (idx !== -1) this.canvasItems.splice(idx, 1, updatedItem);
    },
    deleteItem(id) {
      this.canvasItems = this.canvasItems.filter((i) => i.id !== id);
    },
    saveCanvas() {
      localStorage.setItem("canvas", JSON.stringify(this.canvasItems));
    },
    loadCanvas() {
      const saved = localStorage.getItem("canvas");
      if (saved) this.canvasItems = JSON.parse(saved);
    },
  },
  watch: {
    canvasItems: {
      handler: "saveCanvas",
      deep: true,
    },
  },
  mounted() {
    this.loadCanvas();
    this.fetchImages();
  },
};
</script>

<style scoped>
.app {
  display: flex;
}
</style>
