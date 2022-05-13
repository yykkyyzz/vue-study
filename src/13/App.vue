<!-- "Standard" way of doing things -->
<template>
  <div>
    w: <CounterPanel
      :counter="widthCounter"
      @change-count="changeWidth"
    />
    </div>
    <div>
    h: <CounterPanel
      :counter="heightCounter"
      @change-count="changeHeight"
    />
    </div>
    <div>
      <button @click="clearCount">clear</button>
    </div>
    <hr/>
    <img src="./360192black.png" :width="widthCounter.count" :height="heightCounter.count"/>
    <p v-if="alertVisible">幅が高さ以上のサイズになるよう設定してください。</p>
</template>

<script>

import CounterPanel from '../components/13/CounterPanel.vue';
import Counter from '../assets/13/js/Counter.js';

export default {
  components: {
    CounterPanel,
  },
  data() {
    const options = {
      min: 50,
      max: 1000,
      step: 50,
    };
    return {
      widthCounter: new Counter(200, options),
      heightCounter: new Counter(150, options),
      alertVisible: false
    };
  },
  watch: {
    alertVisible(visible) {
      if(visible) {
        setTimeout(() => (this.alertVisible = false), 1500);
      }
    }
  },
  methods: {
    changeWidth(width) {
      if( width < this.heightCounter.count) {
        this.alertVisible = true;
        return;
      }
      this.widthCounter.count = width;
    },
    changeHeight(height) {
      if( this.widthCounter.count < height) {
        this.alertVisible = true;
        return;
      }
      this.heightCounter.count = height;
    },
    clearCount() {
      this.widthCounter.count = this.widthCounter.initialCount;
      this.heightCounter.count = this.heightCounter.initialCount;
    },
  }
}

</script>