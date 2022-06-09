
<template>
  <div>
      <slot :dialog-visible="state.dialogVisible"/>
  </div>
</template>

<script>

export default {
  
  data() {
    return {
      state: {
        dialogVisible: false,
      },
    };
  },
  provide() {
    const  state  = this;
    return {
      dialogHandleCtx: {
        get dialogVisible() {
          return state.dialogVisible;
        },
        showDialog() {
          state.dialogVisible = true;
        },
        hideDialog() {
          state.dialogVisible = false;
        },
      },
    };
  },
}
</script>
<style scoped>
</style>
