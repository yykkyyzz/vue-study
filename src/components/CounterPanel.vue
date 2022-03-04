<template>
  <button @click="countDown">-</button>
    {{count}}
    <button @click="countUp">+</button>
</template>

<script>
export default {
 props: {
    count: {
      type: Number,
      required: true,
    },
  },
  emits: ['changeCount'],
  methods: {
    countDown() {
      if (this.count > 0) {
        this.$emit('changeCount', this.count - 1);
      }
    },
    countUp() {
      this.$emit('changeCount', this.count + 1);
    },
  }
}
</script>

