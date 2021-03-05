<template >
  <button
    id="remove-btn"
    type="button"
    :style="style"
    class="remove-btn"
    @click="$emit('remove-image')"
  >
    &#10005;
  </button>
</template>
<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'RemoveBtn',
  props: {
    offsetX: {
      type: Number,
      required: true,
    },
    offsetY: {
      type: Number,
      required: true,
    },
  },
  computed: {
    style() {
      return `top: ${this.offsetY}px; left: ${this.offsetX}px`;
    },
  },
});
</script>
<style lang="scss">
  .remove-btn {
    position: absolute;
    z-index: 1;
    height: 1rem;
    width: 1rem;
    border: none;
    border-radius: 50%;
    text-align: center;
    padding: 0;
    line-height: 1rem;
    font-size: .8rem;
    color: #fff;
    background-color: #f00;
    opacity: 0.5;
    cursor: pointer;
    &:hover {
      outline: none;
      opacity: 1;
    }
    &:focus {
      outline: none;
    }
  }
</style>
