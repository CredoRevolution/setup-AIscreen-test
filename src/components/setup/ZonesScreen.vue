<template>
  <div class="main-screen main-screen_screens">
    <img src="@/assets/img/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress">
      <div class="progress"></div>
    </div>
    <h2 class="main-screen__title">
      Would you like to spit the screen into zones?
    </h2>
    <p class="main-screen__text">
      Show your content on screen or in demo mode in 60 seconds. Get started
      quickly and easily.
    </p>
    <swiper
      :modules="modules"
      :loop="true"
      :slides-per-view="2"
      :space-between="50"
      @swiper="onSwiper"
      @slideChange="onSlideChange"
      class="swiper-container main-screen__list_templates"
    >
      <SwiperSlide v-for="(template, index) in templates" :key="index">
        <Template
          :text="template.name"
          :img-src="require(`@/assets/img/Finance-template-${index + 1}.png`)"
          @getActiveData="getActiveData"
          ref="template"
          class="swiper-slide"
          :background="true"
        />
      </SwiperSlide>

      <!-- ОБЯЗАТЕЛЬНО ЗАМЕНИТЬ НА ${INDUSTRY!!!!!!!!} -->
      <swiperNavigation />
    </swiper>
    <p v-if="showError && activeScreens < 1" class="error-message">
      Please select 1 zone
    </p>
    <a
      href="#"
      class="main-screen__btn hover-btn"
      @click="nextScreen"
      ref="nextBtn"
      :class="{
        error: showError || activeScreens > 4,
        grey: activeScreens < 1,
      }"
      >Finish</a
    >
  </div>
</template>

<script>
import Template from '@/components/Template.vue'
import layoutsData from '@/assets/layouts-preconfig.json'
import swiperNavigation from '@/components/swiperNavigation.vue'
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
  },
  data() {
    return {
      templates: [
        { name: 'Main' },
        { name: 'Content + Weather Right + News' },
        { name: 'Main' },
        { name: 'Content + Weather Right + News' },
      ],
      layoutsData,
      activeScreens: 0,
      validation: false,
      showError: false,
    }
  },

  methods: {
    pushTemplatesData(i) {
      this.layoutsData[i].zones[0].content.push(this.templatesData)
      console.log(this.layoutsData)
    },
    nextScreen() {
      if (this.checkValidation()) {
        this.$emit('nextScreen')
      }
    },
    getActiveData(el) {
      this.showError = false
      let activeScreens = 0
      for (let i = 0; i < this.$refs.template.length; i++) {
        if (this.$refs.template[i].isActive()) {
          activeScreens++
        }
      }
      if (activeScreens < 1) {
        this.$refs.nextBtn.classList.add('grey')
        this.showError = true
      } else {
        this.$refs.nextBtn.classList.remove('grey')
      }
      console.log(el)
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
  },
  setup() {
    const onSwiper = (swiper) => {
      console.log(swiper)
    }

    const onSlideChange = () => {
      console.log('slide change')
    }
    return {
      onSwiper,
      onSlideChange,
      modules: [Navigation],
    }
  },
  mounted() {
    console.log(this.layoutsData)
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}

.swiper {
  padding: 0 rem(30px);
  width: 104%;
  margin-left: -2%;
}

.swiper-slide {
  .screen {
    width: 100%;
  }
}
</style>
