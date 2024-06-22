<template>
  <div
    :class="['screen', { active: active }, { 'zones-screen': zonesScreen }]"
    @click="toggleActive"
    ref="screen"
  >
    <div :class="background ? 'img-wrapper background' : 'img-wrapper'">
      <img
        v-if="imageLoaded"
        alt="img"
        class="screen__img"
        :src="require(`@/assets/img/${imgSrc}`)"
      />
      <img
        v-else
        alt="loading"
        class="screen__img loading"
        src="@/assets/img/loading.svg"
      />
    </div>
    <p class="screen__text">{{ text }}</p>
  </div>
</template>

<script>
import { data } from 'qrcode.vue'

export default {
  name: 'Screen',
  props: {
    text: String,
    imgSrc: String,
    background: Boolean,
    zonesScreen: {
      type: Boolean,
      default: false,
      required: false,
    },
    data: {
      type: Object,
      required: false,
      default: () => ({}),
    },
  },
  data() {
    return {
      imageLoaded: false,
      active: false,
    }
  },
  methods: {
    toggleActive() {
      if (this.isTemplatesList()) {
        this.deactivateAllScreens()
        this.activateCurrentScreen()
        return
      }
      if (this.isActive()) {
        this.deactivateCurrentScreen()
      } else {
        this.activateCurrentScreen()
      }
    },
    deactivateAllScreens() {
      this.$emit('deactivateAllScreens')
    },
    deactivateCurrentScreen() {
      this.active = false
      this.sendActiveData()
    },
    isActive() {
      return this.active
    },
    isTemplatesList() {
      return this.zonesScreen
    },
    activateCurrentScreen() {
      this.active = true
      this.sendActiveData(this.$refs.screen)
    },
    sendActiveData() {
      this.$emit('getActiveData', this.$refs.screen)
    },
  },
  mounted() {
    const img = new Image()
    img.src = this.imgSrc
    img.onload = () => {
      this.imageLoaded = true
      console.log(this.imageLoaded)
    }
    img.onerror = () => {
      this.imageLoaded = false
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
  &.zones-screen {
    margin-right: rem(20px);
    .screen__text {
      margin-top: rem(24px);
      font-size: rem(24px);
      line-height: rem(28px);
      font-weight: 700;
    }
    &::after {
      content: unset !important;
    }
  }

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
      border: rem(5px) solid #14121f;
      border-radius: rem(10px);
    }
  }

  &.active {
    background-color: #f5f5f8;
    border: 2px solid #0071e2;
    position: relative;
    .screen__text {
      color: #14121f;
    }
    &::after {
      content: url(@/assets/img/radio.svg);
      position: absolute;
      top: rem(24px);
      right: rem(24px);
      z-index: 2;
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

@media (max-width: 768px) {
  .screen {
    justify-content: flex-start;
    &.zones-screen {
      .screen__text {
        font-size: rem(17px);
        line-height: rem(21px);
        margin-top: rem(8px);
      }
    }
  }
}
</style>
