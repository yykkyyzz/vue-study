<template>
    <div v-if="loadingHandleCtx.loadingVisible" class="loadingstyle">
      now loading...
    </div>
</template>

<script>

export default {
  inject: ['loadingHandleCtx']
}
</script>

<style scoped>
.loadingstyle {
  padding:20px;
  position: fixed;
  top: 0;
  left: 0;
  width:100%;
  height:100%;
  color:#fff;
  background-color: rgba(0,0,0,0.5);
  text-align:center;
}
</style>

