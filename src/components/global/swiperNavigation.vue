<template>
  <div class="swiper-btns">
    <div
      :class="['next-slide-btn swiper-btn', { disabled: reachedEnd }]"
      @click="swiper.slideNext()"
    >
      <img src="@/assets/img/next.svg" alt="next slide" />
    </div>
    <div
      :class="['prev-slide-btn swiper-btn', { disabled: reachedBeginning }]"
      @click="swiper.slidePrev()"
    >
      <img src="@/assets/img/next.svg" alt="previous slide" />
    </div>
  </div>
</template>

<script>
import { useSwiper } from 'swiper/vue'
export default {
  name: 'swiper-navigation',
  setup() {
    const swiper = useSwiper()
    return {
      swiper,
    }
  },
  props: {
    reachedEnd: {
      type: Boolean,
      default: false,
    },
    reachedBeginning: {
      type: Boolean,
      default: true,
    },
    qrLink: {
      type: String,
      default: '',
    },
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}
.swiper-btn {
  position: absolute;
  width: rem(37px);
  height: rem(37px);
  background: rgba(210, 210, 216, 0.7);
  border-radius: 50%;
  opacity: 0.8;
  top: 45%;
  transform: translateY(-50%);
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  img {
    height: rem(13px);
    width: rem(7px);
  }
  &.next-slide-btn {
    right: rem(-38px);
  }
  &.prev-slide-btn {
    left: rem(-38px);
    img {
      transform: rotate(180deg);
    }
  }
  &.disabled {
    background: rgba(210, 210, 216, 0.2);
    &:hover {
      opacity: 0.5;
      cursor: default;
    }
  }
  &:hover {
    transition: all 0.3s ease;
    opacity: 1;
    cursor: pointer;
  }
}
</style>
