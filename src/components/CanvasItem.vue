<template>
  <canvas class="canvas">
  </canvas>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'CanvasItem',
  props: {
    images: {
      type: Array,
      default(): Image[] {
        return [];
      },
    },
  },
  data() {
    return {
      isGrabbing: false,
      grabbedImage: null as null | Image,
    };
  },
  computed: {
    canvas(): HTMLCanvasElement {
      return document.getElementById(this.$attrs.id) as HTMLCanvasElement;
    },
    ctx(): CanvasRenderingContext2D {
      return this.canvas.getContext('2d') as CanvasRenderingContext2D;
    },
  },
  watch: {
    images: {
      handler() {
        this.drawImages();
      },
      deep: true,
    },
  },
  mounted() {
    this.canvas.addEventListener('mousedown', (e) => {
      this.emitGrabbedImage(e);
    });
    this.canvas.addEventListener('mouseup', (e) => {
      this.$emit('drop-image', e);
    });
  },
  methods: {
    emitGrabbedImage(e: MouseEvent): void {
      const images = this.filteredImages();
      for (let i = images.length - 1; i >= 0; i -= 1) {
        const image = images[i];
        if (this.isMouseOnImage(e.offsetX, e.offsetY, image.x, image.y)) {
          const grabbedId = images[i].id;
          const coordinates = this.calculateCoordinates(images[i], e);
          this.$emit('grab-image', { grabbedId, coordinates });
          break;
        }
      }
    },
    drawImages() {
      this.ctx.clearRect(0, 0, 500, 500);
      this.filteredImages().forEach((img) => {
        const imageEl = new Image();
        imageEl.src = URL.createObjectURL(img.file);
        imageEl.onload = () => {
          this.ctx.drawImage(imageEl, img.x, img.y, 128, 128);
        };
      });
    },
    filteredImages() {
      return (this.images as Image[]).filter((img: Image) => img.container === this.$attrs.id);
    },
    calculateCoordinates(image: Image, e: MouseEvent): {
      x: number;
      y: number;
      shiftX: number;
      shiftY: number;
    } {
      const shiftX = image.x - e.offsetX;
      const shiftY = image.y - e.offsetY;
      const x = e.pageX + shiftX;
      const y = e.pageY + shiftY;
      return {
        x,
        y,
        shiftX,
        shiftY,
      };
    },
    isMouseOnImage(mouseX: number, mouseY: number, imgX: number, imgY: number) {
      if (mouseX > imgX && mouseX < imgX + 128
      && mouseY > imgY && mouseY < imgY + 128) {
        return true;
      }
      return false;
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
.canvas {
  position: relative;
  border: 3px solid #888888;
  background-color: #eeeeee;
}

</style>
