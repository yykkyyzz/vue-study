<!-- "Standard" way of doing things -->
<template>
  <div>
  <ImageResizer/>
  <ImageSelector v-if="imageState.imageSelectorVisible"/>
  </div>
</template>

<script>
import ImageResizer from '../components/14/ImageResizer.vue';
import ImageSelector from '../components/14/ImageSelector.vue';
import Counter from '../models/14/Counter.js';
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
      return readonly(widthCounter); // ★変更不可にして公開
    },
    get heightCounter() {
      return readonly(heightCounter); // ★変更不可にして公開
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

const makeImageResizerPreviewCtx = (widthCounter, heightCounter, imageState) => {
  return {
    get width() {
      return widthCounter.count; // ★カウント数のみを公開
    },
    get height() {
      return heightCounter.count; // ★カウント数のみを公開
    },
    get selectedImageSrc() {
      return imageState.selectedImageSrc; // ☆状態データの参照
    },
    showImageSelector() {
      imageState.imageSelectorVisible = true; // ☆状態データの更新
    },
  };
};

const makeImageSelectorCtx = (imageList, imageState) => {
  return {
    get imageList() {
      return Vue.readonly(imageList); // ★変更不可にして公開
    },
    updateSelectedImageSrc(imageSrc) {
      imageState.selectedImageSrc = imageSrc; // 選択中画像URLの変更
    },
    hideImageSelector() {
      imageState.imageSelectorVisible = false; // ダイアログの非表示
    },
  };
};

export default {
  
  components: {
    ImageResizer,
    ImageSelector,
  },
  data() {
    const options = { min: 50, max: 1000, step: 50 };
    return {
      widthCounter: new Counter(200, options),
      heightCounter: new Counter(150, options),
      imageState: {
        selectedImageSrc: imageList[0],
        imageSelectorVisible: false,
      },
      bar: false,
    };
  },
  provide() {
    const { widthCounter, heightCounter, imageState } = this;

    const imageResizerToolsCtx = makeImageResizerToolsCtx(widthCounter, heightCounter);
    const imageResizerPreviewCtx = makeImageResizerPreviewCtx(
      widthCounter,
      heightCounter,
      imageState,
    );
    const imageSelectorCtx = makeImageSelectorCtx(imageList, imageState);

    return {
      imageResizerToolsCtx,
      imageResizerPreviewCtx,
      imageSelectorCtx,
    };
  },
  
}

</script>