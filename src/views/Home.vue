<template>
  <div class="home">
    <CitiesList msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import CitiesList from '@/components/CitiesList.vue'

export default {
  name: 'home',
  components: {
    CitiesList
  }
}
</script>
