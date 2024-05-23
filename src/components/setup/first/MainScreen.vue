<template>
  <div class="main-screen">
    <img src="@/assets/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress"></div>
    <h2 class="main-screen__title">Smooth Setup in 60 seconds…</h2>
    <p class="main-screen__text">
      Show your content on screen or in demo mode in 60 seconds. Get started
      quickly and easily.
    </p>
    <ul class="main-screen__list">
      <Screen text="Coffee 1" :img-src="require('@/assets/coffee1.png')" />
      <Screen text="Coffee 2" :img-src="require('@/assets/coffee1.png')" />
      <Screen text="Coffee 3" :img-src="require('@/assets/coffee1.png')" />
      <Screen text="Coffee 3" :img-src="require('@/assets/coffee1.png')" />
      <Screen text="Menu" :img-src="require('@/assets/coffee1.png')" />
      <Screen text="Menu" :img-src="require('@/assets/coffee1.png')" />
      <Screen text="Menu 2" :img-src="require('@/assets/coffee1.png')" />
      <Screen text="Menu 3" :img-src="require('@/assets/coffee1.png')" />
    </ul>
    <a href="#" class="main-screen__btn hover-btn">Finish</a>
  </div>
</template>

<script>
import Screen from './Screen.vue'

export default {
  name: 'MainScreen',
  components: {
    Screen,
  },
}
</script>

<style lang="scss" scoped>
.main-screen {
  max-width: 1600px;
  background: #fff;
  border-radius: 43px;
  margin: 0 auto;
  padding: 34px 40px;
  &__progress {
    width: 95%;
    margin-top: 11px;
    margin-bottom: 17px;
    height: 4px;
    background: #f5f5f8;
    border-radius: 13px;
    position: relative;

    &::after {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 15%;
      height: 100%;
      background: #14121f;
      border-radius: 13px;
    }
  }
  &__title {
    line-height: 48px;
    font-size: 40px;
    font-weight: 700;

    color: #14121f;
    margin-bottom: 17px;
  }

  &__text {
    line-height: 21px;
    font-size: 17px;
    font-weight: 700;
    color: #86868b;
    margin-bottom: 50px;
  }

  &__list {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    row-gap: 17px;
    margin-bottom: 31px;
  }

  &__btn {
    font-weight: 700;
    font-size: 24px;
    line-height: 29px;
    letter-spacing: 0.02em;
    color: #fff;
    padding: 17px 31px;
    background: #0071e2;
    border-radius: 999px;
    text-decoration: none;
    transition: all 0.3s ease;
    box-sizing: content-box;
    display: flex;
    margin-left: auto;
    width: max-content;
  }
}
</style>
