<!-- "Standard" way of doing things -->
<template>
  <button @click="show = true"> Login </button>
  <login-popup v-if="show" />
</template>

<script>
//import LoginPopup from './components/LoginPopup.vue'
import { defineAsyncComponent } from 'vue'
export default {
  //components: { LoginPopup },
  components: { 
   "LoginPopup" : defineAsyncComponent(() => import('./components/LoginPopup.vue'))
  },
  data() {
    return {
      show: false
    }
  }
}
</script>