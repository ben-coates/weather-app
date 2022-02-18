<template>
  <!-- Loader Component. Rendered until weather data has been loaded. -->
  <Loader 
    v-if="needWeather"
    msg="Weather is Loading"
    :needZip = needZip
    v-model:zip="zip"
  />
  <!-- Grid Container. Rendered once weather data has been loaded. -->
  <div class="grid-container" v-else>
    <div class="page-heading">
      <h2>Weather</h2>
      <h1 class="location">{{ locationName }}</h1>
    </div>
    <!-- Current Weather Component -->
    <Currently
      :icon="'https://openweathermap.org/img/wn/' + weather.current.weather[0].icon + '@4x.png'"
      :description="weather.current.weather[0].description"
      :temp="weather.current.temp"
      :tempUnit="'F'"
      :feelsLike="weather.current.feels_like"
      :windSpeed="Math.round(weather.current.wind_speed)"
      :humidity="Math.round(weather.current.humidity)" 
    />
    <!-- Hourly Forcast Component -->
    <Hourly :hourly="this.weather.hourly.slice(1,13)" />
    <!-- 7 day forecast -->
    <ForecastItem v-for="day in weather.daily.slice(1,8)" :key="day.id"
      :day="day.dt"
      :icon="'https://openweathermap.org/img/wn/' + day.weather[0].icon + '@2x.png'"
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

export default {
  name: 'App',
  components: {
    Loader,
    Currently,
    Hourly,
    ForecastItem
  },
  data() {
    return {
      location: Object,
      weather: null,
      zip: ""
    }
  },
  computed: {
    lat() {
      // Returns the latitude value retrieved by the location query of the Zip Code
      return this.location.results[0].locations[0].latLng.lat
    },
    lng() {
      // Returns the longitude value retrieved by the location query of the Zip Code
      return this.location.results[0].locations[0].latLng.lng
    },
    locationName() {
      // Returns the name of the most specific area given the the location query of the Zip Code
      
      // @TODO: Iterate over the values to find the most specific location value given 

      return this.location.results[0].locations[0].adminArea4;
    },
    needZip() {
      // Returns false when the Zip Code is at least five digits
      return (this.zip.length >= 5) ? false : true;
    },
    needWeather() {
      // Returns false when the weather object is no longer null
      return (this.weather) ? false : true;
    }
  },
  async mounted() {
    // Look for URL Parameters for looking up location information
    const urlParams = new URLSearchParams(window.location.search);
    if( urlParams.get("zip") ) {
      this.zip = urlParams.get("zip");
    } 
    // else {
    //   let tempPos = "Temp";
    //   navigator.geolocation.getCurrentPosition(position => {
    //     tempPos = position.coords
    //   }, err => console.log(err))
    //   this.geoPos = tempPos;
    // }
  },
  watch: {
    // Watchers
    // 
    // Being watched: zip
    // Asynchronous function watching for when the zip code length is greater than or equal to 5 digits
    // Sets this.location equal to the returned value of a location query with the new ZipCode value
    // Calls the getWeather function
    zip: async function(val) {
      if (this.zip.length >= 5) {
        this.location = await this.getLocationWithZip(val)
        this.weather = await this.getWeather(this.lat, this.lng)
      }
    }
  },
  methods: {
    getLocationWithZip(zipCode) {
      return fetch("https://open.mapquestapi.com/geocoding/v1/address?key=" + process.env.VUE_APP_MAPQUEST + "&location=" + zipCode).then(response => response.json())
    },
    getWeather(lat, lng) {
      return fetch('https://api.openweathermap.org/data/2.5/onecall?lat=' + lat + '&lon=' + lng + '&units=imperial&exclude=minutely,alerts&appid=' + process.env.VUE_APP_APIKEY).then( response => response.json())
    }
  }
}
</script>

<style lang="scss">
  @import './scss/_imports.scss';
  
  * {
    box-sizing: border-box;
  }

  html, body, #app {
    font-size: 10px;
    font-family: "Lato", sans-serif;
    background-color: darkgray;
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
  }

  .grid-container {
    display: flex;
    flex-direction: column;
    grid-gap: 1rem;
    gap: 1rem;
    padding: 1rem;

    .page-heading {
      grid-column: 1 / span 7;

      h1.location {
        font-size: 4rem;
        font-weight: 700;
        text-align: center;
        margin: 0;
      }

      h2 {
        text-align: center;
        margin: 0;
      }
    }
  }

  @include breakpoint(xxl) {
    .grid-container {
        padding: 2rem;
        height: 100%;
        width: 100%;
        display: grid;
        grid-template-rows: auto 2fr 1fr;
        grid-template-columns: repeat(7, 1fr);
        grid-gap: 2rem;
    }
  }
</style>