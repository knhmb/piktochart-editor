<template>
  <div
    class="draggable"
    :style="{ top: item.y + 'px', left: item.x + 'px' }"
    @mousedown="startDrag"
  >
    <img v-if="item.type === 'image'" :src="item.src" />
    <div v-else class="text">{{ item.text }}</div>
    <button class="delete-btn" @click="$emit('delete', item.id)">×</button>
  </div>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      dragging: false,
      offsetX: 0,
      offsetY: 0,
    };
  },
  methods: {
    startDrag(e) {
      this.dragging = true;
      this.offsetX = e.offsetX;
      this.offsetY = e.offsetY;
      window.addEventListener("mousemove", this.onDrag);
      window.addEventListener("mouseup", this.stopDrag);
    },
    onDrag(e) {
      if (!this.dragging) return;
      this.$emit("update", {
        ...this.item,
        x: e.pageX - this.offsetX,
        y: e.pageY - this.offsetY,
      });
    },
    stopDrag() {
      this.dragging = false;
      window.removeEventListener("mousemove", this.onDrag);
      window.removeEventListener("mouseup", this.stopDrag);
    },
  },
};
</script>

<style scoped>
.draggable {
  position: absolute;
  cursor: move;
  border: 1px dashed #aaa;
  padding: 4px;
}
.text {
  font-size: 16px;
  font-weight: bold;
}
.delete-btn {
  position: absolute;
  top: -10px;
  right: -10px;
  background: red;
  color: white;
  border: none;
  border-radius: 50%;
}
img {
  max-width: 100px;
}
</style>
