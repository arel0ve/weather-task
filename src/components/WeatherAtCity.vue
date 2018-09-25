<template>
  <div class="col s12 m6">
    <div class="card blue-grey darken-1">
      <div class="card-content white-text">
        <span class="card-title">{{ cityWeather.name }}</span>
        <p>Weather: {{ cityWeather.weather }}.</p>
        <p>Temperature: {{ cityWeather.temp }} °C.</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WeatherAtCity',
  props: ['cityWeather']
}
</script>

<style scoped>

</style>
