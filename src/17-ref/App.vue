<!-- "Standard" way of doing things -->
<template>
  <div>
      <button @click="countUp">+</button> {{count.x}} {{count.y}}
      <p>{{count}}</p>
      <!-- count { "x": 100, "y": 150 } -->
      <p>{{count.value}}</p>
      <!-- count.value 何も表示されない -->
  </div>
</template>

<script>
import { ref } from 'vue';
  export default {
    setup() {
        
      const count = ref({ x: 100, y: 150 });

      // .valueの値は変えず、オブジェクトのプロパティの値のみを変更している
      const countUp = () => {
        count.value.x = count.value.x + 10;
        count.value.y = count.value.y + 10;
      };

      // テンプレートへ公開
      return { count, countUp };
    }
  }
</script>
