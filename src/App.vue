<template>
  <TypingGame />
</template>

<script>
import TypingGame from './components/TypingGame.vue'

export default {
  name: 'App',
  components: {
    TypingGame
  }
}
</script>

<style>
#app {
  min-height: 100vh;
  background: linear-gradient(#7f53ac, #647dee);
}
</style>
