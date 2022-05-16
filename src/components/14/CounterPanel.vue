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
  },
}
</script>

