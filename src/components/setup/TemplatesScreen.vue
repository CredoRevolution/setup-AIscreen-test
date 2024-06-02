<template>
  <div class="main-screen main-screen_screens">
    <img src="@/assets/img/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress">
      <div class="progress"></div>
    </div>
    <h2 class="main-screen__title">Smooth Setup in 60 seconds…</h2>
    <p class="main-screen__text">
      Show your content on screen or in demo mode in 60 seconds. Get started
      quickly and easily.
    </p>
    <ul class="main-screen__list">
      <Template
        text="Coffee 1"
        :img-src="require(`@/assets/img/${industry}-last-1.png`)"
        @getActiveData="getActiveData"
      />
      <Template
        text="Coffee 2"
        :img-src="require(`@/assets/img/${industry}-last-2.png`)"
        @getActiveData="getActiveData"
      />
      <Template
        text="Coffee 3"
        :img-src="require(`@/assets/img/${industry}-last-3.png`)"
        @getActiveData="getActiveData"
      />
      <Template
        text="Coffee 3"
        :img-src="require(`@/assets/img/${industry}-last-4.png`)"
        @getActiveData="getActiveData"
      />
      <Template
        text="Menu"
        :img-src="require(`@/assets/img/${industry}-last-5.png`)"
        @getActiveData="getActiveData"
      />
      <Template
        text="Menu"
        :img-src="require(`@/assets/img/${industry}-last-6.png`)"
        @getActiveData="getActiveData"
      />
      <Template
        text="Menu 2"
        :img-src="require(`@/assets/img/${industry}-last-7.png`)"
        @getActiveData="getActiveData"
      />
      <Template
        text="Menu 3"
        :img-src="require(`@/assets/img/${industry}-last-8.png`)"
        @getActiveData="getActiveData"
      />
    </ul>
    <a href="#" class="main-screen__btn hover-btn" @click="nextScreen"
      >Finish</a
    >
  </div>
</template>

<script>
import Template from '@/components/Template.vue'
import layoutsData from '@/assets/layouts-preconfig.json'
const layouts = layoutsData?.map((layout) => layout) || []
console.log(layouts)
import axios from 'axios'

export default {
  name: 'DesignsScreen',
  components: {
    Template,
  },
  data() {
    return {
      layouts,
    }
  },
  props: {
    industry: String,
  },

  data() {
    return {
      validation: false,
    }
  },

  methods: {
    nextScreen() {
      this.checkValidation()
      this.getTemplatesData()
      if (this.validation) {
        this.$emit('nextScreen')
      } else {
        window.alert('Please fill in all required fields')
      }
    },
    checkValidation() {
      const allScreens = document.querySelectorAll('.screen')
      this.validation = false
      allScreens.forEach((screen) => {
        if (screen.classList.contains('active')) {
          this.validation = true
          return
        }
      })
    },
    getTemplatesData() {
      const activeScreens = document.querySelectorAll('.screen.active')
      const activeScreensNames = []

      activeScreens.forEach((screen) => {
        const screenName = screen.querySelector('.screen__text').textContent
        activeScreensNames.push(screenName)
      })

      layouts[0].zones[0].content.push(...activeScreensNames)
      layouts[1].zones[0].content.push(...activeScreensNames)

      console.log(layouts)
    },
    getActiveData(el) {
      console.log(el)
    },
  },
}
</script>
