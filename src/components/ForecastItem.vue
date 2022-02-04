<template>
    <div class="forecast-item">
        <h1 class="day">{{ this.convertToWeekday(day) }} </h1>
        <img class="icon" :src="icon" :alt="description" />
        <Temperature :temperature="this.high"/>
        <Temperature :temperature="this.low"/>
        <div class="precipitation">
            <p>Chance of Precipitation</p>
            <p>{{ Math.round(precipitation * 100)}}%</p>
        </div>
    </div>
</template>

<script>
    import Temperature from './Temperature.vue'

    export default {
        name: 'ForecastItem',
        props: {
            day: Number,            // Timestamp
            icon: String,           // URL to icon graphic
            description: String,    // String for the alt description of the icon
            high: Number,           // Number of high temperature to pass to the temperature component
            low: Number,            // Number of low temperature to pass to the temperature component
            precipitation: Number   // Precipitation change expected in decimal form
        },
        components: {
            Temperature
        },
        methods: {
            // Parameters
            // date: Number - Timestamp 
            // IMPORTANT assuming timestamp in seconds
            // 
            // Returns: String
            // Day of week abbreviation
            convertToWeekday: function(date) {
                var day = new Date(date * 1000);                            // Create Date object using timestamp, timestamp is given in seconds and converted to milliseconds
                var days = ["Sun","Mon","Tue","Wed","Thu","Fri","Sat"];     // Create array for days of the week abbreviations
                return days[day.getDay()];                                  // Returns day of the week abbreviation
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .forecast-item {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: rgb(256,256,256);
            border-radius: 1.2rem;
            padding: 4rem;
            box-shadow: 0rem 0.4rem 0.4rem rgba(0, 0, 0, 0.25);

        .day {
            font-size: 3rem;
            text-transform: uppercase;
            margin: 0;
        }

        .icon {
            border-radius: 50%;
            height: 10rem;
        }

        .precipitation {
            font-size: 2rem;
            color: rgba(0,0,0,0.54);
            margin: 0;
            text-align: center;

            p {
                margin: 0;
            }
        }
    }
</style>
