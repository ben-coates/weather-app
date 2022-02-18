<template>
    <div class="loader">
      <div class="container">
        <svg width="245" height="150" viewBox="0 0 245 150" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path class="letter" d="M118.2 61.17C118.2 63.81 117.68 66.37 116.64 68.85C115.6 71.33 114.2 73.49 112.44 75.33C114.84 77.09 116.56 79.25 117.6 81.81C118.64 84.29 119.16 86.89 119.16 89.61V94.65C119.16 98.01 118.68 101.05 117.72 103.77C116.76 106.49 115.4 108.77 113.64 110.61C111.96 112.45 109.92 113.85 107.52 114.81C105.2 115.77 102.6 116.25 99.7204 116.25H62.5204V34.53H98.6404C101.52 34.53 104.16 35.01 106.56 35.97C108.96 36.93 111 38.29 112.68 40.05C114.44 41.81 115.8 43.93 116.76 46.41C117.72 48.89 118.2 51.61 118.2 54.57V61.17ZM76.6805 68.49H96.1204C97.1604 68.49 98.1604 68.25 99.1204 67.77C100.08 67.29 100.92 66.65 101.64 65.85C102.44 65.05 103.04 64.13 103.44 63.09C103.84 61.97 104.04 60.81 104.04 59.61V58.41C104.04 57.13 103.84 55.97 103.44 54.93C103.04 53.89 102.44 52.97 101.64 52.17C100.92 51.29 100.08 50.61 99.1204 50.13C98.1604 49.65 97.1604 49.41 96.1204 49.41H76.6805V68.49ZM105.12 91.29C105.12 90.09 104.88 88.97 104.4 87.93C104 86.81 103.44 85.85 102.72 85.05C102 84.25 101.12 83.61 100.08 83.13C99.1204 82.65 98.1204 82.41 97.0804 82.41H76.6805V101.13H97.0804C98.1204 101.13 99.1204 100.89 100.08 100.41C101.12 99.93 102 99.29 102.72 98.49C103.44 97.69 104 96.77 104.4 95.73C104.88 94.61 105.12 93.41 105.12 92.13V91.29Z" fill="black"/>
        <path class="letter" d="M160.588 102.09C162.908 102.09 164.588 101.41 165.628 100.05C166.748 98.61 167.588 96.89 168.148 94.89H183.268C182.948 97.85 182.188 100.65 180.988 103.29C179.788 105.85 178.188 108.09 176.188 110.01C174.268 111.93 171.948 113.45 169.228 114.57C166.588 115.69 163.748 116.25 160.708 116.25H150.388C147.188 116.25 144.148 115.65 141.268 114.45C138.388 113.17 135.868 111.45 133.708 109.29C131.548 107.13 129.828 104.61 128.548 101.73C127.348 98.85 126.748 95.77 126.748 92.49V58.41C126.748 55.13 127.348 52.05 128.548 49.17C129.828 46.21 131.548 43.65 133.708 41.49C135.868 39.33 138.388 37.65 141.268 36.45C144.148 35.17 147.188 34.53 150.388 34.53H160.708C163.828 34.53 166.708 35.09 169.348 36.21C171.988 37.33 174.268 38.89 176.188 40.89C178.188 42.81 179.788 45.09 180.988 47.73C182.188 50.37 182.948 53.21 183.268 56.25H168.148C167.748 54.17 166.988 52.41 165.868 50.97C164.748 49.53 162.988 48.81 160.588 48.81H150.508C147.708 48.81 145.468 49.81 143.788 51.81C142.188 53.81 141.388 56.25 141.388 59.13V91.65C141.388 94.53 142.188 97.01 143.788 99.09C145.468 101.09 147.708 102.09 150.508 102.09H160.588Z" fill="black"/>
        <path d="M0 0H29.16V23.5334H14.16V126.671H29.16V150H0V0Z" fill="black"/>
        <path d="M244.785 0H215.625V23.5334H230.625V126.671H215.625V150H244.785V0Z" fill="black"/>
        </svg>
        <h1 id="message">{{ msg }}</h1>
        <div id="loading-circle">
          <span id="circle-cover"></span>
        </div>
      </div>
      <div v-if="needZip">
          <label>Zip Code</label>
          <input
            type="String"
            placeholder="Zip Code"
            :value="zip"
            @input="$emit('update:zip', $event.target.value)"
          >
          <button type="button" @click="$parent.getWeather()">Get Weather</button>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Loader',
  props: {
    msg: String,
    zip: String,
    needZip: Boolean
  },
  emits: ['update:zip']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    @import '../scss/_imports.scss';
      .loader {
          display: flex;
          align-items: center;
          justify-content: center;
          flex-direction: column;
          height: 100%;
          width: 100%;

          h1 {
              font-size: 2.6rem;
              text-align: center;
          }

          /* The animation code */
          @keyframes loader {
            from {fill: black;}
            to {fill: darken($app-background-color, 10%)}
          }

          @keyframes mover {
            from {
              transform: rotate(0deg);
            }
            to {
              transform: rotate(360deg);
            }
          }

          svg path {
            animation-name: loader;
            animation-duration: 2s;
            animation-direction: alternate;
            animation-iteration-count: infinite;
          }

          #loading-circle {
            border-radius: 50%;
            position: relative;
            height: 50px;
            width: 50px;
            background-image: linear-gradient(black 0%, $app-background-color);
            margin: 10px auto;
            animation-name: mover;
            animation-duration: 2s;
            animation-iteration-count: infinite;
            animation-timing-function: linear;

            #circle-cover {
                border-radius: 50%;
                left: 5px;
                top: 5px;
                position: absolute;
                height: 40px;
                width: 40px;
                background-color: $app-background-color;
            }
          }
      }
</style>
