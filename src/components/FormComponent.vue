<template>
  <form
    id="form"
    class="form"
    @submit.prevent="submit"
  >
    <FormInput
      :images="images"
      @update-name="updateName"
      @update-error-status="updateErrorStatus"
    />
    <FormSelect @update-container="updateContainer"/>
    <FormFile
      @upload-images="updateImages"
      @update-error-status="updateErrorStatus"
    />
    <FormSubmitButton/>
  </form>
</template>

<script lang="ts">
import Vue from 'vue';
import FormInput from './FormInput.vue';
import FormSelect from './FormSelect.vue';
import FormFile from './FormFile.vue';
import FormSubmitButton from './FormSubmitButton.vue';

export default Vue.extend({
  name: 'FormComponent',
  components: {
    FormInput,
    FormSelect,
    FormFile,
    FormSubmitButton,
  },
  props: {
    images: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      files: [] as File[],
      imageName: '',
      imageContainer: '',
      inputError: false,
      fileError: false,
    };
  },
  methods: {
    updateName(newName: string): void {
      this.imageName = newName;
    },
    updateContainer(container: string): void {
      this.imageContainer = container;
    },
    updateImages(files: File[]): void {
      this.files = files;
    },
    updateErrorStatus(status: { fileError: boolean; inputError: boolean}) {
      if (Object.keys(status)[0] === 'fileError') {
        this.fileError = status.fileError;
      }
      if (Object.keys(status)[0] === 'inputError') {
        this.inputError = status.inputError;
      }
    },
    submit(e: Event) {
      if (!this.inputError && !this.fileError && this.files.length > 0) {
        const imagesToSubmit: NewImage[] = [];
        for (let i = 0; i < this.files.length; i += 1) {
          const newImage: NewImage = {
            name: '',
            src: '',
            container: '',
          };
          newImage.container = this.imageContainer;
          newImage.src = URL.createObjectURL(this.files[i]);
          if (this.files.length === 1 && i === 0) {
            newImage.name = this.imageName;
          } else {
            newImage.name = `${this.imageName}-${i + 1}`;
          }
          imagesToSubmit.push(newImage);
        }
        (e.target as HTMLFormElement).reset();
        this.$emit('add-images', imagesToSubmit);
      }
    },
  },
});

interface NewImage {
  name: string;
  src: string;
  container: string;
}

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
