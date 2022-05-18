<template>
  <div>
      <ImageResizerTools/>
      <hr/>
      <ImageResizerPreview/>
    </div>
</template>

<script>
import ImageResizerTools from './ImageResizerTools.vue';
import ImageResizerPreview from './ImageResizerPreview.vue';

export default {
  components: {
    ImageResizerTools,
    ImageResizerPreview,
  },
}
</script>
<style scoped>
.imageResizer {
  position: relative;
}
</style>
