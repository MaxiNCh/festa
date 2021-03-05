<template>
  <div v-if="isAddAction" class="log__record">
    <span class="log__date">[{{time}}] - </span>
    <span class="log__object">{{this.action.objectName}} </span>
    <span class="log__action">added to </span>
    <span class="log__container">{{this.action.container1}}</span>
  </div>
  <div v-else-if="isMoveAction" class="log__record">
    <span class="log__date">[{{time}}] - </span>
    <span class="log__object">{{this.action.objectName}} </span>
    <span class="log__action">moved from </span>
    <span class="log__container">{{this.action.container1}} </span>
    <span class="log__action">to </span>
    <span class="log__container">{{this.action.container2}}.</span>
  </div>
  <div v-else-if="isDeleteAction" class="log__record">
    <span class="log__date">[{{time}}] - </span>
    <span class="log__object">{{this.action.objectName}} </span>
    <span class="log__action">added to </span>
    <span class="log__container">Container A</span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'LogRecord',
  props: {
    action: {
      type: Object,
      required: true,
    },
  },
  computed: {
    isAddAction(): boolean {
      return this.action.type === 'add';
    },
    isMoveAction(): boolean {
      return this.action.type === 'move';
    },
    isDeleteAction(): boolean {
      return this.action.type === 'delete';
    },
    time(): string {
      return this.action.date.toLocaleTimeString();
    },
  },
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
