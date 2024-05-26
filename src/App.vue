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
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background: url('../src/assets/main-bg.png');
  background-size: cover;
  height: 100vh;
  font-family: Satoshi-variable, sans-serif;
  padding-top: 100px;
}

button {
  cursor: pointer;
  border: none;
  background: none;
  padding: 0;
  &:disabled {
    cursor: not-allowed;
  }
}

.hover-btn {
  font-weight: 700;
  font-size: 24px;
  line-height: 29px;
  letter-spacing: 0.02em;
  color: #fff;
  padding: 17px 31px;
  background: #0071e2;
  border-radius: 999px;
  text-decoration: none;
  transition: all 0.3s ease;
  box-sizing: content-box;
  margin-left: auto;
  width: max-content;
  cursor: pointer;
  display: inline-block;
  text-align: center;
  transition: all 0.5s ease;
  cursor: pointer;
  position: relative;
  z-index: 9;
  &::before {
    content: '';
    width: 97%;
    height: 97%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 30px;
    z-index: -1;
    transition: all 0.3s;
    box-sizing: content-box;
    border: 2px solid #0071e2;
  }
}

.hover-btn:hover::before {
  transition: all 0.3s ease;
  width: 100% !important;
  height: 100% !important;
  padding: 3px;
  border-radius: 1000px;
}
</style>
