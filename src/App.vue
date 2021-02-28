<template>
  <div id="app">
    <BlockItem :title="'Container A'">
      <CanvasItem
        :id="'canvasA'"
        :width="500"
        :height="500"
      />
    </BlockItem>
    <BlockItem :title="'Container B'">
      <CanvasItem
        :id="'canvasB'"
        :width="500"
        :height="500"
      />
    </BlockItem>
    <BlockItem :title="'Add new object'">
      <FormComponent></FormComponent>
    </BlockItem>
    <BlockItem :title="'Log'">
      <LogField></LogField>
    </BlockItem>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import BlockItem from './components/BlockItem.vue';
import CanvasItem from './components/CanvasItem.vue';
import FormComponent from './components/FormComponent.vue';
import LogField from './components/LogField.vue';

export default Vue.extend({
  name: 'App',
  components: {
    CanvasItem,
    BlockItem,
    FormComponent,
    LogField,
  },
  data() {
    return {
      images: [],
    };
  },
});

</script>

<style lang="scss">
#app {
  display: grid;
  grid-template-columns: 31.25em 31.25em;
  justify-content: space-between;
  box-sizing: border-box;
  padding: 7em calc(50% - 600px);
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
* {
  box-sizing: border-box;
}
</style>
