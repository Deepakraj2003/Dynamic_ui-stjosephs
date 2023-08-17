<template>
  <div class="page">
    <div class="page-content" :style="{ width: contentWidth + 'px' }">
  <fo></fo>
    </div>
    <div class="resizer" @mousedown="startResize"></div>
  </div>
</template>

<script>
import fo from "./form.vue"
export default {

  data() {
    return {
      contentWidth: 600,
      resizing: false,
      startX: 0,
      minWidth: 300, // Minimum width for the content area
    };
  },
  components:{
    fo
  },
  methods: {
    startResize(event) {
      this.resizing = true;
      this.startX = event.clientX;
      document.addEventListener('mousemove', this.handleResize);
      document.addEventListener('mouseup', this.endResize);
    },
    handleResize(event) {
      if (!this.resizing) return;
      const deltaX = event.clientX - this.startX;
      const newWidth = this.contentWidth + deltaX;

      // Adjust width based on minimum width
      if (newWidth >= this.minWidth) {
        this.contentWidth = newWidth;
      }

      this.startX = event.clientX;
    },
    endResize() {
      this.resizing = false;
      document.removeEventListener('mousemove', this.handleResize);
      document.removeEventListener('mouseup', this.endResize);
    },
  },
  beforeUnmount() {
    document.removeEventListener('mousemove', this.handleResize);
    document.removeEventListener('mouseup', this.endResize);
  },
};
</script>

<style scoped>
.page {
  display: flex;
  align-items: stretch;
  height: 400px;
  border: 1px solid #ccc;
}

.page-content {
  flex: 1;
  overflow: auto;
}

.resizer {
  width: 5px;
  background-color: #333;
  cursor: ew-resize;
}
</style>