<template>
    <div class="imageResizerPreview">
      <div><img :src="ctx.selectedImageSrc" :width="ctx.width" :height="ctx.height"/></div>
      <div><button @click="changeImage">CHANGE IMAGE</button></div>
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
<style scoped>
button {
  margin: 10px 0 0;
}
</style>

