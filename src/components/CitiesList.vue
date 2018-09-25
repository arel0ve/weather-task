<template>
  <div class="container">
    <div class="row justify-content-center cities">
      <WeatherAtCity v-for="city in selectedCities" :key="city.id" :cityWeather="city"/>
    </div>
    <div class="row justify-content-center">{{ info }}</div>
  </div>
</template>

<script>
import WeatherAtCity from '@/components/WeatherAtCity.vue'

export default {
  name: 'SearchCity',
  components: {
    WeatherAtCity
  },
  data: function () {
    return {
      currentCity: '',
      selectedCities: [],
      info: 'The weather will update when you reload page'
    }
  },
  created: function () {
    if (localStorage.getItem('cities') === null) {
      localStorage.setItem('cities', '[]')
    } else {
      this.updateWeather()
    }
  },
  methods: {
    updateWeather: function () {
      this.selectedCities = []
      const cities = JSON.parse(localStorage.getItem('cities'))
      for (let city of cities) {
        this.getWeatherByCity(city)
      }
    },
    getWeatherByCity: function (city) {
      fetch('http://api.openweathermap.org/data/2.5/weather?q=' + city +
        '&appid=42dc89867c94158b8422b57f492e627c')
        .then(response => {
          if (response.status !== 200) {
            throw new Error(response.status + '')
          }
          return response.json()
        })
        .then(weather => {
          this.selectedCities.push({
            id: this.selectedCities.length,
            name: weather.name,
            country: weather.sys.country,
            weather: weather.weather[0].main,
            temp: Math.round((weather.main.temp - 273.15) * 100) / 100,
            humidity: weather.main.humidity
          })
          this.info = 'The weather will update when you reload page'
        })
        .catch(() => {
          this.info = 'The weather will update when you reload page'
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
