
<template>
  <div>
      DialogHandleProvider: {{state.dialogVisible}}<slot :foo="state.dialogVisible"/>
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
  mounted() { 
    console.log('mounted: DialogHandleProvider');
  },

  provide() {
    const  state  = this.state; // ここの書き方が const state = this だったからできなかった
    
    return {
      dialogHandleCtx: {
        get dialogVisible() {
          console.log('called get dialogVisible : DialogHandleProvider: ' + state.dialogVisible);
          return state.dialogVisible;
        },
        showDialog() {
          state.dialogVisible = true;
          console.log('called showDialog: DialogHandleProvider '+ state.dialogVisible);
        },
        hideDialog() {
          state.dialogVisible = false;
            console.log('called hideDialog: DialogHandleProvider '+ state.dialogVisible);
        },
      },
    };
  },
}
</script>
<style scoped>
</style>
