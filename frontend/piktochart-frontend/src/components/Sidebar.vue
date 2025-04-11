<template>
  <div class="sidebar">
    <h3>Images</h3>
    <form @submit.prevent="upload">
      <input type="file" @change="onFileChange" />
      <button type="submit">Upload</button>
    </form>
    <div class="images">
      <img
        v-for="img in images"
        :key="img"
        :src="img"
        @click="$emit('add-image', img)"
        class="thumbnail"
      />
    </div>
    <button @click="$emit('add-text')">Add Text</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["images"],
  data() {
    return {
      file: null,
    };
  },
  methods: {
    onFileChange(e) {
      this.file = e.target.files[0];
    },
    async upload() {
      const formData = new FormData();
      formData.append("upload", this.file);
      await axios.post("http://localhost:8000/uploads", formData);
      this.$emit("refresh-images");
    },
  },
};
</script>

<style scoped>
.sidebar {
  width: 200px;
  padding: 10px;
}
.thumbnail {
  width: 100%;
  cursor: pointer;
  margin-bottom: 10px;
}
</style>
