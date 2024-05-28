<template>
  <div class="main-screen main-screen_screens">
    <img src="@/assets/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress">
      <div class="progress"></div>
    </div>
    <h2 class="main-screen__title">Smooth Setup in 60 seconds…</h2>
    <p class="main-screen__text">
      Show your content on screen or in demo mode in 60 seconds. Get started
      quickly and easily.
    </p>
    <ul class="main-screen__list main-screen__list_templates">
      <Screen
        text="Main"
        :img-src="require(`@/assets/Finance-template-1.png`)"
        @getActiveData="getActiveData"
      />
      <!-- ОБЯЗАТЕЛЬНО ЗАМЕНИТЬ НА ${INDUSTRY!!!!!!!!} -->
      <Screen
        text="Content + Weather Right + News"
        :img-src="require(`@/assets/Finance-template-2.png`)"
        @getActiveData="getActiveData"
      />
      <!-- ОБЯЗАТЕЛЬНО ЗАМЕНИТЬ НА ${INDUSTRY!!!!!!!!} -->
    </ul>
    <a href="#" class="main-screen__btn hover-btn" @click="nextScreen"
      >Finish</a
    >
  </div>
</template>

<script>
import Screen from '@/components/Screen.vue'

export default {
  name: 'MainScreen',
  components: {
    Screen,
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
      if (this.validation) {
        this.$emit('nextScreen')
      } else {
        window.alert('Please fill in all required fields')
      }
    },
    getActiveData(el) {
      console.log(el)
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
  },
}
</script>
