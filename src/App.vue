<template>
  <div id="app">
    <GrabbedImage
      v-if="showImage"
      :src="imageSrc"
      :imageX="imageCoordinates.x"
      :imageY="imageCoordinates.y"
    />
    <BlockItem :title="'Container A'">
      <CanvasItem
        :id="'containerA'"
        :width="500"
        :height="500"
        :images="images"
        @grab-image="grabImage"
        @drop-image="dropImage"
      />
    </BlockItem>
    <BlockItem :title="'Container B'">
      <CanvasItem
        :id="'containerB'"
        :width="500"
        :height="500"
        :images="images"
        @grab-image="grabImage"
        @drop-image="dropImage"
      />
    </BlockItem>
    <BlockItem :title="'Add new object'">
      <FormComponent
        :images="images"
        @add-images="addImages"
      />
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
import GrabbedImage from './components/GrabbedImage.vue';

export default Vue.extend({
  name: 'App',
  components: {
    CanvasItem,
    BlockItem,
    FormComponent,
    LogField,
    GrabbedImage,
  },
  data() {
    return {
      images: [] as Image[],
      showImage: false,
      imageSrc: '',
      imageCoordinates: {
        x: 0,
        y: 0,
      },
      shiftImage: {
        x: 0,
        y: 0,
      },
      imageId: '',
    };
  },
  mounted() {
    document.addEventListener('mousemove', (e) => {
      if (this.showImage === true) {
        this.imageCoordinates.x = e.pageX + this.shiftImage.x;
        this.imageCoordinates.y = e.pageY + this.shiftImage.y;
      }
    });
    document.addEventListener('mouseup', () => {
      this.showImage = false;
    });
  },
  methods: {
    addImages(images: Image[]) {
      images.forEach((image) => {
        const x: number = Math.round((Math.random() * 372));
        const y: number = Math.round((Math.random() * 372));
        const id = `_${Math.random().toString(36).substr(2, 9)}`;
        const newImage = {
          ...image, x, y, id, isGrabbed: false,
        };
        this.images.push(newImage);
      });
    },
    grabImage({ grabbedId, coordinates }: DataForGrabbedImage) {
      const index = this.images.findIndex((img) => img.id === grabbedId);
      this.imageSrc = URL.createObjectURL(this.images[index].file);
      this.imageId = grabbedId;
      this.imageCoordinates.x = coordinates.x;
      this.imageCoordinates.y = coordinates.y;
      this.shiftImage.x = coordinates.shiftX;
      this.shiftImage.y = coordinates.shiftY;
      setTimeout(() => {
        this.showImage = true;
      }, 0.1);
    },
    dropImage(e: MouseEvent) {
      if (this.showImage === true) {
        const images = [...this.images];
        const movedImage: Image[] = images.splice(
          images.findIndex((img) => img.id === this.imageId), 1,
        );
        movedImage[0].x = e.offsetX + this.shiftImage.x;
        movedImage[0].y = e.offsetY + this.shiftImage.y;
        movedImage[0].container = (e.target as HTMLElement).id;
        images.push(movedImage[0]);
        this.images = [...images];
      }
      this.showImage = false;
    },
  },
});

interface DataForGrabbedImage {
  grabbedId: string;
  coordinates: { x: number; y: number; shiftX: number; shiftY: number };
}

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
#app {
  display: grid;
  grid-template-columns: 31.25em 31.25em;
  justify-content: space-between;
  box-sizing: border-box;
  padding: 7em calc(50% - 600px);
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: relative;
}
* {
  box-sizing: border-box;
}
</style>
