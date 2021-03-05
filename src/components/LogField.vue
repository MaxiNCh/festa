<template>
  <div class="log">
    <LogRecord
      v-for="action of this.reverseActions"
      :action="action"
      :key="action.id"
    />
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
  props: {
    actions: {
      type: Array,
      required: true,
    },
  },
  computed: {
    reverseActions() {
      return this.actions.slice().reverse();
    },
  },
});
</script>

<style lang="scss">
  .log {
    border: 3px solid #888;
    width: 100%;
    height: 15rem;
    padding: 0.625rem;
    background: #fff;
    overflow: scroll;
  }
</style>
