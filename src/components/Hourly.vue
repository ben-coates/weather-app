<template>
    <section class="hourly">
        <h1>Hourly</h1>
            <h2>Time</h2>
            <h2>Description</h2>
            <h2>Temp</h2>
            <h2>Humidity</h2>
            <h2>Wind Speed</h2>
            <h2>Precip</h2>
        <Hour v-for="hour in hourly" :key="hour.id"
            :timestamp="hour.dt"
            :description="hour.weather[0].description"
            :icon="'http://openweathermap.org/img/wn/' + hour.weather[0].icon + '@2x.png'"
            :temp="hour.temp"
            :humidity="hour.humidity"
            :windSpeed="hour.wind_speed"
            :pop="hour.pop"
        />
        <!-- <div class="hour" v-for="hour in weather.hourly.slice(1,13)" :key="hour.id" >
            <p>{{ twelveHourTime(hour.dt) }}</p>
            <p>{{ hour.weather[0].description }} <img :src="'http://openweathermap.org/img/wn/' + hour.weather[0].icon + '@2x.png'"/></p>
            <p>{{ calF(hour.temp) }} <span class="celsius">{{ calC(hour.temp) }}</span></p>
            <p>{{ Math.round(hour.humidity) }}%</p>
            <p>{{ Math.round(hour.wind_speed) }} mph</p>
            <p>{{ Math.round(hour.pop * 100) }}%</p>
        </div> -->
    </section>
</template>

<script>
import Hour from './Hour.vue';

export default {
    name: 'Hourly',
    data() {
        return { 
            degree: "°"
        }
    },
    props: {
        hourly: Object
    },
    components: {
        Hour
    },
    methods: {
        twelveHourTime(timestamp) {
            var date = new Date(timestamp * 1000);
            var hours = date.getHours();
            var ampm = hours >= 12 ? 'PM' : 'AM';
            hours = hours % 12;
            hours = hours ? hours : 12; // the hour '0' should be '12'
            return hours + ":00 " + ampm;
        },
        calF(fTemp) {
            return Math.round(fTemp) + this.degree;
        },
        calC(fTemp) {
            return Math.round((fTemp - 32) * (5/9)) + this.degree;
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .hourly {
        grid-area: hourly;
        background-color: rgb(256,256,256);
        border-radius: 1.2rem;
        box-shadow: 0rem 0.4rem 0.4rem rgba(0, 0, 0, 0.25);
        padding: 5rem;
        display: grid;
        grid-template-columns: repeat(6, 1fr);

        h1 {
            font-size: 3.6rem;
            margin: 0;
            grid-column: 1/7;
            text-align: center;
        }

        h2 {
            text-align: center;
            border-bottom: 1px solid lightgray;
            margin: 0;
        }
    }
</style>
