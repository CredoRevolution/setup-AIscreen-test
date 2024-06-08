<template>
  <div class="main-screen main-screen_screens">
    <img src="@/assets/img/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress">
      <div class="progress"></div>
    </div>
    <h2 class="main-screen__title">Smooth Setup in 60 seconds…</h2>
    <p class="main-screen__text">Select 4 templates to see the magic!</p>
    <ul class="main-screen__list">
      <Template
        v-for="(template, index) in templates"
        :key="index"
        :text="template.name"
        :img-src="require(`@/assets/img/${industry}-last-${index + 1}.png`)"
        @getActiveData="getActiveData"
        ref="screen"
      />
    </ul>
    <p v-if="showError && !(activeScreens === 4)" class="error-message">
      Please select 4 templates
    </p>
    <a
      href="#"
      class="main-screen__btn hover-btn"
      @click="nextScreen"
      ref="nextBtn"
      :class="{
        error: showError || activeScreens > 4,
        grey: activeScreens < 4 || activeScreens > 4,
      }"
      >Finish</a
    >
  </div>
</template>

<script>
import Template from '@/components/Template.vue'

export default {
  name: 'DesignsScreen',
  components: {
    Template,
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
    checkValidation() {
      this.validation = false
      if (this.activeScreens === 4) {
        this.validation = true
        return true
      }
      return false
    },
    getTemplatesData() {
      const activeScreens = document.querySelectorAll('.screen.active')
      const activeScreensNames = []

      activeScreens.forEach((screen) => {
        const screenName = screen.querySelector('.screen__text').textContent
        activeScreensNames.push(screenName)
      })

      this.$emit('getTemplatesData', activeScreensNames)
    },
    getActiveData(el) {
      let activeScreens = 0
      for (let i = 0; i < this.$refs.screen.length; i++) {
        if (this.$refs.screen[i].isActive()) {
          activeScreens++
        }
      }
      const isActiveScreensIncreased = activeScreens > this.activeScreens
      this.activeScreens = activeScreens
      if (this.activeScreens <= 4) {
        this.$emit('moveProgressBar', isActiveScreensIncreased)
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
  },
}
</script>
