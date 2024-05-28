<template>
  <div class="screen" @click="addActive">
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
    addActive() {
      let allScreens = document.querySelectorAll('.screen.active')

      if (this.$el.classList.contains('active')) {
        this.$el.classList.remove('active')
      } else {
        if (allScreens.length === 4) {
          return
        }
        this.$el.classList.add('active')
      }

      this.progressBar()
    },
    progressBar() {
      let allScreens = document.querySelectorAll('.screen.active')
      let progress = document.querySelector('.main-screen__progress .progress')
      progress.style.width = `${(allScreens.length / 4) * 100}%`
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
