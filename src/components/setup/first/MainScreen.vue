<template>
  <div class="main-screen">
    <img src="@/assets/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <div class="main-screen__progress">
      <div class="progress"></div>
    </div>
    <h2 class="main-screen__title">Smooth Setup in 60 seconds…</h2>
    <p class="main-screen__text">
      Show your content on screen or in demo mode in 60 seconds. Get started
      quickly and easily.
    </p>
    <ul class="main-screen__list">
      <Screen
        text="Coffee 1"
        :img-src="require(`@/assets/${industry}-last-1.png`)"
      />
      <Screen
        text="Coffee 2"
        :img-src="require(`@/assets/${industry}-last-2.png`)"
      />
      <Screen
        text="Coffee 3"
        :img-src="require(`@/assets/${industry}-last-3.png`)"
      />
      <Screen
        text="Coffee 3"
        :img-src="require(`@/assets/${industry}-last-4.png`)"
      />
      <Screen
        text="Menu"
        :img-src="require(`@/assets/${industry}-last-5.png`)"
      />
      <Screen
        text="Menu"
        :img-src="require(`@/assets/${industry}-last-6.png`)"
      />
      <Screen
        text="Menu 2"
        :img-src="require(`@/assets/${industry}-last-7.png`)"
      />
      <Screen
        text="Menu 3"
        :img-src="require(`@/assets/${industry}-last-8.png`)"
      />
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
  props: {
    industry: String,
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}
.main-screen {
  max-height: 95%;
  height: 95%;
  max-width: 90%;
  background: #fff;
  border-radius: rem(43px);
  margin: 0 auto;
  padding: rem(34px) rem(40px);
  &__progress {
    width: 95%;
    margin-top: rem(11px);
    margin-bottom: rem(17px);
    height: rem(4px);
    background: #f5f5f8;
    border-radius: rem(13px);
    position: relative;

    .progress {
      transition: all 0.3s ease;
      position: absolute;
      top: 0;
      left: 0;
      width: 1%;
      height: 100%;
      background: #14121f;
      border-radius: rem(13px);
    }
  }
  &__title {
    line-height: rem(48px);
    font-size: rem(40px);
    font-weight: 700;

    color: #14121f;
    margin-bottom: rem(17px);
  }

  &__text {
    line-height: rem(21px);
    font-size: rem(17px);
    font-weight: 700;
    color: #86868b;
    margin-bottom: rem(50px);
  }

  &__list {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    row-gap: rem(17px);
    margin-bottom: rem(31px);
  }

  &__btn {
    font-weight: 700;
    font-size: rem(24px);
    line-height: rem(29px);
    letter-spacing: 0.02em;
    color: #fff;
    padding: rem(17px) rem(31px);
    background: #0071e2;
    border-radius: rem(999px);
    text-decoration: none;
    transition: all 0.3s ease;
    box-sizing: content-box;
    display: flex;
    margin-left: auto;
    width: max-content;
  }
}
</style>
