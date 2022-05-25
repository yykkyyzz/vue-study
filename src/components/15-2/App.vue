<!-- "Standard" way of doing things -->
<template>
  <div>
      <DialogHandleProvider v-slot="{dialogVisible}">
      <AppHandleProvider>
        <ImageResizer>
          <template v-slot:tools><ImageResizerTools/></template>
          <template v-slot:preview><ImageResizerPreview/></template>
        </ImageResizer>
        <ImageSelector v-if="dialogVisible"/>
      </AppHandleProvider>
    </DialogHandleProvider>
  </div>
</template>

<script>
import { DialogHandleProvider } from './DialogHandleProvider.vue';
import { AppHandleProvider } from './AppHandleProvider.vue';
import { ImageResizer } from './ImageResizer.vue';
import { ImageResizerTools } from './ImageResizerTools.vue';
import { ImageResizerPreview } from './ImageResizerPreview.vue';
import { ImageSelector } from './ImageSelector.vue';


export const App = {

  components: {
    DialogHandleProvider,
    AppHandleProvider,
    ImageResizer,
    ImageResizerTools,
    ImageResizerPreview,
    ImageSelector
  },
  
}

</script>