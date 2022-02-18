//  Temperature Component
//  Does the conversion of temperatures from either Fahrenheit or Celsius. 
//  Returns a div with the class temperature containing both fahrenheit and celsius temperatures.
//  Expects the parent component to provide the bulk of the styling.

<template>  
    <span class="temperature fahrenheit">{{ fTemp }}°<span v-if="includeUnit">&nbsp;F</span></span>
    <span class="temperature celsius">{{ cTemp }}°<span v-if="includeUnit">&nbsp;C</span></span>
</template>

<script>
    export default {
        name: 'Temperature',
        props: {
            // Temperature value
            // Type: Number
            // Required prop
            temp: {
                type: Number,
                required: true
            },
            // String validated to either F or C for determining conversion formula
            // Type: String
            // Required prop
            unit: {
                type: String,
                required: true,
                validator(value) { 
                    return ['F', 'C'].includes(value)
                }
            },
            includeUnit: {
                type: Boolean,
                default: false,
            }
        },
        computed: {
            // If the unitGiven string is F returns the temp recieved from the parent. Otherwise converts the temperature from celsius and returns that value
            fTemp: function() {
                return (this.unit === "F") ? Math.round(this.temp) : Math.round((this.temp * 9/5) + 32)
            },
            // If the unitGiven string is C returns the temp recieved from the parent. Otherwise converts the temperature from fahrenheit and returns that value
            cTemp: function() {
                return (this.unit === "C") ? Math.round(this.temp) : Math.round((this.temp - 32) * (5/9))
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .temperature {
        margin: 0 0.3rem;

        &.celsius {
            font-size: 0.8em;
            font-weight: 200;
            vertical-align: top;
        }
    }
</style>
