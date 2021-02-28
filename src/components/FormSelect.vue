<template>
  <div class="select-wrapper">
    <label
      for="container-select"
      class="form__label"
    >
      Object container
    </label >
    <div class="arrow__wrapper">
      <select
        id="container-select"
        name="container-select"
        class="form__select"
        type="text"
        required
      >
        <option value="containerA">Container A</option>
        <option value="containerB">Container B</option>
      </select>
      <div class="arrow__block">
        <p class="arrow__down"></p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'FormSelect',
});
</script>

<style lang="scss">
  .select-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .form__select {
    appearance: none;
    border: 1px solid #888;
    border-radius: 4px;
    height: 2rem;
    width: 12.5rem;
    padding: 0 .5rem;
    background: unset;
    font-size: 0.875rem;
    &:hover {
      cursor: pointer;
      border-color: #000;
    }
    &:focus {
      outline: none;
    }
    option {
      line-height: 2rem;
    }
  }
  .arrow {
    &__wrapper {
      position: relative;
      display: inline-block;
    }
    &__block {
    position: absolute;
    z-index: -1;
    display: inline-block;
    right: 0;
    box-sizing: border-box;
    border-left: 1px solid #888;
    height: 2em;
    width: 2em;
    }
    &__down {
      position: absolute;
      bottom: -3px;
      right: 12px;
      border: solid #888;
      border-width: 0 2px 2px 0;
      display: inline-block;
      padding: 3px;
      transform: rotate(45deg);
      -webkit-transform: rotate(45deg);
    }
  }
</style>
