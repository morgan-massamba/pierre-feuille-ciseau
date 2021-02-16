<template>
  <Game/>
</template>

<script>
import Game from './components/Game.vue'

export default {
  name: 'App',
  components: {
    Game
  }
}
</script>

<style lang="scss">
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body{
  min-height: 100vh;
  background: radial-gradient(circle at 50% 20% ,hsl(214, 47%, 23%), hsl(237, 49%, 15%));
  width: 100%;
  color: white;
}

</style>
