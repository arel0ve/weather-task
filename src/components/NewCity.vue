<template>
  <div class="container">
    <form>
      <div class="form-row">
        <div class="col-lg-3 col-md-4 col-sm-6 col-12">
          <label class="active" for="city_name">City</label>
          <input v-model="currentCity" id="city_name" type="text" class="form-control"
                 :class="{ ok: resultStatus, error: !resultStatus && resultStatus !== null }">
          <small id="passwordHelpBlock" class="form-text text-muted">{{ msg }}</small>
        </div>
        <WeatherAtCity :cityWeather="selectedCity" :style="{display: displayWeather}"/>
      </div>
      <hr>
      <div class="form-row">
        <div class="col-lg-3 col-sm-4 col-6">
          <button class="btn btn-primary" type="submit" @click.prevent="showCityWeather()">
            Show weather
          </button>
        </div>
        <div class="col-lg-3 col-sm-4 col-6">
          <button class="btn btn-primary" type="submit" @click.prevent="addCityToList()">
            Add city to list
          </button>
        </div>
        <div class="col-lg-3 col-sm-4 col-6">
          <router-link to="/">
            <button class="btn btn-primary" type="submit">Back</button>
          </router-link>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import WeatherAtCity from '@/components/WeatherAtCity.vue'

export default {
  name: 'NewCity',
  components: {
    WeatherAtCity
  },
  data: function () {
    return {
      currentCity: '',
      selectedCity: {},
      msg: 'Please, input name of city',
      resultStatus: null,
      displayWeather: 'none'
    }
  },
  methods: {
    showCityWeather: function () {
      fetch('http://api.openweathermap.org/data/2.5/weather?q=' + this.currentCity +
        '&appid=42dc89867c94158b8422b57f492e627c')
        .then(response => {
          if (response.status !== 200) {
            throw new Error(response.status + '')
          }
          this.resultStatus = true
          return response.json()
        })
        .then(weather => {
          this.selectedCity = {
            name: weather.name,
            country: weather.sys.country,
            weather: weather.weather[0].main,
            temp: Math.round((weather.main.temp - 273.15) * 100) / 100,
            humidity: weather.main.humidity
          }
          this.displayWeather = 'inline-block'
        })
        .catch((err) => {
          this.__catchCity(err)
        })
    },
    addCityToList: function () {
      fetch('http://api.openweathermap.org/data/2.5/weather?q=' + this.currentCity +
        '&appid=42dc89867c94158b8422b57f492e627c')
        .then(response => {
          if (response.status !== 200) {
            throw new Error(response.status + '')
          }
          this.resultStatus = true
          let cities = JSON.parse(localStorage.getItem('cities'))
          cities.push(this.currentCity)
          localStorage.setItem('cities', JSON.stringify(cities))
          this.msg = `Selected city have added to your city list successful`
          this.displayWeather = 'none'
        })
        .catch((err) => {
          this.__catchCity(err)
        })
    },
    __catchCity: function (err) {
      this.resultStatus = false
      switch (err.message) {
        case '404':
          this.msg = `City not found, please try again later`
          break
        default:
          this.msg = `Something bad, please try again later`
          break
      }
    }
  }
}
</script>

<style scoped>
a {
  color: aliceblue;
}
a:hover {
  text-decoration: none;
}
.ok {
  color: green;
}
.error {
  color: red;
}
</style>
