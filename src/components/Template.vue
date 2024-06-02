<template>
  <div class="screen" @click="toggleActive" ref="screen">
    <img alt="img" class="sreen__img" :src="imgSrc" />
    <p class="screen__text">{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: 'Screen',
  props: {
    text: String,
    imgSrc: String,
  },
  data() {
    return {}
  },
  methods: {
    toggleActive() {
      const allScreens = this.getAllScreens()
      const isActive = this.isActive()
      const isTemplatesList = this.isTemplatesList()

      if (isTemplatesList) {
        this.deactivateAllScreens()
        this.activateCurrentScreen()
        return
      }
      if (isActive) {
        this.deactivateCurrentScreen()
      } else {
        if (allScreens.length === 4) {
          return
        }
        this.activateCurrentScreen()
      }
    },
    deactivateAllScreens() {
      const screens = this.getAllScreens()
      screens.forEach((screen) => {
        screen.classList.remove('active')
      })
    },
    deactivateCurrentScreen() {
      this.$el.classList.remove('active')
    },
    getAllScreens() {
      return Array.from(document.querySelectorAll('.screen.active'))
    },
    isActive() {
      if (this.$el.classList.contains('active')) {
        return true
      }
      return false
    },
    isTemplatesList() {
      return this.$el.closest('.main-screen__list_templates') !== null
    },
    activateCurrentScreen() {
      this.sendActiveData(this.$el)
      this.$el.classList.add('active')
    },
    sendActiveData($el) {
      this.$emit('getActiveData', $el)
    },
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}
.screen {
  cursor: pointer;
  width: 30%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 22%;
  border-radius: rem(20px);
  padding: rem(8px) rem(8px) rem(10px) rem(8px);
  background-color: #fff;
  border: 2px solid transparent;

  img {
    border-radius: rem(12px);
    width: 100%;
  }

  &.active {
    background-color: #f5f5f8;
    border: 2px solid #0071e2;
    .screen__text {
      color: #14121f;
    }
  }
  &__text {
    font-size: rem(17px);
    line-height: rem(21px);
    text-align: center;
    margin-top: rem(8px);
    letter-spacing: 0.02em;
    font-weight: 700;
    color: #86868b;
  }
}
</style>
