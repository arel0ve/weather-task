<template>
  <div class="container">
    <div class="row">
      <div class="input-field">
        <input v-model="currentCity" id="city_name" type="text"
               :class="{valid: isCityValid, invalid: !isCityValid && isCityValid !== null}">
        <label class="active" for="city_name">City</label>
        <span class="helper-text" data-error="Wrong city name or bad internet connection"
              data-success="City found">Input name of city</span>
      </div>
    </div>
    <div class="row">
      <a class="waves-effect waves-light btn" @click.prevent="getWeatherByCity(currentCity)">
        <i class="material-icons right">cloud</i>
        Check weather here and add city to local storage
      </a>
    </div>
    <div class="row cities">
      <WeatherAtCity v-for="city in selectedCities" :key="city.id" :cityWeather="city"/>
    </div>
    <div class="row">The weather will update when you reload page</div>
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
      isCityValid: null
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
      localStorage.setItem('cities', '[]')
      for (let city of cities) {
        this.getWeatherByCity(city.name)
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
            weather: weather.weather[0].main,
            temp: Math.round((weather.main.temp - 273.15) * 100) / 100
          })
          console.log(this.selectedCities)
          this.isCityValid = true
          localStorage.setItem('cities', '[]')
          localStorage.setItem('cities', JSON.stringify(this.selectedCities))
        })
        .catch(() => {
          this.isCityValid = false
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
a {
  color: #d6f9f3;
}
.cities {
  background: #b5a4a2;
}
</style>
