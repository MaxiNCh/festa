<template>
  <div class="log">
    <LogRecord></LogRecord>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import LogRecord from './LogRecord.vue';

export default Vue.extend({
  name: 'LogField',
  components: {
    LogRecord,
  },
});
</script>

<style lang="scss">
  .log {
    border: 3px solid #888;
    width: 100%;
    height: 100%;
    padding: 0.625rem;
    background: #fff;
  }
</style>
