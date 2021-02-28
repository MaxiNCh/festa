<template>
  <form class="form">
    <FormInput></FormInput>
    <FormSelect></FormSelect>
    <FormFile></FormFile>
    <FormSubmitButton></FormSubmitButton>
  </form>
</template>

<script lang="ts">
import Vue from 'vue';
import FormInput from './FormInput.vue';
import FormSelect from './FormSelect.vue';
import FormFile from './FormFile.vue';
import FormSubmitButton from './FormSubmitButton.vue';

export default Vue.extend({
  name: 'FormItem',
  components: {
    FormInput,
    FormSelect,
    FormFile,
    FormSubmitButton,
  },
  data() {
    return {

    };
  },
});
</script>

<style lang="scss">
  .form {
    width: 21rem;
    &__label {
      font-size: 0.875em;
    }
    & > * {
      margin-top: 1.2rem;
    }
  }
</style>
