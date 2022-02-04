<template>
    <div class="loader">
      <h1>{{ msg }}</h1>
      <img src="../assets/loading.gif"/>
    </div>
</template>

<script>
export default {
  name: 'Loader',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .loader {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        height: 100%;
        width: 100%;

        h1 {
            font-size: 76px;
            font-weight: 100;
        }

        img {
            height: 200px;
        }
    }
</style>
