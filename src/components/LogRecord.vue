<template>
  <div class="log__record">
    <span class="log__date">[12:34:45] - </span>
    <span class="log__object">Object 1 </span>
    <span class="log__action">added to </span>
    <span class="log__container">Container A</span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'LogRecord',
});
</script>

<style lang="scss">
  .log {
    &__record {
      font-size: .75rem;
    }
    &__date {
      color: #999;
    }
    &__object {
      color: #03AC00;
      font-weight: 700;
    }
    &__container {
      color: #0094FF;
      font-weight: 700;
    }
  }
</style>
