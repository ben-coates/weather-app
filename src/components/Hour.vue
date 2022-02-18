<template>
    <p>{{ twelveHourTime(timestamp) }}</p>
    <p>{{ description }} <img :src="icon" :alt="description"></p>
    <p><Temperature :temp="temp" :unit="'F'"/></p>
    <p>{{ Math.round(humidity) }}%</p>
    <p>{{ Math.round(windSpeed) }} mph</p>
    <p>{{ Math.round(pop * 100) }}%</p>
</template>

<script>
import Temperature from './Temperature.vue'

export default {
    name: 'Hour',
    props: {
        timestamp: Number,
        description: String,
        icon: String,
        temp: Number,
        humidity: Number,
        windSpeed: Number,
        pop: Number
    },
    components: {
        Temperature
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
    p {
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0;
        border-right: 1px solid lightgrey;
    }

    img {
        display: inline;
        height: 2rem;
        vertical-align: middle;
    }
</style>
