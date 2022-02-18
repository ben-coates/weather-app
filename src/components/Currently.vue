<template>
    <section class="currently">
        <h1>Currently</h1>
        <img :src="icon" :alt="description" />
        <h2 v-if="description" id="summary">{{ description }}</h2>
        <p class="info-text info-text-main"><Temperature :temp="temp" :unit="'F'" :includeUnit="true"/></p>
        <p v-if="feelsLike" class="info-text info-text-sub">Feels Like: <Temperature :temp="feelsLike" :unit="'F'"/></p>
        <p v-if="windSpeed" class="info-text info-text-sub">Wind Speed: <span>{{ windSpeed }} mph</span></p>
        <p v-if="humidity" class="info-text info-text-sub">Humidity: <span>{{ humidity }}%</span></p>
    </section>
</template>

<script>
import Temperature from './Temperature.vue'

export default {
    name: 'Currently',
    props: {
        // = = = = = = = = = = = = = = = = = = = = = = = =
        // Required Props

        // URL for icon 
        icon: {
            type: String,
            require: true
        },
        // Current temperature value
        temp: {
            type: Number,
            required: true
        },
        // String validated to either F or C for determining conversion formula
        tempUnit: {
            type: String,
            required: true,
            validator(value) { 
                return ['F', 'C'].includes(value)
            }
        },
        // = = = = = = = = = = = = = = = = = = = = = = = =
        // Optional Props

        // Description of current weather
        description: String,
        feelsLike: Number,
        windSpeed: Number,
        humidity: Number
    },
    components: {
        Temperature
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    @import '../scss/_imports.scss';

    .currently {
        background-color: $component-background-color;
        border-radius: $border-radius;
        @include box-shadow(0 0.4rem 0.4rem rgba(0, 0, 0, 0.25));
        padding: 3rem;
        align-items: center;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;

            h1 {
                font-size: 2.4rem;
                margin: 0;
            }

            img {
                display: block;
                max-width: 200px;
            }

            h2#summary {
                text-transform: capitalize;
                font-size: 2.6rem;
                margin:0;
            }

            .info-text {
                display: inline;
                margin: 0;

                &.info-text-main {
                    font-size: 2.8rem;
                    margin: 0.5rem 0;
                }

                &.info-text-sub {
                    font-size: 1.6rem;
                    color: rgba(0,0,0,0.54);
                }
            }
    }

    @include breakpoint(xxl) {
        .currently {
            grid-column: 1 / span 3;
            background-color: rgb(256,256,256);
            border-radius: 1.2rem;
            box-shadow: 0rem 0.4rem 0.4rem rgba(0, 0, 0, 0.25);
            padding: 5rem;
            align-items: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            font-size: 16px;

            img {
                max-width: 400px;
            }

            h1 {
                justify-self: start;
                font-size: 3.6rem;
                margin: 0;
            }

            .subtext {
                display: inline;
                font-size: 2.4rem;
                color: rgba(0,0,0,0.54);
                margin: 0;

                .temperature {
                    display: inline;
                    font-size: inherit;
                }
            }

            .temperature {
                font-size: 3.4rem;
                margin: 0;
            }

            #summary {
                text-transform: capitalize;
                font-size: 3.6rem;
                margin:0;
            }

            .celsius {
                font-size: 0.8em;
                font-weight: 200;
                vertical-align: top;
            }
        }
    }
</style>
