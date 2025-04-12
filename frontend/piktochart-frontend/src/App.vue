<template>
  <div>
    <loading-spinner v-if="loading"></loading-spinner>
    <div class="app">
      <sidebar
        :images="images"
        :fetch-images="fetchImages"
        :save-items="saveItems"
        @add-image="addImage"
        @add-text="addText"
      ></sidebar>
      <div class="canvas" ref="canvas">
        <DraggableItem
          v-for="item in items"
          :key="item.id"
          :item="item"
          @update="updateItem"
          @delete="deleteItem"
        />
      </div>
    </div>
  </div>
</template>

<script>
import DraggableItem from "./components/DraggableItem.vue";
import Sidebar from "./components/Sidebar.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

export default {
  components: { DraggableItem, Sidebar, LoadingSpinner },
  data() {
    return {
      images: [],
      items: [],
      loading: false,
    };
  },
  methods: {
    fetchImages() {
      this.loading = true;
      fetch("http://localhost:8000/images")
        .then((res) => res.json())
        .then((data) => {
          this.loading = false;
          this.images = data.map((name) => name);
        });
    },
    updateItem(updated) {
      const idx = this.items.findIndex((i) => i.id === updated.id);
      if (idx !== -1) this.items.splice(idx, 1, updated);
      this.saveItems();
    },
    addImage(src) {
      this.items.push({
        id: Date.now(),
        type: "image",
        src,
        x: 100,
        y: 100,
      });
      this.saveItems();
    },
    addText() {
      this.items.push({
        id: Date.now(),
        type: "text",
        text: "Edit me",
        x: 100,
        y: 100,
      });
      this.saveItems();
    },
    deleteItem(id) {
      this.items = this.items.filter((i) => i.id !== id);
      this.saveItems();
    },
    saveItems() {
      console.log("rached");

      localStorage.setItem("canvas-items", JSON.stringify(this.items));
    },
    loadItems() {
      const saved = localStorage.getItem("canvas-items");
      if (saved) this.items = JSON.parse(saved);
    },
  },
  created() {
    this.fetchImages();
    this.loadItems();
  },
};
</script>

<style scoped>
.app {
  display: flex;
  font-family: "Segoe UI", sans-serif;
  height: 100vh;
}

.canvas {
  flex: 1;
  position: relative;
  background: #fff;
  border: 4px dashed #ddd;
  margin: 1rem;
  overflow: auto;
}
</style>
