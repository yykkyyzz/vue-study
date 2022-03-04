<!-- "Standard" way of doing things -->
<template>
  <CounterPanel
      :count="count"
      @change-count="changeCount"
    />
    <hr/>
    <div>
      <button @click="clearCount">clear</button>
    </div>
</template>

<script>
import CounterPanel from './components/CounterPanel.vue';
export default {
  components: {
    CounterPanel,
  },
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    changeCount(count) {
      this.count = count;
    },
    clearCount() {
      this.count = 0;
    },
  },
}

</script>