<template>
  <div class="main-screen main-screen_screens">
    <img src="@/assets/img/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress">
      <div class="progress" ref="progress"></div>
    </div>
    <h2 class="main-screen__title">Smooth Setup in 60 seconds…</h2>
    <p class="main-screen__text">
      Select
      <span
        :class="[
          activeScreens === 4 ? 'valid' : '',
          activeScreens > 4 ? 'error' : '',
          '',
        ]"
        >4 templates</span
      >
      to see the magic!
    </p>
    <ul class="main-screen__list" v-if="currentPageWidth > 768">
      <Template
        v-for="(template, index) in templates"
        :key="index"
        :text="template.name"
        :img-src="require(`@/assets/img/${industry}-last-${index + 1}.png`)"
        @getActiveData="getActiveData"
        ref="screen"
        :background="false"
      />
    </ul>
    <swiper
      v-else
      :modules="modules"
      :loop="false"
      :slides-per-view="1.2"
      :space-between="12"
      :centered-slides="true"
      @slideChange="handleSlideChange"
      class="swiper-container main-screen__list_templates"
    >
      <SwiperSlide v-for="(template, index) in templates" :key="index">
        <Template
          :text="template.name"
          :img-src="require(`@/assets/img/${industry}-last-${index + 1}.png`)"
          @getActiveData="getActiveData"
          ref="screen"
          class="swiper-slide"
        />
      </SwiperSlide>
      <!-- <swiperNavigation
        :reachedEnd="reachedEnd"
        :reachedBeginning="reachedBeginning"
      /> -->
    </swiper>
    <p v-if="showError && !(activeScreens === 4)" class="error-message">
      Please select 4 templates
    </p>
    <div class="main-screen__btn-wrapper">
      <a
        href="#"
        class="main-screen__btn hover-btn blue-btn"
        @click="prevScreen"
        ref="prevBtn"
        >Prev</a
      >
      <a
        href="#"
        class="main-screen__btn hover-btn blue-btn"
        @click="nextScreen"
        ref="nextBtn"
        :class="{
          error: showError || activeScreens > 4,
          grey: activeScreens < 4 || activeScreens > 4,
        }"
        >Next</a
      >
    </div>
  </div>
</template>

<script>
import Template from '@/components/global/Template.vue'
import swiperNavigation from '@/components/global/swiperNavigation.vue'
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation } from 'swiper/modules'

export default {
  name: 'DesignsScreen',
  components: {
    Template,
    Swiper,
    SwiperSlide,
    swiperNavigation,
  },
  props: {
    industry: String,
  },
  data() {
    return {
      templates: [
        { name: 'Coffee 1' },
        { name: 'Coffee 2' },
        { name: 'Coffee 3' },
        { name: 'Coffee 3' },
        { name: 'Menu' },
        { name: 'Menu' },
        { name: 'Menu 2' },
        { name: 'Menu 3' },
      ],
      activeScreens: 0,
      validation: false,
      showError: false,
      currentPageWidth: 0,
      reachedEnd: false,
      reachedBeginning: true,
    }
  },
  methods: {
    nextScreen() {
      this.checkValidation()
      if (this.validation) {
        this.getTemplatesData()
        this.$emit('nextScreen')
      } else {
        this.showError = true
      }
    },
    onresize() {
      this.currentPageWidth = window.innerWidth
    },
    prevScreen() {
      this.$emit('prevScreen')
    },
    checkValidation() {
      this.validation = false
      if (this.activeScreens === 4) {
        this.validation = true
        return true
      }
      return false
    },
    getTemplatesData() {
      const activeScreens = this.$refs.screen.filter((el) => el.isActive())
      const activeScreensNames = []

      activeScreens.forEach((screen) => {
        const screenName = screen.text

        activeScreensNames.push(screenName)
      })

      console.log(activeScreensNames)

      this.$emit('getTemplatesData', activeScreensNames)
    },
    getActiveData(el) {
      let activeScreens = 0
      let additionalCheck = false
      for (let i = 0; i < this.$refs.screen.length; i++) {
        if (this.$refs.screen[i].isActive()) {
          activeScreens++
        }
      }

      const isActiveScreensIncreased = activeScreens > this.activeScreens
      if (this.activeScreens === 5) {
        additionalCheck = true
      }
      this.activeScreens = activeScreens
      if (this.activeScreens <= 4 && !additionalCheck) {
        this.$emit(
          'moveProgressBar',
          isActiveScreensIncreased,
          this.$refs.progress
        )
      }
      if (this.activeScreens > 4 || this.activeScreens < 4) {
        this.$refs.nextBtn.classList.add('grey')
      } else {
        this.$refs.nextBtn.classList.remove('grey')
      }

      if (this.activeScreens > 4) {
        this.$refs.screen.forEach((screen) => {
          if (screen.isActive()) {
            screen.$el.style.transition = '0.5s ease'
            screen.$el.classList.add('error-blink')
            setTimeout(() => {
              screen.$el.classList.remove('error-blink')
            }, 200)
            setTimeout(() => {
              screen.$el.classList.add('error-blink')
            }, 600)
            setTimeout(() => {
              screen.$el.classList.remove('error-blink')
            }, 900)
            setTimeout(() => {
              screen.$el.classList.add('error-blink')
            }, 1200)
            setTimeout(() => {
              screen.$el.classList.remove('error-blink')
            }, 1500)
            setTimeout(() => {
              screen.$el.style.transition = 'none'
            }, 2000)
          }
        })
      }
    },
    handleSlideChange(swiper) {
      this.reachedEnd = swiper.isEnd
      this.reachedBeginning = swiper.isBeginning
      console.log(this.reachedEnd)
    },
  },
  setup() {
    return {
      modules: [Navigation],
    }
  },

  mounted() {
    window.addEventListener('resize', this.onresize)
    this.$emit('progressBar', this.$refs.progress)
  },
}
</script>
