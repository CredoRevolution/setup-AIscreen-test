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
    <MainScreen1
      v-if="currentScreen === 2"
      :industry="industry"
      @nextScreen="nextScreen"
    />
    <TemplatesScreen
      v-if="currentScreen === 3"
      :industry="industry"
      @nextScreen="nextScreen"
    />
    <TeamScreen
      v-if="currentScreen === 4"
      :industry="industry"
      @nextScreen="nextScreen"
    />
  </div>
</template>

<script>
import MainScreen1 from './components/setup/first/MainScreen.vue'
import MainScreen2 from './components/setup/second/MainScreen.vue'
import MainScreen3 from './components/setup/third/MainScreen.vue'
import TemplatesScreen from './components/setup/templatesScreen/MainScreen.vue'
import TeamScreen from './components/setup/team/MainScreen.vue'
export default {
  name: 'App',
  components: {
    MainScreen1,
    MainScreen2,
    MainScreen3,
    TemplatesScreen,
    TeamScreen,
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

      this.progressBar()
    },
    prevScreen() {
      this.currentScreen--
      console.log(this.currentScreen)
    },
    changeIndustry(industry) {
      this.industry = industry
      console.log(this.industry)
    },
    progressBar() {
      this.$nextTick(() => {
        let progress = document.querySelector(
          '.main-screen__progress .progress'
        )
        if (progress) {
          if (this.currentScreen === 2) {
            progress.style.width = '0'
            setTimeout(() => {
              progress.style.transition = 'width 0.5s ease'
              progress.style.width = '15%'
            }, 200)
          }
          if (this.currentScreen === 3) {
            progress.style.width = '15%'
            setTimeout(() => {
              progress.style.transition = 'width 0.5s ease'
              progress.style.width = '45%'
            }, 200)
          }
        }
      })
    },
  },
}
</script>

<style lang="scss">
@import './scss/main.scss';
</style>
