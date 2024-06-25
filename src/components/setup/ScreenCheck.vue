<template>
  <div class="main-screen main-screen_check">
    <div class="logo-adaptation">
      <img
        v-if="require(`@/assets/img/logo.svg`)"
        :src="require(`@/assets/img/logo.svg`)"
        alt="aiscreen"
        class="main-screen-main__logo"
      />
    </div>
    <div class="main-screen-wrapper">
      <div class="main-screen__main">
        <img
          v-if="require(`@/assets/img/logo.svg`)"
          :src="require(`@/assets/img/logo.svg`)"
          alt="aiscreen"
          class="main-screen-main__logo"
        />
        <h2 class="main-screen-main__title">
          Welcome to your free trial Mykita Shcherbina!
        </h2>
        <p class="main-screen-main__text">
          Show your content on screen or in demo mode in 60 seconds. Get started
          quickly and easily.
        </p>
        <form action="#" :class="['main-screen-main__form']">
          <button
            class="main-screen__form-btn main-screen__form-btn_mt hover-btn blue-btn"
            @click.prevent="nextScreen"
          >
            Launch your screen-free demo
          </button>
          <button
            class="main-screen__form-btn main-screen__form-btn_additional"
            @click.prevent="myOwnScreen"
          >
            Continue with my own screen
          </button>
        </form>
      </div>
      <div class="main-screen__img">
        <div class="backgorund">
          <img
            v-if="industry !== 'Digital Menu Boards' && industry !== 'Retail'"
            :src="require(`@/assets/img/${industry}/bg.png`)"
            alt="img"
            class="backgorund__img"
          />
          <video
            v-else-if="industry === 'Digital Menu Boards'"
            autoplay
            muted
            loop
            alt="img"
            class="backgorund__img"
            playsinline
            ref="bigImg"
          >
            <source
              :src="require(`@/assets/img/${industry}/bg.mp4`)"
              type="video/mp4"
              alt="img"
            />
          </video>
          <video
            v-else-if="industry === 'Retail'"
            autoplay
            playsinline
            muted
            loop
            alt="img"
            class="backgorund__img"
            ref="bigImg"
          >
            <source
              :src="require(`@/assets/img/${industry}/bg.mp4`)"
              type="video/mp4"
              alt="img"
            />
          </video>
        </div>
        <div class="img-wrapper">
          <img :src="require(`@/assets/img/${industry}/main.png`)" alt="img" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GreetingsScreen',
  data() {
    return {
      validationCount: 0,
    }
  },
  props: {
    industry: {
      type: String,
      required: true,
    },
  },
  components: {},
  computed: {},
  methods: {
    nextScreen() {
      this.$emit('nextScreen')
    },
    myOwnScreen() {
      this.$emit('myOwnScreen')
    },
  },
  mounted() {},
}
</script>

<style lang="scss" scoped></style>
