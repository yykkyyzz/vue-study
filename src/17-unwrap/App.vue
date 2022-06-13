<!-- "Standard" way of doing things -->
<template>
  <div>
    <button @click="toggleVisible">toggle visible</button>
    <button @click="moveRight">move right</button>
    <hr/>
    <!-- .valueの記述はない -->
    <span v-if="state.visible">
      position: {{state.position.x}} {{state.position.y}}
    </span>
  </div>
</template>

<script>
import { ref } from 'vue';
import { reactive } from 'vue';
import { computed } from 'vue';

  export default {
    setup() {
      // 状態データと算出プロパティをstateに束ねる
      const state = {
        visible: ref(true),
        position: ref({ x: 100, y: 150 }),
        size: ref({ width: 300, height: 400 }),
        areaSize: computed(() => state.size.value.width * state.size.value.height),
      };
      const toggleVisible = () => (state.visible.value = !state.visible.value);
      const moveRight = () => {
        state.position.value.x++;
      };

      // テンプレート公開前にstateをreactiveで再生成
      return { state: reactive(state), toggleVisible, moveRight };
    },
  }
</script>
