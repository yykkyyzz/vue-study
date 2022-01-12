<template>
   <div>
     <h2> Filter LearnVue Articles </h2>
     <input 
      type='text' 
      placeholder='Filter Search' 
      v-model='query'
    />
    <button @click='reset'> Reset </button>
    <search-results :query='query' />
   </div>
</template>

<script>
import { ref } from 'vue'
import SearchResults from './SearchResults.vue'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {
    SearchResults
  },
  setup () {
    const query = ref('')
    const reset = (evt) => {
        query.value = '' // clears the query
      }
      return {
        reset,
        query
      }
  }
}
</script>
