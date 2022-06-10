<template>
    <div class="imageResizerPreview">
      <button @click="changeImage">CHANGE IMAGE</button>
      <img :src="ctx.selectedImageSrc" :width="ctx.width" :height="ctx.height"/>
    </div>
</template>

<script>

export default {
  
  inject: ['imageResizerPreviewCtx'],
  mounted() { 
    console.log('mounted: imageResizerPreview');
  },

  computed: {
    ctx() {
      return this.imageResizerPreviewCtx;
    },
  },
  methods: {
    changeImage() {
      console.log('called changeImage: imageResizerPreview');
      this.ctx.showImageSelector();
    },
  },
}
</script>
<style scoped>
button {
  margin: 0 0 0 15px;
  width: 100px;
  background-color: #eee;
  border: 1px solid #333;
  border-radius: 5px;
}

</style>

