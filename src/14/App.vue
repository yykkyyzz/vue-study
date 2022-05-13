<!-- "Standard" way of doing things -->
<template>
  <div>
  <ImageResizer/>
  <ImageSelector v-if="imageState.imageSelectorVisible"/>
  </div>
</template>

<script>

import CounterPanel from '../components/13/CounterPanel.vue';
import Counter from '../assets/13/js/Counter.js';
const imageList = ['/assets/images/360192black.png','/assets/images/360192yellow.png'];
const makeImageResizerToolsCtx = (...) => {...};
const makeImageResizerPreviewCtx = (...) => {...};
const makeImageSelectorCtx = (...) => {...};

const makeImageResizerToolsCtx = (widthCounter, heightCounter) => {
  return {
    get widthCounter() {
      return Vue.readonly(widthCounter); // ★変更不可にして公開
    },
    get heightCounter() {
      return Vue.readonly(heightCounter); // ★変更不可にして公開
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

export default {
  
  components: {
    CounterPanel,
  },
  data() {
    const options = {
      min: 50,
      max: 1000,
      step: 50,
    };
    
    return {
      widthCounter: new Counter(200, options),
      heightCounter: new Counter(150, options),
      imageState: {
        selectedImageSrc: imageList[0],
        imageSelectorVisible: false,
      },
    };
  },
  provide() {
    const { widthCounter, heightCounter, imageState } = this;

    return {
      imageResizerToolsCtx: makeImageResizerToolsCtx(
        widthCounter, heightCounter
      ),
      imageResizerPreviewCtx: makeImageResizerPreviewCtx(
        widthCounter, heightCounter, imageState,
      ),
      imageSelectorCtx: makeImageSelectorCtx(
        imageState, imageList
      ),
    }
  },
  watch: {
    alertVisible(visible) {
      if(visible) {
        setTimeout(() => (this.alertVisible = false), 1500);
      }
    }
  },
  methods: {
    changeWidth(width) {
      if( width < this.heightCounter.count) {
        this.alertVisible = true;
        return;
      }
      this.widthCounter.count = width;
    },
    changeHeight(height) {
      if( this.widthCounter.count < height) {
        this.alertVisible = true;
        return;
      }
      this.heightCounter.count = height;
    },
    clearCount() {
      this.widthCounter.count = this.widthCounter.initialCount;
      this.heightCounter.count = this.heightCounter.initialCount;
    },
  },
  
}

</script>