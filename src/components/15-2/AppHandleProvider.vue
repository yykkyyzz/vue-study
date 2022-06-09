
<template>
  <div>
    <slot/>
  </div>
</template>

<script>
import Counter from '../../models/14/Counter.js';
import DialogHandleProvider from './DialogHandleProvider.vue';
import { readonly } from 'vue';

const imageList = [
  './assets/images/360192black.png',
  './assets/images/360192yellow.png',
  './assets/images/438292bluefff.png',
  './assets/images/438292pinkfff.png'
];

const makeImageResizerToolsCtx = (widthCounter, heightCounter) => {
  return {
    get widthCounter() {
      return readonly(widthCounter);
    },
    get heightCounter() {
      return readonly(heightCounter);
    },
    updateWidth(width) {
      widthCounter.count = width;
    },
    updateHeight(height) {
      heightCounter.count = height;
    },
    initializeSize() {
      widthCounter.count = widthCounter.initialCount;
      heightCounter.count = heightCounter.initialCount;
    },
  };
};

const makeImageResizerPreviewCtx = (widthCounter, heightCounter, imageState, dialogHandleCtx) => {
  return {
    get width() {
      return widthCounter.count;
    },
    get height() {
      return heightCounter.count;
    },
    get selectedImageSrc() {
      return imageState.selectedImageSrc;
    },
    showImageSelector() {
      dialogHandleCtx.showDialog();
    },
  };
};

const makeImageSelectorCtx = (imageList, imageState, dialogHandleCtx) => {
  return {
    get imageList() {
      return Vue.readonly(imageList);
    },
    updateSelectedImageSrc(imageSrc) {
      imageState.selectedImageSrc = imageSrc;
    },
    hideImageSelector() {
      dialogHandleCtx.hideDialog();
    },
  };
};

export default {

   components: {
    DialogHandleProvider,
  },

  inject: ['dialogHandleCtx'],

  data() {
    const options = { min: 50, max: 1000, step: 50 };
    return {
      widthCounter: new Counter(200, options),
      heightCounter: new Counter(150, options),
      imageState: {
        selectedImageSrc: imageList[0],
      },
    };
  },
  provide() {
    const { widthCounter, heightCounter, imageState, dialogHandleCtx } = this;

    const imageResizerToolsCtx = makeImageResizerToolsCtx(widthCounter, heightCounter);
    const imageResizerPreviewCtx = makeImageResizerPreviewCtx(
      widthCounter,
      heightCounter,
      imageState,
      dialogHandleCtx,
    );
    const imageSelectorCtx = makeImageSelectorCtx(imageList, imageState, dialogHandleCtx);

    return {
      imageResizerToolsCtx,
      imageResizerPreviewCtx,
      imageSelectorCtx,
    };
  },
}
</script>
<style scoped>
</style>
