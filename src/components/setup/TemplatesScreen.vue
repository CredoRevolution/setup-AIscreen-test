<template>
  <div class="main-screen main-screen_screens">
    <div class="logo-adaptation">
      <img
        v-if="require(`@/assets/img/logo.svg`)"
        :src="require(`@/assets/img/logo.svg`)"
        alt="aiscreen"
        class="main-screen-main__logo"
      />
    </div>
    <img src="@/assets/img/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress">
      <div class="progress" ref="progress"></div>
    </div>
    <h2 class="main-screen__title">Smooth Setup in 60 seconds…</h2>
    <p class="main-screen__text">
      Select
      <span :class="[activeScreens > 4 ? 'error' : '', '']">4 templates</span>
      to see the magic!
    </p>
    <ul class="main-screen__list" v-if="currentPageWidth > 768">
      <Template
        v-for="(template, index) in templates"
        :key="index"
        :text="template.name"
        :background="false"
        :imgSrc="
          template.preview_image
            ? require(`@/assets/img/${template.preview_image}`)
            : null
        "
        :data="template"
        @getActiveData="getActiveData"
        ref="screen"
      />
    </ul>
    <swiper
      v-else
      :modules="modules"
      :loop="false"
      :slides-per-view="1.05"
      :space-between="7"
      :centered-slides="true"
      @slideChange="handleSlideChange"
      class="swiper-container main-screen__list_templates main-screen__list_templates--mobile"
    >
      <SwiperSlide v-for="(template, index) in templates" :key="index">
        <Template
          :text="template.name"
          :imgSrc="require(`@/assets/img/${template.preview_image}`)"
          :data="template"
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
        class="main-screen__btn prev-btn"
        @click="prevScreen"
        ref="prevBtn"
        >Previous step</a
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
import templatesInfo from '@/assets/templatesInfo.json'
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation } from 'swiper/modules'
import { data } from 'qrcode.vue'

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
      templates: templatesInfo[this.industry],
      activeScreens: 0,
      validation: false,
      showError: false,
      currentPageWidth: 0,
      reachedEnd: false,
      reachedBeginning: true,
      activeScreensData: [],
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
      this.activeScreensData = []
      this.$refs.screen.forEach((screen) => {
        if (screen.isActive()) {
          this.activeScreensData.push({
            data: screen.data,
          })
        }
      })
      this.$emit('getTemplatesData', this.activeScreensData)
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
      this.$refs.screen.forEach((screen) => {
        if (screen.isActive()) {
          screen.$el.style.transition = '0.2s ease'
          screen.$el.classList.remove('error-blink')
        }
      })
      if (this.activeScreens > 4) {
        this.$refs.screen.forEach((screen) => {
          this.$nextTick(() => {
            if (screen.isActive()) {
              screen.$el.style.transition = '0.2s ease'
              screen.$el.classList.add('error-blink')
            }
          })
        })
      }
    },
    handleSlideChange(swiper) {
      this.reachedEnd = swiper.isEnd
      this.reachedBeginning = swiper.isBeginning
    },
  },
  setup() {
    return {
      modules: [Navigation],
    }
  },

  mounted() {
    this.activeScreensData = []
    this.onresize()
    this.$emit('progressBar', this.$refs.progress)
  },
}
</script>
