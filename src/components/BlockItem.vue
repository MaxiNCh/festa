<template>
  <div class="block">
    <h1 class="block__h1">{{title}}</h1>
    <slot></slot>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'BlockItem',
  props: {
    title: {
      type: String,
      required: true,
    },
  },
});

</script>

<style lang="scss" scoped>
.block {
  margin-bottom: 2.3rem;
  &__h1 {
    text-align: left;
    font-size: 1.5rem;
    color: #888;
    font-weight: 400;
  }
}

</style>
