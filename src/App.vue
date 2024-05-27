<template>
  <div id="app">
    <MainScreen2
      v-if="currentScreen === 0"
      @nextScreen="nextScreen"
      @changeIndustry="changeIndustry"
    />
    <MainScreen3
      v-if="currentScreen === 1"
      @nextScreen="nextScreen"
      :industry="industry"
    />
    <MainScreen1 v-if="currentScreen === 2" :industry="industry" />
  </div>
</template>

<script>
import MainScreen1 from './components/setup/first/MainScreen.vue'
import MainScreen2 from './components/setup/second/MainScreen.vue'
import MainScreen3 from './components/setup/third/MainScreen.vue'
export default {
  name: 'App',
  components: {
    MainScreen1,
    MainScreen2,
    MainScreen3,
  },
  data() {
    return {
      currentScreen: 0,
      screens: [MainScreen1, MainScreen2],
      industry: '',
    }
  },
  methods: {
    nextScreen() {
      this.currentScreen++
      console.log(this.currentScreen)
    },
    prevScreen() {
      this.currentScreen--
      console.log(this.currentScreen)
    },
    changeIndustry(industry) {
      this.industry = industry
      console.log(this.industry)
    },
  },
}
</script>

<style lang="scss">
@import './scss/main.scss';
</style>
