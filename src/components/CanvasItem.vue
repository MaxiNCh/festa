<template>
  <div class="canvas-wrapper">
    <RemoveBtn
      v-if="cursorOnImage"
      :offsetX="buttonOffsetX"
      :offsetY="buttonOffsetY"
      @remove-image="removeImage"
    />
    <canvas width="500" height="500" :id="canvasId" class="canvas">
    </canvas>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import RemoveBtn from './RemoveButton.vue';

export default Vue.extend({
  name: 'CanvasItem',
  components: {
    RemoveBtn,
  },
  props: {
    images: {
      type: Array,
      default(): Image[] {
        return [];
      },
    },
    canvasId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      cursorOnImage: false,
      imageId: '',
      buttonOffsetX: -200,
      buttonOffsetY: -200,
    };
  },
  computed: {
    canvas(): HTMLCanvasElement {
      return document.getElementById(this.canvasId) as HTMLCanvasElement;
    },
    ctx(): CanvasRenderingContext2D {
      return this.canvas.getContext('2d') as CanvasRenderingContext2D;
    },
    removeBtn(): HTMLButtonElement {
      return document.getElementById('remove-btn') as HTMLButtonElement;
    },
  },
  watch: {
    images: {
      handler() {
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
    this.canvas.addEventListener('mousemove', (e) => {
      this.canvas.style.cursor = 'default';
      this.cursorOnImage = false;
      for (let i = 0; i < this.images.length; i += 1) {
        if (this.isCursorOnImage(
          e.offsetX,
          e.offsetY,
          (this.images[i] as Image).x,
          (this.images[i] as Image).y,
        ) && this.canvasId === (this.images[i] as Image).container) {
          this.cursorOnImage = true;
          this.buttonOffsetX = (this.images[i] as Image).x + 113;
          this.buttonOffsetY = (this.images[i] as Image).y + 15;
          this.imageId = (this.images[i] as Image).id;
          this.canvas.style.cursor = 'pointer';
        }
      }
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
    removeImage() {
      this.cursorOnImage = false;
      this.$emit('remove-image', this.imageId);
    },
    canvasImages(): Image[] {
      return (this.images as Image[]).filter((img: Image) => img.container === this.canvasId);
    },
    isCursorOnImage(mouseX: number, mouseY: number, imgX: number, imgY: number) {
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
  background-color: #eeeeee;
  height: 100%;
  width: 100%;
  &-wrapper {
    position: relative;
    box-sizing: content-box;
    border: 3px solid #888888;
    width: 500px;
    height: 500px;
    display: inline-block;
  }
}

</style>
