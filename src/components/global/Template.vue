<template>
  <div class="screen" @click="toggleActive" ref="screen">
    <div :class="background ? 'img-wrapper background' : 'img-wrapper'">
      <img v-if="imageLoaded" alt="img" class="screen__img" :src="imgSrc" />
    </div>
    <p class="screen__text">{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: 'Screen',
  props: {
    text: String,
    imgSrc: String,
    background: Boolean,
  },
  data() {
    return {
      imageLoaded: false,
    }
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
      this.sendActiveData(this.$el)
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
      this.$el.classList.add('active')
      this.sendActiveData(this.$el)
    },
    sendActiveData($el) {
      this.$emit('getActiveData', $el)
    },
  },
  mounted() {
    const img = new Image()
    img.src = this.imgSrc
    img.onload = () => {
      this.imageLoaded = true
    }
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

  .img-wrapper {
    width: 100%;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    .screen__img {
      border-radius: rem(12px);
      width: 98%;
      position: relative;
      z-index: 1;
    }
    &.background {
      &::after {
        content: '';
        position: absolute;
        top: -2%;
        left: 0;
        width: 100%;
        height: 104%;
        background-color: #000000;
        border-radius: rem(10px);
      }
    }
  }

  &.active {
    background-color: #f5f5f8;
    border: 2px solid #0071e2;
    .screen__text {
      color: #14121f;
    }
  }
  &.error-blink {
    border: 2px solid red;
    transition: border 0.5s ease;
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
@media (max-height: 780px) {
  .screen__text {
    font-size: rem(14px);
    margin-top: rem(3px);
  }
}
</style>
