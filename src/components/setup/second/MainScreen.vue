<template>
  <div class="main-screen">
    <div class="main-screen-wrapper">
      <div class="main-screen__main">
        <img
          src="@/assets/logo.svg"
          alt="aiscreen"
          class="main-screen-main__logo"
        />
        <h2 class="main-screen-main__title">
          Welcome to AIScreen Mykita Shcherbina!
        </h2>
        <p class="main-screen-main__text">
          Show your content on screen or in demo mode in 60 seconds. Get started
          quickly and easily.
        </p>
        <form action="#" class="main-screen-main__form">
          <div class="main-screen__form-item">
            <input
              type="text"
              class="main-screen__form-input"
              placeholder="Input text"
            />
          </div>
          <div class="main-screen__form-item">
            <select class="main-screen__form-select">
              <option value="1">Option 1</option>
              <option value="2">Option 2</option>
            </select>
          </div>
          <div class="main-screen__form-item">
            <select class="main-screen__form-select">
              <option value="1">Option 1</option>
              <option value="2">Option 2</option>
            </select>
          </div>
        </form>
      </div>
      <div class="main-screen__img">
        <img src="@/assets/plug.png" alt="img" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MainScreen',
}
</script>

<style lang="scss" scoped>
.main-screen {
  max-width: 1600px;
  background: #fff;
  border-radius: 30px;
  margin: 0 auto;
  padding: 13px 13px 14px 80px;
  &-wrapper {
    display: flex;
    flex-direction: row;
    gap: 90px;
  }
  &__main {
    display: flex;
    max-width: 426px;
    flex-direction: column;
    .main-screen-main__logo {
      margin-bottom: 111px;
      max-width: 128px;
      padding-top: 27px;
    }
    .main-screen-main__title {
      line-height: 48px;
      font-size: 40px;
      margin-bottom: 17px;
    }
    .main-screen-main__text {
      line-height: 21px;
      font-size: 17px;
      margin-bottom: 55px;
    }
  }
  &__img {
    width: 100%;
    height: 100%;
    border-radius: 17px;
    img {
      width: 100%;
      height: 100%;
    }
  }
}
</style>
