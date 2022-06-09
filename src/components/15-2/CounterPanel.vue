<template>
    <button @click="countDown">-</button>
    <input v-model.number.lazy="count" type="number" :min="counter.min" :max="counter.max"/>
    <button @click="countUp">+</button>
</template>

<script>
import Counter from '../../models/14/Counter.js';

export default {
  props: {
    counter: {
      type: Counter,
      required: true,
    },
  },
  emits: ['changeCount'],
  computed: {
    count: {
      get() {
        return this.counter.count;
      },
      set(newCount) {
        this.$emit('changeCount', newCount);
      },
    },
  },
  methods: {
    countDown() {
      this.$emit('changeCount', this.counter.stepDownValue);
    },
    countUp() {
      this.$emit('changeCount', this.counter.stepUpValue);
    },
  }
}
</script>
<style scoped>
button {
  margin: 0 0 0 15px;
  width: 100px;
  background-color: #eee;
  border: 1px solid #333;
  border-radius: 5px;
}
input {
  width: 120px;
}
</style>

