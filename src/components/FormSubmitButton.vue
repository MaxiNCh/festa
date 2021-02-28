<template>
  <div>
    <button class="form__button" type="submit">Add object</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'FormSubmitButton',
});
</script>

<style lang="scss">
.form__button {
    width: 10rem;
    border-radius: 4px;
    border: none;
    padding: 0;
    background-color: #03AC00;
    line-height: 32px;
    font-size: 12px;
    text-align: center;
    text-transform: uppercase;
    color: #FFF;
    &:hover {
      cursor: pointer;
      opacity: .8;
    }
    &:focus {
      outline: none;
    }
  }
</style>
