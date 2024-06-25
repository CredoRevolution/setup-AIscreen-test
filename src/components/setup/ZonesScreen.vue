<template>
  <div class="main-screen main-screen_screens main-screen_zones">
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
    <h2 class="main-screen__title">
      Would you like to spit the screen into zones?
    </h2>
    <p class="main-screen__text">
      Show your content on screen or in demo mode in 60 seconds. Get started
      quickly and easily.
    </p>
    <swiper
      v-if="templatesData.length"
      :modules="modules"
      :loop="false"
      :slides-per-view="1.1"
      :space-between="10"
      :centeredSlides="true"
      :breakpoints="{
        768: {
          slidesPerView: 2,
          centeredSlides: false,
          spaceBetween: 17,
          loop: false,
        },
      }"
      @slideChange="handleSlideChange"
      class="swiper-container main-screen__list_templates"
    >
      <SwiperSlide v-for="(layout, index) in layoutsData" :key="layout.id">
        <Template
          :background="true"
          :zonesScreen="true"
          :text="layout.name"
          :img-src="
            require(`@/assets/img/industries/templates/Finance-template-${
              index + 1
            }.png`)
          "
          @getActiveData="getActiveData"
          ref="template"
          class="swiper-slide"
          @deactivateAllScreens="deactivateAllScreens"
        />
      </SwiperSlide>
      <swiperNavigation
        v-if="currentPageWidth > 768"
        :reachedEnd="reachedEnd"
        :reachedBeginning="reachedBeginning"
      />
      <!-- ОБЯЗАТЕЛЬНО ЗАМЕНИТЬ НА ${INDUSTRY!!!!!!!!} -->
    </swiper>

    <p v-if="showError && activeScreens < 1" class="error-message">
      Please select 1 zone
    </p>
    <div class="main-screen__btn-wrapper">
      <a
        href="#"
        class="main-screen__btn prev-btn"
        @click="prevScreen"
        ref="prevtBtn"
        >Previous step</a
      ><a
        href="#"
        class="main-screen__btn hover-btn blue-btn"
        @click="nextScreen"
        ref="nextBtn"
        :class="{
          error: showError || activeScreens > 4,
          grey: activeScreens < 1,
        }"
        >Finish</a
      >
    </div>
  </div>
</template>

<script>
import Template from '@/components/global/Template.vue'
import layoutsDataOriginal from '@/assets/layouts-preconfig.json'
import swiperNavigation from '@/components/global/swiperNavigation.vue'
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation } from 'swiper/modules'

// Import Swiper styles
import 'swiper/scss'
import 'swiper/scss/navigation'

export default {
  name: 'ZonesScreen',
  components: {
    Template,
    Swiper,
    SwiperSlide,
    swiperNavigation,
  },
  props: {
    industry: String,
    templatesData: Array,
    // templatesWindowHeight: { type: Number, required: false },
  },
  data() {
    return {
      layoutsData: JSON.parse(JSON.stringify(layoutsDataOriginal)),
      activeScreens: 0,
      validation: false,
      showError: false,
      reachedEnd: false,
      reachedBeginning: true,
    }
  },

  methods: {
    pushTemplatesData(i) {
      this.layoutsData[i].zones[0].content.push(this.templatesData)
    },
    nextScreen() {
      if (this.checkValidation()) {
        this.$emit('nextScreen')
      }
    },
    prevScreen() {
      this.$emit('prevScreen')
    },
    getActiveData(el) {
      this.showError = false
      let activeScreens = 0
      for (let i = 0; i < this.$refs.template.length; i++) {
        if (this.$refs.template[i].isActive()) {
          activeScreens++
          this.$emit('moveProgressBarOnce', this.$refs.progress)
        }
      }
      if (activeScreens < 1) {
        this.$refs.nextBtn.classList.add('grey')
        this.showError = true
      } else {
        this.$refs.nextBtn.classList.remove('grey')
      }
    },
    deactivateAllScreens() {
      for (let i = 0; i < this.$refs.template.length; i++) {
        this.$refs.template[i].deactivateCurrentScreen()
      }
    },
    checkValidation() {
      let activeScreens = 0
      for (let i = 0; i < this.$refs.template.length; i++) {
        if (this.$refs.template[i].isActive()) {
          activeScreens++
          this.pushTemplatesData(i)
          break
        } else {
          activeScreens = 0
        }
      }
      if (activeScreens >= 1) {
        return true
      }
      this.showError = true
      return false
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
  computed: {
    currentPageWidth() {
      return window.innerWidth
    },
  },
  mounted() {
    this.$emit('progressBar', this.$refs.progress)
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}
</style>
