<template>
    <section class="currently">
        <h2>Currently</h2>
        <img :src="icon" :alt="description" />
        <h4 id="summary">{{ description }}</h4>
        <Temperature :temperature="this.fTemp" />
        <p class="subtext">Feels Like: {{ feelsLikeF }} <span class="celsius">{{ feelsLikeC }}</span></p>
        <p class="subtext">Wind Speed: <span>{{ windSpeed }} mph</span></p>
        <p class="subtext">Humidity: <span>{{ humidity }}%</span></p>
    </section>
</template>

<script>
import Temperature from './Temperature.vue'

export default {
    name: 'Currently',
    props: {
        icon: String,
        description: String,
        fTemp: Number,
        cTemp: Number,
        feelsLikeF: String,
        feelsLikeC: String,
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
    .currently {
        grid-area: currently;
        background-color: rgb(256,256,256);
        border-radius: 1.2rem;
        box-shadow: 0rem 0.4rem 0.4rem rgba(0, 0, 0, 0.25);
        padding: 5rem;
        align-items: center;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        font-size: 16px;

            img {
                height: 300px;
            }

            h2 {
                font-size: 3.6rem;
                margin: 0;
            }

            .subtext {
                font-size: 2.4rem;
                color: rgba(0,0,0,0.54);
                margin: 0;
            }

            #temp {
                font-size: 5.4rem;
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
</style>
