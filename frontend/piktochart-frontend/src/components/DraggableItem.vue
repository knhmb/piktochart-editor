<template>
  <div
    class="draggable"
    ref="element"
    :style="{ top: item.y + 'px', left: item.x + 'px' }"
    @mousedown="startDrag"
  >
    <img v-if="item.type === 'image'" :src="item.src" />

    <!-- Text block -->
    <div v-else class="text" @dblclick="edit = true">
      <input
        v-if="edit"
        type="text"
        v-model="editableText"
        @blur="commitEdit"
        @keydown.enter="commitEdit"
        class="text-input"
        autofocus
      />
      <span v-else>{{ item.text }}</span>
    </div>

    <button class="delete-btn" @click.stop="$emit('delete', item.id)">×</button>
  </div>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      offsetX: 0,
      offsetY: 0,
      dragging: false,
      edit: false,
      editableText: "",
    };
  },
  methods: {
    startDrag(e) {
      if (this.edit) return; // disable drag while editing
      const rect = this.$refs.element.getBoundingClientRect();
      this.offsetX = e.clientX - rect.left;
      this.offsetY = e.clientY - rect.top;

      this.dragging = true;
      window.addEventListener("mousemove", this.onDrag);
      window.addEventListener("mouseup", this.stopDrag);
    },
    onDrag(e) {
      if (!this.dragging) return;
      const canvasRect =
        this.$refs.element.parentElement.getBoundingClientRect();
      const x = e.clientX - canvasRect.left - this.offsetX;
      const y = e.clientY - canvasRect.top - this.offsetY;
      this.$emit("update", { ...this.item, x, y });
    },
    stopDrag() {
      this.dragging = false;
      window.removeEventListener("mousemove", this.onDrag);
      window.removeEventListener("mouseup", this.stopDrag);
    },
    commitEdit() {
      this.edit = false;
      if (this.editableText !== this.item.text) {
        this.$emit("update", { ...this.item, text: this.editableText });
      }
    },
  },
  watch: {
    edit(val) {
      if (val) this.editableText = this.item.text;
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
  background: white;
}
.text {
  font-size: 16px;
  font-weight: bold;
}
.text-input {
  font-size: 16px;
  font-weight: bold;
  border: none;
  outline: none;
  background: transparent;
}
.delete-btn {
  position: absolute;
  top: -10px;
  right: -10px;
  background: red;
  color: white;
  border: none;
  border-radius: 50%;
  cursor: pointer;
}
img {
  max-width: 100px;
}
</style>
