<template>
  <div id="app">
    <GrabbedImage
      v-for="image of this.grabbedImages"
      :src="image.src"
      :imageX="image.absX"
      :imageY="image.absY"
      :key="image.id"
    />
    <BlockItem :title="'Container A'">
      <CanvasItem
        :canvasId="'containerA'"
        :images="images"
        @grab-image="grabImage"
        @drop-image="dropImage"
        @remove-image="removeImage"
      />
    </BlockItem>
    <BlockItem :title="'Container B'">
      <CanvasItem
        :canvasId="'containerB'"
        :images="images"
        @grab-image="grabImage"
        @drop-image="dropImage"
        @remove-image="removeImage"
      />
    </BlockItem>
    <BlockItem :title="'Add new object'">
      <FormComponent
        :images="images"
        @add-images="addImages"
      />
    </BlockItem>
    <BlockItem :title="'Log'">
      <LogField :actions="actions"/>
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
      isImageGrabbed: false,
      isCtrlPressed: false,
      isMoving: false,
      actionTypes: [
        'add',
        'move',
        'delete',
      ],
      actions: [] as Action[],
    };
  },
  computed: {
    grabbedImages(): Image[] {
      return this.images.filter((img) => img.isGrabbed === true);
    },
  },
  mounted() {
    document.addEventListener('mousemove', (e) => {
      if (this.isCtrlPressed === false
        || (this.isCtrlPressed === true && this.isImageGrabbed === true)) {
        for (let i = 0; i < this.images.length; i += 1) {
          if (this.images[i].isGrabbed) {
            this.isMoving = true;
            this.images[i].absX = e.pageX + this.images[i].shiftX;
            this.images[i].absY = e.pageY + this.images[i].shiftY;
          }
        }
      }
    });
    document.addEventListener('mouseup', () => {
      for (let i = 0; i < this.images.length; i += 1) {
        this.images[i].isGrabbed = false;
      }
      this.isImageGrabbed = false;
    });
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Control') {
        this.isCtrlPressed = true;
      }
    });
    document.addEventListener('keyup', (e) => {
      if (e.key === 'Control') {
        this.isCtrlPressed = false;
        for (let i = 0; i < this.images.length; i += 1) {
          this.images[i].isGrabbed = false;
        }
      }
    });
  },
  methods: {
    addImages(images: Image[]) {
      images.forEach((image) => {
        const x: number = Math.round((Math.random() * 372));
        const y: number = Math.round((Math.random() * 372));
        const id = `_${Math.random().toString(36).substr(2, 9)}`;
        const newImage: Image = {
          ...image,
          id,
          x, // Coordinates of image in canvas
          y, // Coordinates of image in canvas
          absX: 0, // Absolute coordinates in document
          absY: 0, // Absolute coordinates in document
          shiftX: 0, // Shift of mouse relativly to image
          shiftY: 0, // Shift of mouse relativly to image
          isGrabbed: false,
        };
        this.images.push(newImage);
        const newAction: Action = {
          date: new Date(),
          id: `_${Math.random().toString(36).substr(2, 9)}`,
          objectName: newImage.name,
          type: this.actionTypes[0],
          container1: newImage.container,
          container2: '',
        };
        this.actions.push(newAction);
      });
    },
    removeImage(id: string) {
      console.log('Remove image id: ', id);
      const index = this.images.findIndex((img) => img.id === id);
      const newAction = {
        date: new Date(),
        id: `_${Math.random().toString(36).substr(2, 9)}`,
        objectName: this.images[index].name,
        type: this.actionTypes[2],
        container1: this.images[index].container,
        container2: '',
      };
      this.actions.push(newAction);
      this.images.splice(index, 1);
    },
    grabImage(e: MouseEvent) {
      /**
       * Check if we already grabed an image.
       * If so we should only grabe following images in the same container;
       */
      let container = '';
      const indexC = this.images.findIndex((img) => img.isGrabbed);
      if (indexC !== -1) {
        container = this.images[indexC].container;
      }
      /**
       * The index of images which we are grabbing next
       */
      let index = -1;
      /**
       * By this for loop we grab the only image, which is over other images,
       * if several images simultaneously under the cursor.
       */
      for (let i = this.images.length - 1; i >= 0; i -= 1) {
        if (this.isCursorOnImage(e.offsetX, e.offsetY, this.images[i].x, this.images[i].y)) {
          if ((container === ''
           || ((e.target as HTMLCanvasElement).id === container
           && this.images[i].container === container))) {
          // if ((container === '' || this.images[i].container === container)) {
            this.isImageGrabbed = true;
            if (this.images[i].isGrabbed !== true) {
              this.updateCoordinates(e, i);
              index = i;
              break;
            }
          }
        }
      }
      if (index !== -1) {
        this.updateShift(e, index);
      }
    },
    /**
     * Update distance between cursor and images to keep initial order
     */
    updateShift(e: MouseEvent, index: number) {
      for (let i = 0; i < this.images.length; i += 1) {
        if (this.images[i].isGrabbed === true && index !== i) {
          this.images[i].shiftX = this.images[i].x - e.offsetX;
          this.images[i].shiftY = this.images[i].y - e.offsetY;
          this.images[i].absX = e.pageX + this.images[i].shiftX;
          this.images[i].absY = e.pageY + this.images[i].shiftY;
        }
      }
    },
    /**
     * Upadge absolute coordinages and shift of just grabbed image
     */
    updateCoordinates(e: MouseEvent, i: number) {
      this.images[i].isGrabbed = true;
      this.images[i].shiftX = this.images[i].x - e.offsetX;
      this.images[i].shiftY = this.images[i].y - e.offsetY;
      this.images[i].absX = e.pageX + this.images[i].shiftX;
      this.images[i].absY = e.pageY + this.images[i].shiftY;
    },
    /**
     * Put object on canvas after moving
     */
    dropImage(e: MouseEvent) {
      if (this.isCtrlPressed === false || this.isMoving === true) {
        for (let i = 0; i < this.images.length; i += 1) {
          if (this.images[i].isGrabbed) {
            // Check if container changes after moving and add move action
            if (this.images[i].container !== (e.target as HTMLElement).id) {
              this.addMoveAction(
                this.images[i].name,
                this.images[i].container,
                (e.target as HTMLElement).id,
              );
            }
            this.images[i].x = e.offsetX + this.images[i].shiftX;
            this.images[i].y = e.offsetY + this.images[i].shiftY;
            this.images[i].container = (e.target as HTMLElement).id;
            this.images[i].isGrabbed = false;
            this.images.push(this.images.splice(i, 1)[0]);
            i -= 1;
          }
          this.isMoving = false;
        }
      }
      this.isImageGrabbed = false;
    },
    addMoveAction(name: string, container1: string, container2: string) {
      const newAction: Action = {
        date: new Date(),
        id: `_${Math.random().toString(36).substr(2, 9)}`,
        objectName: name,
        type: this.actionTypes[1],
        container1,
        container2,
      };
      this.actions.push(newAction);
    },
    /**
     * Check if cursor over an image
     */
    isCursorOnImage(mouseX: number, mouseY: number, imgX: number, imgY: number) {
      if (mouseX > imgX && mouseX < imgX + 128
      && mouseY > imgY && mouseY < imgY + 128) {
        return true;
      }
      return false;
    },
  },
});

interface DataForGrabbedImage {
  grabbedId: string;
  coordinates: { x: number; y: number; shiftX: number; shiftY: number };
}

interface Image {
  name: string;
  src: string;
  container: string;
  id: string;
  x: number; // Coordinates of image in canvas
  y: number; // Coordinates of image in canvas
  absX: number; // Absolute coordinates in document
  absY: number; // Absolute coordinates in document
  shiftY: number; // Shift of mouse relativly to image
  shiftX: number; // Shift of mouse relativly to image
  isGrabbed: boolean;
}

interface Action {
  date: Date;
  id: string;
  objectName: string;
  type: string;
  container1: string;
  container2: string;
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
