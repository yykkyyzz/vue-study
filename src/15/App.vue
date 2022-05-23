<!-- "Standard" way of doing things -->
<template>
  <div>
    <LoadingHandleProvider>
      <MyAppComponent/>
      <LoadingScreen/>
    </LoadingHandleProvider>  </div>
</template>

<script>
import LoadingScreen from '../components/15/LoadingScreen.vue';
import LoadingHandleProvider from '../components/15/LoadingHandleProvider.vue';
import MyAppComponent from '../components/15/MyAppComponent.vue';
import { readonly } from 'vue';


export default {
  
  components: {
    LoadingHandleProvider,
    LoadingScreen,
    MyAppComponent,
  },
  
}

</script>