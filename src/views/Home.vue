<template>
  <div class="home">
    <SearchCity msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import SearchCity from '@/components/SearchCity.vue'

export default {
  name: 'home',
  components: {
    SearchCity
  }
}
</script>
