<template>
      <div class="imageSelector">
      <p>click image</p>
      <ul>
        <li v-for="imageSrc in ctx.imageList">
          <button  @click="changeSelectedImage(imageSrc)">
            <img :src="imageSrc"/>
          </button>
        </li>
      </ul>
    </div>
</template>

<script>
export default {
  inject: ['imageSelectorCtx'],
  mounted() { 
    console.log('mounted: ImageSelector');
  },

  computed: {
    ctx() {
      return this.imageSelectorCtx;
    },
  },
  methods: {
    changeSelectedImage(imageSrc) {
      
      this.ctx.updateSelectedImageSrc(imageSrc);
      this.ctx.hideImageSelector();
    },
  },
}
</script>
<style scoped>
.imageSelector {
  width: 100%;
  height: 30%;
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translate(-50%,-50%);
  background-color: #eee;
}
.imageSelector ul {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  width: 100%;
}
.imageSelector ul li {
  width: 20%;
  list-style: none;
  padding: 0 5px;
}
.imageSelector ul li button img {
  width: 100%;
}
.closebtn {
  background-color: #fff;
  color: #333;
  border: 1px solid #333;
  border-radius: 5px;
  box-shadow: none;
  width: 100px;
  height: 20px;
}
.text {
  font-size: 18px;
  font-weight: bold;
}
</style>

