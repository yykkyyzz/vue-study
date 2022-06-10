<!-- "Standard" way of doing things -->
<template>
  <div>
      <DialogHandleProvider v-slot="{foo}">
      <AppHandleProvider>
        <ImageResizer>
          <template v-slot:tools><ImageResizerTools/></template>
          <template v-slot:preview><ImageResizerPreview/></template>
        </ImageResizer>
        <ImageSelector v-if="foo"/>
      </AppHandleProvider>
    </DialogHandleProvider>
  </div>
</template>

<script>
import DialogHandleProvider from '../components/15-2/DialogHandleProvider.vue';
import AppHandleProvider from '../components/15-2/AppHandleProvider.vue';
import ImageResizer from '../components/15-2/ImageResizer.vue';
import ImageResizerTools from '../components/15-2/ImageResizerTools.vue';
import ImageResizerPreview from '../components/15-2/ImageResizerPreview.vue';
import ImageSelector from '../components/15-2/ImageSelector.vue';

export default {

  components: {
    DialogHandleProvider,
    AppHandleProvider,
    ImageResizer,
    ImageResizerTools,
    ImageResizerPreview,
    ImageSelector,
  }
  
}

</script>
