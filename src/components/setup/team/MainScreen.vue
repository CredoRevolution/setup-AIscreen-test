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

    <a href="#" class="main-screen__btn hover-btn" @click="nextScreen"
      >Finish</a
    >
  </div>
</template>

<script>
export default {
  name: 'TeamScreen',
  methods: {
    nextScreen() {
      this.$emit('nextScreen')
    },
  },
}
</script>
