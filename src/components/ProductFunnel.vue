<template>
  <div id="productFunnel" class="product-funnel">
    <WelcomeScreen
      v-if="currentScreen === 0"
      @nextScreen="nextScreen"
      @changeIndustry="changeIndustry"
    />
    <JobScreen
      v-if="currentScreen === 1"
      @nextScreen="nextScreen"
      :industry="industry"
    />
    <TemplatesScreen
      v-if="currentScreen === 2"
      :industry="industry"
      @nextScreen="nextScreen"
      @getTemplatesData="getTemplatesData"
      @moveProgressBar="moveProgressBar"
    />
    <ZonesScreen
      v-if="currentScreen === 3"
      :industry="industry"
      @nextScreen="nextScreen"
      :templatesData="templatesData"
    />
    <TeamScreen
      v-if="currentScreen === 4"
      :industry="industry"
      @nextScreen="nextScreen"
      @closeScreen="closeScreen"
    />
    <QrScreen
      v-if="currentScreen === 5"
      :industry="industry"
      @nextScreen="nextScreen"
      @closeScreen="closeScreen"
    />
  </div>
</template>

<script>
import TemplatesScreen from './setup/TemplatesScreen.vue'
import WelcomeScreen from './setup/WelcomeScreen.vue'
import JobScreen from './setup/JobScreen.vue'
import ZonesScreen from './setup/ZonesScreen.vue'
import TeamScreen from './setup/TeamScreen.vue'
import QrScreen from './setup/QrScreen.vue'
export default {
  name: 'ProductFunnel',
  components: {
    ZonesScreen,
    WelcomeScreen,
    JobScreen,
    TemplatesScreen,
    TeamScreen,
    QrScreen,
  },
  data() {
    return {
      currentScreen: 0,
      industry: '',
      templatesData: [],
      progressBarPercent: 0,
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
    closeScreen() {
      this.currentScreen = 999
    },
    getTemplatesData(templatesData) {
      this.templatesData = templatesData
      console.log(this.templatesData)
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
              progress.style.width = '45%'
            }, 200)
          }
          if (this.currentScreen === 3) {
            progress.style.width = this.progressBarPercent + '%'
            setTimeout(() => {
              progress.style.transition = 'width 0.5s ease'
              progress.style.width = '90%'
            }, 200)
          }
        }
      })
    },
    moveProgressBar(isActiveScreensIncreased) {
      this.$nextTick(() => {
        let progress = document.querySelector(
          '.main-screen__progress .progress'
        )
        if (progress) {
          const currentWidth = parseInt(progress.style.width)
          if (!isActiveScreensIncreased) {
            progress.style.width = `${currentWidth - 10}%`
            this.progressBarPercent = parseInt(progress.style.width)
          } else {
            progress.style.width = `${currentWidth + 10.5}%`
            this.progressBarPercent = parseInt(progress.style.width)
          }
        }
      })
    },
  },
}
</script>
