<template>
    <div class="imageResizerPreview">
      <button @click="changeImage">CHANGE IMAGE</button>
      <img :src="ctx.selectedImageSrc" :width="ctx.width" :height="ctx.height"/>
    </div>
</template>

<script>

export default {
  inject: ['imageResizerPreviewCtx'],
  computed: {
    ctx() {
      return this.imageResizerPreviewCtx;
    },
  },
  methods: {
    changeImage() {
      this.ctx.showImageSelector();
    },
  },
}
</script>

