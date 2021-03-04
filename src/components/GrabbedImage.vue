<template>
  <img
    id="grabbed"
    height="128"
    width="128"
    :style="style"
    class="grabbed-image"
    draggable="false"
  />
</template>

<script lanb='ts'>
import Vue from 'vue';

export default Vue.extend({
  name: 'GrabbedImage',
  props: {
    imageX: {
      type: Number,
      required: true,
    },
    imageY: {
      type: Number,
      required: true,
    },
  },
  computed: {
    style() {
      return `top: ${this.imageY}px; left: ${this.imageX}px`;
    },
  },
});
</script>

<style lang=scss>
.grabbed-image {
  position: absolute;
  opacity: 0.3;
  z-index: 1;
  pointer-events: none;
}
</style>
