<template>
  <div class="col-lg-3 col-md-4 col-sm-6 col-12">
    <div class="card">
      <div class="card-body white-text">
        <h5 class="card-title">{{ cityWeather.name }}</h5>
        <h6 class="card-subtitle mb-2 text-muted">{{ cityWeather.country }}</h6>
        <p>Weather: {{ cityWeather.weather }}.</p>
        <p>Temperature: {{ cityWeather.temp }} °C.</p>
        <p>Humidity: {{ cityWeather.humidity }} %.</p>
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
