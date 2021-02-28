<template>
  <div class="input-wrapper">
    <label class="form__label" for="object-name">
      Object name
    </label >
    <input
      id="object-name"
      name="object-name"
      type="text"
      class="form__input"
      required
    >
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'FormInput',
});
</script>

<style lang="scss">
  .form__input {
    font-size: 0.875rem;
    height: 2rem;
    border-radius: 4px;
    border: 1px solid #888;
    padding: 0 .5rem;
    width: 12.5rem;
    &:hover {
      border-color: #000;
    }
    &:focus {
      outline: none;
      border-color: #000;
    }
  }
  .input-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
</style>
