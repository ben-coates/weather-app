<template>
    <section id="hourly">
        <h1>Hourly</h1>
        <div id="hourly-forecast">
            <h2>Time</h2>
            <h2>Description</h2>
            <h2>Temp</h2>
            <h2>Humidity</h2>
            <h2>Wind Speed</h2>
            <h2>Precip</h2>
            <Hour v-for="hour in hourly" :key="hour.id"
                :timestamp="hour.dt"
                :description="hour.weather[0].description"
                :icon="'https://openweathermap.org/img/wn/' + hour.weather[0].icon + '@2x.png'"
                :temp="hour.temp"
                :humidity="hour.humidity"
                :windSpeed="hour.wind_speed"
                :pop="hour.pop"
            />
        </div>
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
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    @import '../scss/_imports.scss';

    #hourly {
        background-color: $component-background-color;
        border-radius: $border-radius;
        @include box-shadow(0rem 0.4rem 0.4rem rgba(0, 0, 0, 0.25));
        padding: 5rem;

        h1 {
            font-size: 2.4rem;
            margin: 0;
            grid-column: 1/7;
            text-align: center;
        }

        #hourly-forecast {
            overflow: scroll;
            display: grid;
            grid-template-columns: repeat(6, 40%);

            h2 {
                text-align: center;
                border-bottom: 1px solid lightgray;
                margin: 0;
            }

            :nth-child(12n+7), :nth-child(12n+8), :nth-child(12n+9), :nth-child(12n+10), :nth-child(12n+11), :nth-child(12n+12) {
                background-color: #e7e7e7;
            }

            :nth-child(6n+12) {
                border: none;
            }
        }
    }

    @include breakpoint(xxl) {
        #hourly {
            grid-column: 4 / span 4;
            display: flex;
            flex-direction: column;;
            background-color: rgb(256,256,256);
            border-radius: 1.2rem;
            box-shadow: 0rem 0.4rem 0.4rem rgba(0, 0, 0, 0.25);
            padding: 5rem;

            h1 {
                font-size: 3.6rem;
                margin: 1rem;
                text-align: center;
            }

            #hourly-forecast {
                display: grid;
                grid-template-columns: repeat(6, 1fr);
                grid-template-rows: repeat(auto);
                height: 100%;

                h2 {
                    text-align: center;
                    border-bottom: 1px solid lightgray;
                    margin: 0;
                }   
            }
        }
    }
</style>
