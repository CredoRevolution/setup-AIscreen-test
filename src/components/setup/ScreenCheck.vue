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
            @click.prevent="nextScreen"
          >
            Continue with my own screen
          </button>
        </form>
      </div>
      <div class="main-screen__img">
        <div class="backgorund">
          <img
            :src="require(`@/assets/img/industries/Industry.png`)"
            alt="img"
            class="backgorund__img"
            ref="bigImg"
          />
        </div>
        <div class="img-wrapper">
          <img
            :src="require(`@/assets/img/industries/Industry-small.png`)"
            alt="img"
            class="main-screen-main__small-img background"
            ref="smallImg"
          />
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
      industry: '',
      validationCount: 0,
    }
  },
  components: {},
  computed: {},
  methods: {
    nextScreen() {
      this.$emit('nextScreen')
    },
  },
  mounted() {},
}
</script>

<style lang="scss" scoped></style>
