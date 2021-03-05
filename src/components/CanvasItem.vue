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
        console.log('watch...');
        this.drawImages();
      },
    },
  },
  mounted() {
    this.canvas.addEventListener('mousedown', (e) => {
      this.$emit('grab-image', e);
    });
    this.canvas.addEventListener('mouseup', (e) => {
      e.stopPropagation();
      this.$emit('drop-image', e);
    });
  },
  methods: {
    drawImages() {
      this.ctx.clearRect(0, 0, 500, 500);
      this.canvasImages().forEach((img) => {
        const imageEl = new Image();
        imageEl.src = img.src;
        imageEl.onload = () => {
          this.ctx.drawImage(imageEl, img.x, img.y, 128, 128);
        };
      });
    },
    canvasImages() {
      return (this.images as Image[]).filter((img: Image) => img.container === this.$attrs.id);
    },
  },
});

interface Image {
  name: string;
  src: string;
  container: string;
  id: string;
  x: number;
  y: number;
  newX: number;
  newY: number;
  shiftX: number;
  shiftY: number;
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
