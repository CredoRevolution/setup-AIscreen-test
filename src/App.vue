<template>
  <div id="app">
    <MainScreen2 v-if="currentScreen === 0" />
    <MainScreen1 v-if="currentScreen === 1" />
  </div>
</template>

<script>
import MainScreen1 from './components/setup/first/MainScreen.vue'
import MainScreen2 from './components/setup/second/MainScreen.vue'
export default {
  name: 'App',
  components: {
    MainScreen1,
    MainScreen2,
  },
  data() {
    return {
      currentScreen: 0,
      screens: [MainScreen1, MainScreen2],
    }
  },
  methods: {
    nextScreen() {
      this.currentScreen++
    },
    prevScreen() {
      this.currentScreen--
    },
  },
}
</script>

<style lang="scss">
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-color: black;
  font-family: Satoshi-variable, sans-serif;
  height: 100vh;
  padding-top: 30px;
}
</style>
