<template>
  <div class='root'>
    <p> Showing {{ filteredTitles.length }} results for "{{ query }}" </p>
    <ul>
      <li v-for='title in filteredTitles' :key='title.Page'>
        {{ title.Name }}<br>{{ title.Page }}
      </li>
    </ul>
  </div>
</template>

<script>
import { computed,onMounted,onUpdated,onUnmounted } from 'vue'
import titles from '../post-data.json'

export default {
  props: {
    query: String
  },
  setup (props, context) {
    const filteredTitles = computed(() => {
      return titles.filter(s => s.Name.toLowerCase().includes(props.query.toLowerCase()) && s.Page.toLowerCase().includes(props.query.toLowerCase()) )
    })
    return {
      filteredTitles
    },
    
    onMounted(() => {
      console.log('mounted')
    }),

    onUpdated(() => {
      console.log('updated')
    }),
    onUnmounted(() => {
      console.log('unmounted')
    })

  }

}
</script>
<style scoped>
ul li {
  list-style: none;
  margin-bottom: 1em;
}
</style>
