<template>
  <div class="sidebar">
    <h2>Assets</h2>
    <button @click="addText">+ Add Text</button>
    <input type="file" @change="uploadImage" />
    <p v-if="error" class="error-message">{{ error }}</p>
    <div class="image-list">
      <img
        v-for="(img, i) in images"
        :key="i"
        :src="img"
        @click="addImage(img)"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["images", "fetchImages", "saveItems"],
  emits: ["refresh-images", "add-image", "add-text"],
  data() {
    return {
      file: null,
      error: null,
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
    uploadImage(e) {
      const file = e.target.files[0];
      if (!file) return;

      // Check file type
      if (!file.type.startsWith("image/")) {
        this.error = "Only image files are allowed (jpg, png)";
        return;
      }

      this.error = null;
      const formData = new FormData();
      formData.append("upload", file);
      fetch("http://localhost:8000/uploads", {
        method: "POST",
        body: formData,
      })
        .then(() => {
          this.fetchImages();
        })
        .catch(() => {
          this.error = "Failed to upload image. Try again.";
        });
    },
    addImage(src) {
      this.$emit("add-image", src);
    },
    addText() {
      this.$emit("add-text");
    },
  },
};
</script>

<style scoped>
.sidebar {
  width: 250px;
  background: #f5f5f5;
  padding: 1rem;
  border-right: 1px solid #ddd;
  overflow: hidden scroll;
}

.sidebar h2 {
  margin-top: 0;
}

.sidebar button {
  background-color: #3b82f6;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  margin-bottom: 1rem;
}

.sidebar input {
  margin-bottom: 1rem;
}

.sidebar .error-message {
  color: red;
  font-size: 12px;
  margin-bottom: 0.5rem;
}

.image-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.image-list img {
  max-width: 100%;
  cursor: pointer;
  border: 1px solid #ccc;
  padding: 4px;
  border-radius: 4px;
  transition: 0.2s;
}

.image-list img:hover {
  border-color: #3b82f6;
  transform: scale(1.03);
}
</style>
