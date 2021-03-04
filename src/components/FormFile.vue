<template>
  <div class="file-wrapper">
    <label for="image-file">
      <input
        id="image-file"
        name="image-file"
        type="file"
        ref="image-file"
        class="form__file"
        multiple
        required
        accept=".svg,.png"
        @change="uploadImages"
      >
    </label >
    <div class="file-error">
      <span v-if="sizeError" >
        * File size too large.
      </span>
      <span v-if="typeError" >
        * File type is not allowed.
      </span>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'FormFile',
  data() {
    return {
      files: [] as File[],
      typeError: false,
      sizeError: false,
    };
  },
  computed: {
    inputEl(): HTMLInputElement {
      return document.getElementById('image-file') as HTMLInputElement;
    },
  },
  methods: {
    uploadImages() {
      if (this.inputEl.files !== null) {
        this.files = Array.from(this.inputEl.files);
        this.$emit('update-error-status', { fileError: false });
        this.sizeError = false;
        this.typeError = false;
        this.files.forEach((file) => {
          this.validate(file);
        });
      }
      if (this.sizeError || this.typeError) {
        this.$emit('update-error-status', { fileError: true });
      } else {
        this.$emit('upload-images', this.files);
      }
    },
    validate(file: File) {
      const fileName: string = file.name;
      const fileSize: number = file.size;
      const allowedExtensions = ['svg', 'png'];
      const sizeLimit = 500000;
      const fileExtension: string = fileName.split('.').pop() ?? '';
      if (!allowedExtensions.includes(fileExtension)) {
        this.typeError = true;
      }
      if (fileSize > sizeLimit) {
        this.sizeError = true;
      }
    },
  },
});

</script>

<style lang="scss">
.file-wrapper {
  position: relative;
}
.file-error {
  position: absolute;
  right: -3rem ;
  bottom: -.5rem;
  color: #f00;
  font-size: .75rem;
}
.form__file::-webkit-file-upload-button {
  display: none;
}
.form__file {
  &:hover {
    outline: none;
    cursor: pointer;
  }
  &:focus {
    outline: none;
  }
  &::before {
    content: 'Select image';
    display: inline-block;
    border-radius: 4px;
    text-transform: uppercase;
    font-size: 12px;
    line-height: 32px;
    text-align: center;
    width: 10rem;
    background: #0094FF;
    color: #fff;
    margin-right: .5rem;
  }
  &:hover::before {
    cursor: pointer;
    opacity: .8;
  }
  &:focus::before {
    outline: none;
  }
}
.form__file:hover::before {
  border-color: black;
}
</style>
