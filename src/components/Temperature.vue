<template>
    <div :style="style" class="temperature">
        <p class="fahrenheit">{{ fTempFormat(temperature) }}</p>
        <p class="celsius">{{ cTempFormat(temperature) }}</p>
    </div>
</template>

<script>
    export default {
        name: 'Temperature',
        props: {
            temperature: Number,
            fontSizeBase: Number
        },
        computed: {
            style() {
                return 'font-size: ' + this.fontSizeBase + 'px';
            }
        },
        methods: {
            // Parameters
            // temp: Number - Fahrenheit temperature measurement
            // 
            // Returns: String
            // Rounded Fahrenheit temperature measurement formated with a degree symbol on the end.
            fTempFormat(temp) {
                return Math.round(temp) + "°";
            },
            // Parameters
            // temp: Number - Fahrenheit temperature measurement
            // 
            // Returns: String
            // Rounded Celsius temperature measurement formated with a degree symbol on the end.
            cTempFormat(temp) {
                return Math.round((temp - 32) * (5/9)) + "°";
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .temperature {
        font-size: 4.8rem;

        p {
            display: inline;
            margin: 0 0.5rem;
        }

        .celsius {
            font-size: 0.8em;
            font-weight: 200;
            vertical-align: top;
        }
    }
</style>
