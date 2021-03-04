<template>
  <div class="input-wrapper">
    <label class="form__label" for="image-name">
      Object name
    </label >
    <input
      id="image-name"
      v-model="imageName"
      name="image-name"
      type="text"
      class="form__input"
      required
      @change="validate"
    >
    <span v-if="nameExists" class="name-error">* Such name already exists.</span>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'FormInput',
  props: {
    images: {
      type: Array,
      required: true,
    },
  },
  computed: {
    nameExists(): boolean {
      return (this.images as Image[]).findIndex((img) => img.name === this.imageName) >= 0;
    },
  },
  data() {
    return {
      imageName: '',
    };
  },
  mounted() {
    (document.getElementById('form') as HTMLFormElement).addEventListener('reset', () => {
      this.imageName = '';
    });
  },
  methods: {
    validate() {
      this.$emit('update-error-status', { inputError: true });
      if (!this.nameExists) {
        this.$emit('update-error-status', { inputError: false });
        this.$emit('update-name', this.imageName);
      }
    },
  },
});

interface Image {
  name: string;
  file: File;
  container: string;
  x: number;
  y: number;
  id: string;
  isGrabbed: boolean;
}
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
    position: relative;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .name-error {
    position: absolute;
    right: -10rem;
    color: #f00;
    font-size: .75rem;
  }
</style>
