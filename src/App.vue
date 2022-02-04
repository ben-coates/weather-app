<template>
  <Loader v-if="weather === null" msg="Weather is Loading"/>
  <div class="grid-container" v-else>
    <Currently
      :icon="'http://openweathermap.org/img/wn/' + weather.current.weather[0].icon + '@2x.png'"
      :description="weather.current.weather[0].description"
      :fTemp="weather.current.temp"
      :cTemp="weather.current.temp"
      :feelsLikeF="calF(weather.current.feels_like)"
      :feelsLikeC="calC(weather.current.feels_like)"
      :windSpeed="Math.round(weather.current.wind_speed)"
      :humidity="Math.round(weather.current.humidity)" 
    />
    <Hourly :hourly="this.weather.hourly.slice(1,13)" />
    <ForecastItem v-for="day in weather.daily.slice(1,8)" :key="day.id"
      :day="day.dt"
      :icon="'http://openweathermap.org/img/wn/' + day.weather[0].icon + '@2x.png'"
      :description="day.weather[0].description"
      :high="day.temp.max"
      :low="day.temp.min"
      :precipitation="day.pop"
    />
  </div>
</template>

<script>
import Loader from './components/Loader.vue'
import Currently from './components/Currently.vue'
import Hourly from './components/Hourly.vue'
import ForecastItem from './components/ForecastItem.vue'
import axios from 'axios' 

export default {
  name: 'App',
  data() {
    return { 
      weather: null,
      degree: "°"
    }
  },
  components: {
    Loader,
    Currently,
    Hourly,
    ForecastItem
  },
  created() {
    this.getData();
  },
  methods: {
    getData: function() {
      axios
        .get(
          'https://api.openweathermap.org/data/2.5/onecall?lat=37.547779&lon=-83.491699&units=imperial&exclude=minutely,alerts&appid=' + process.env.VUE_APP_APIKEY,
          {headers: {"Content-Type": "application/json"}})
        .then(
          response => {
            this.weather = response.data;
          }
        )
        .catch(function (error) {
            // handle error
            console.log(error.toJSON());
          }
        );
    },
    calF(fTemp) {
        return Math.round(fTemp) + this.degree;
    },
    calC(fTemp) {
        return Math.round((fTemp - 32) * (5/9)) + this.degree;
    }
  },
}
</script>

<style lang="scss">
  * {
    box-sizing: border-box;
  }

  html, body, #app {
    font-size: 10px;
    font-family: "Lato", sans-serif;
    background-color: darkgray;
    margin: 0;
    height: 100%;
    width: 100%;
    padding: 0;
  }

  .grid-container {
      padding: 2rem;
      height: 100%;
      width: 100%;
      display: grid;
      grid-template-areas:  "currently currently currently hourly hourly hourly hourly"
                            "currently currently currently hourly hourly hourly hourly";
      grid-template-rows: 2fr 1fr;
      grid-template-columns: repeat(7, 1fr);
      grid-gap: 2rem;
  }
</style>