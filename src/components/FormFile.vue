<template>
  <div>
    <label for="object-file">
      <input
        id="object-file"
        name="object-file"
        type="file"
        class="form__file"
        required
        multiple>
    </label >
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'FormFile',
});
</script>

<style lang="scss">
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
