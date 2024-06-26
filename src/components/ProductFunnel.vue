<template>
  <div id="productFunnel" class="product-funnel">
    <WelcomeScreen
      v-if="currentScreen === 0"
      @nextScreen="nextScreen"
      @changeIndustry="changeIndustry"
    />
    <JobScreen
      v-show="currentScreen === 1"
      @nextScreen="nextScreen"
      :industry="industry"
      @prevScreen="prevScreen"
    />
    <ScreenCheck
      v-if="currentScreen === 2"
      @nextScreen="nextScreen"
      @myOwnScreen="myOwnScreen"
      :industry="industry"
    />
    <TemplatesScreen
      v-if="currentScreen === 3"
      :industry="industry"
      @nextScreen="nextScreen"
      @prevScreen="prevScreen"
      @getTemplatesData="getTemplatesData"
      @moveProgressBar="moveProgressBar"
      @progressBar="progressBar"
      ref="templates"
    />
    <ZonesScreen
      v-if="currentScreen === 4"
      :industry="industry"
      :templatesData="templatesData"
      @prevScreen="prevScreen"
      @nextScreen="nextScreen"
      @progressBar="progressBar"
    />
    <QrScreen
      v-if="currentScreen === 5"
      :industry="industry"
      :qrLink="qrLink"
      @closeQr="closeQr"
      @openQr="openQr"
      @closeScreen="closeScreen"
    />
    <TeamScreen
      v-if="currentScreen === 6"
      :industry="industry"
      @prevScreen="prevScreen"
      @nextScreen="nextScreen"
      @closeScreen="closeScreen"
    />
  </div>
</template>

<script>
import ScreenCheck from './setup/ScreenCheck.vue'
import TemplatesScreen from './setup/TemplatesScreen.vue'
import WelcomeScreen from './setup/WelcomeScreen.vue'
import JobScreen from './setup/JobScreen.vue'
import ZonesScreen from './setup/ZonesScreen.vue'
import TeamScreen from './setup/TeamScreen.vue'
import QrScreen from './setup/QrScreen.vue'
export default {
  name: 'ProductFunnel',
  components: {
    ZonesScreen,
    WelcomeScreen,
    JobScreen,
    TemplatesScreen,
    TeamScreen,
    QrScreen,
    ScreenCheck,
  },
  data() {
    return {
      currentScreen: 0,
      industry: '',
      templatesData: [],
      progressBarPercent: 0,
      templatesWindowHeight: 0,
      gatheredData: {},
    }
  },
  props: {
    qrLink: {
      type: String,
      required: false,
    },
  },
  methods: {
    nextScreen(data) {
      this.currentScreen++
      this.gatheredData = { ...this.gatheredData, ...data }
      console.log(this.gatheredData)
      this.progressBar()
    },
    prevScreen() {
      this.currentScreen--
    },
    closeScreen() {
      this.currentScreen = 999
    },
    myOwnScreen() {
      console.log('myOwnScreen')
      this.currentScreen++
    },
    getTemplatesData(templatesData) {
      this.templatesData = templatesData
    },
    changeIndustry(industry) {
      this.industry = industry
    },
    closeQr() {
      console.log('closeQr')
    },
    openQr() {
      console.log('openQr')
      this.currentScreen++
    },
    progressBar(progress) {
      this.$nextTick(() => {
        if (progress) {
          if (this.currentScreen === 3) {
            progress.style.width = '0'
            setTimeout(() => {
              progress.style.transition = 'width 0.5s ease'
              progress.style.width = '25%'
            }, 200)
          }
          if (this.currentScreen === 4) {
            progress.style.width = this.progressBarPercent + '%'
            setTimeout(() => {
              progress.style.transition = 'width 0.5s ease'
              progress.style.width = '92%'
            }, 200)
          }
        }
      })
    },
    moveProgressBar(isActiveScreensIncreased, progress) {
      this.$nextTick(() => {
        if (progress) {
          const currentWidth = parseInt(progress.style.width)
          if (!isActiveScreensIncreased) {
            progress.style.width = `${currentWidth - 15}%`
            this.progressBarPercent = parseInt(progress.style.width)
          } else {
            progress.style.width = `${currentWidth + 15.5}%`
            this.progressBarPercent = parseInt(progress.style.width)
          }
        }
      })
    },
    moveProgressBarOnce(progress) {
      this.$nextTick(() => {
        if (progress) {
          progress.style.width = '95%'
        }
      })
    },
  },
}
</script>

<style lang="scss">
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

@function rem($value) {
  @return $value / 16px + rem;
}

#productFunnel {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  button {
    cursor: pointer;
    border: none;
    background: none;
    padding: 0;
    font-family: Satoshi-variable, sans-serif;
    &:disabled {
      cursor: not-allowed;
    }
  }

  .loading {
    aspect-ratio: 16 / 9;
    border-color: transparent;
  }

  .blue-btn {
    &:hover {
      background: #3598fb !important;
    }
    &::before {
      display: none;
    }
  }
  .hover-btn {
    font-weight: 700;
    font-size: rem(24px);
    line-height: rem(29px);
    letter-spacing: 0.02em;
    color: #fff;
    padding: rem(17px) rem(31px);
    background: #0071e2;
    border-radius: rem(30px);
    text-decoration: none;
    transition: all 0.3s ease;
    box-sizing: content-box;
    margin-left: auto;
    width: max-content;
    cursor: pointer;
    display: inline-block;
    text-align: center;
    transition: all 0.5s ease;
    cursor: pointer;
    position: relative;
    z-index: 9;
    &::before {
      content: '';
      width: 97%;
      height: 97%;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      border-radius: inherit;
      z-index: -1;
      transition: all 0.3s;
      box-sizing: content-box;
      border: 2px solid #0071e2;
    }
    &.grey {
      background: #f5f5f8 !important;
      color: #14121f;
      &::before {
        border: 2px solid #f5f5f8;
      }
    }
  }

  .hover-btn:hover::before {
    transition: all 0.3s ease;
    width: 100% !important;
    height: 100% !important;
    padding: rem(4px);
    border-radius: rem(999px);
  }

  @media (max-width: 768px) {
    .hover-btn {
      font-size: rem(19px);
      line-height: rem(23px);
      padding-top: rem(14px);
      padding-bottom: rem(14px);
    }
  }

  .product-funnel {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .error-message {
    color: #d42b2b;
    font-size: rem(13px);
    line-height: rem(16px);
    margin-top: rem(5px);
    position: relative;
    margin-left: rem(18px);
    display: flex;
    align-items: center;
    flex-direction: row;
    &::before {
      position: absolute;
      content: url('@/assets/img/warning.svg');
      font-size: rem(15px);
      line-height: rem(18px);
      font-weight: 500;
      position: absolute;
      width: 100%;
      height: 100%;
      left: rem(-18px);
      color: #d42b2b;
      bottom: -10%;
      display: flex;
      align-items: center;
    }
  }

  .swiper {
    overflow: unset;
    width: 100%;
    margin: 0 auto;
    .swiper-slide {
      opacity: 0;
      transition: opacity 0.3s ease;
      &.swiper-slide-active {
        opacity: 1 !important;
        transition: opacity 0.3s ease;
        .swiper-slide {
          opacity: 1 !important;
          transition: opacity 0.3s ease;
        }
      }
      &.swiper-slide-next {
        opacity: 1 !important;
        transition: opacity 0.3s ease;
        .swiper-slide {
          opacity: 1 !important;
          transition: opacity 0.3s ease;
        }
      }
    }
    &.main-screen__list_templates--mobile {
      .swiper-slide {
        opacity: 1 !important;
        transition: opacity 0.3s ease;
        .swiper-slide {
          opacity: 1 !important;
          transition: opacity 0.3s ease;
        }
      }
    }
  }

  .swiper-slide {
    .screen {
      width: 100%;
    }
  }

  .logo-adaptation {
    display: none;
  }

  .main-screen {
    max-width: 95%;
    width: rem(1600px);
    height: 83.4vh;
    background: #fff;
    border-radius: rem(30px);
    margin: 0 auto;
    padding: rem(13px) rem(13px) rem(14px) rem(80px);
    position: relative;
    overflow-x: hidden;
    &__loading {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 10;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    &__loading-line {
      width: 10px;
      height: 20px;
      background: rgba(0, 0, 0, 0.2);
      margin: 0 rem(10px);
    }
    &-wrapper {
      display: flex;
      flex-direction: row;
      gap: rem(90px);
      height: 100%;
    }
    &__main {
      display: flex;
      max-width: rem(446px);
      flex-direction: column;
      width: 70%;
      .main-screen-main__logo {
        margin-bottom: auto;
        max-width: rem(128px);
        padding-top: rem(27px);
      }
      .main-screen-main__title {
        line-height: rem(48px);
        font-size: rem(40px);
        margin-bottom: rem(17px);
      }
      .main-screen-main__text {
        line-height: rem(21px);
        font-size: rem(17px);
        margin-bottom: rem(55px);
        font-weight: 700;
        color: #86868b;
      }
      .main-screen-main__form {
        display: flex;
        flex-direction: column;
        gap: rem(15px);
        // padding-bottom: 60vw;
        margin-bottom: auto;
        .main-screen__form-text {
          font-family: Satoshi-variable, sans-serif;
          margin-top: rem(14px);
          margin-bottom: -5px;
          font-weight: 700;
          font-size: rem(17px);
          line-height: rem(21px);
          color: #14121f;
        }

        .warning {
          border: 1px solid #d42b2b !important;
          position: relative;
          margin-bottom: 10px;
          &::before {
            content: url('@/assets/img/warning.svg')
              '    Please, correct the data';
            font-size: rem(15px);
            line-height: rem(18px);
            font-weight: 500;
            position: absolute;
            width: 100%;
            height: 100%;
            bottom: -110%;
            left: 0;
            color: #d42b2b;
          }
          &::placeholder {
            color: #d42b2b !important;
          }
        }

        &.state-active {
          padding-bottom: rem(98px);
        }
        .main-screen__form-btn {
          margin-top: rem(14px);
          width: 100%;
          text-align: center;
          display: unset;
          padding: rem(17px) 0 !important;
          &_additional {
            font-weight: 700;
            font-size: rem(24px);
            line-height: rem(29px);
            letter-spacing: 0.02em;
            margin-top: rem(7px);
            color: rgba(20, 18, 31, 1);
            border: 2px solid rgba(20, 18, 31, 1) !important;
            background-color: #fff !important;
            border-radius: rem(999px);
            transition: all 0.3s ease;
            &:hover {
              background-color: rgba(20, 18, 31, 1) !important;
              color: #fff;
              transition: all 0.3s ease;
            }
          }
          &_mt {
            margin-top: 0;
          }
        }
        .prev-btn {
          color: rgba(134, 134, 139, 1);
          font-size: rem(18px);
          line-height: rem(22px);
          font-weight: 500;
          transition: opacity 0.3s ease;
          background: none;
          text-decoration: none;
          margin: 0 auto;
          &:hover {
            opacity: 0.8;
            transition: opacity 0.3s ease;
          }
        }
        .main-screen__form-item {
          position: relative;
          border: 1px solid transparent;
          &.warning {
            .main-screen__form-input {
              color: #d42b2b;
              &::placeholder {
                color: #d42b2b;
              }
            }
          }
          border-radius: rem(13px);
        }
      }
    }
    &__img {
      width: 100%;
      position: relative;
      overflow: hidden;
      z-index: 1;
      .hide {
        opacity: 0;
        transition: all 0.5s ease;
      }
      .show {
        opacity: 1;
        transition: all 0.5s ease;
      }
      .backgorund {
        img {
          width: 100%;
          height: 100%;
        }
        width: 100%;
        height: 100%;
        border-radius: rem(17px);
      }
      .img-wrapper {
        position: relative;
        width: 75%;
        height: max-content;
        top: -50%;
        left: 50%;
        transform: translate(-50%, -50%);
        display: flex;
        justify-content: center;
        align-items: center;
        // border: 5px solid #000;
        // border-bottom: 7px solid #000;
        img {
          position: relative;
          z-index: 2;
          width: 98%;
          height: auto;
        }
        &::after {
          content: '';
          z-index: 1;
          position: absolute;
          top: -1.5%;
          width: 100%;
          height: 105%;
          background: rgba(0, 0, 0, 1);
          // border-radius: rem(10px);
        }
      }
      &.main-screen__img_changing {
        opacity: 1;
        position: absolute;
        left: -100%;
        width: 100%;
        height: 100%;
        transition: all 0.5s ease;
      }
    }
    .main-screen__exit {
      position: absolute;
      top: rem(17px);
      right: rem(17px);
      cursor: pointer;
      border-radius: 50%;
      background: rgba(245, 245, 248, 1);
      transition: all 0.3s ease;
      width: rem(60px);
      height: rem(60px);
      img {
        width: 100%;
        height: 100%;
        padding: rem(20px);
        transition: all 0.3s ease;

        &:hover {
          opacity: 0.8;
          transition: all 0.3s ease;
        }
      }
      &:hover {
        transform: scale(1.1);
        transition: all 0.3s ease;
      }
    }
  }

  .main-screen_zones.main-screen {
    overflow-y: hidden;
  }

  .main-screen_screens.main-screen {
    max-width: 90%;
    background: #fff;
    border-radius: rem(43px);
    margin: 0 auto;
    height: auto;
    padding: rem(34px) rem(40px);
    .main-screen__progress {
      width: 95%;
      margin-top: rem(11px);
      margin-bottom: rem(17px);
      height: rem(6px);
      background: #f5f5f8;
      border-radius: rem(13px);
      position: relative;

      .progress {
        transition: all 0.3s ease;
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        background: #14121f;
        border-radius: rem(13px);
      }
    }
    .main-screen__title {
      line-height: rem(48px);
      font-size: rem(40px);
      font-weight: 700;
      color: #14121f;
      margin-bottom: rem(17px);
      &.main-screen__title_team {
        margin-top: rem(38px);
      }
    }

    .main-screen__text {
      line-height: rem(21px);
      font-size: rem(17px);
      font-weight: 700;
      color: #86868b;
      margin-bottom: rem(50px);
      span {
        transition: all 0.3s ease;
        &.valid {
          color: #248d04;
          transition: all 0.3s ease;
        }
        &.error {
          color: #d42b2b;
          transition: all 0.3s ease;
        }
      }
    }

    .main-screen__list {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      flex-wrap: wrap;
      row-gap: rem(17px);
      margin-bottom: rem(31px);
      &_templates {
        margin-bottom: rem(31px);
      }
    }

    .main-screen__btn {
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
      &.grey {
        background: rgba(245, 245, 248, 1);
        color: rgba(134, 134, 139, 1);
        cursor: not-allowed;
        &::before {
          border: 2px solid rgba(245, 245, 248, 1);
        }
      }
      &.prev-btn {
        color: rgba(134, 134, 139, 1);
        font-size: rem(18px);
        line-height: rem(22px);
        font-weight: 500;
        display: flex;
        flex-direction: row;
        align-items: center;
        gap: rem(13px);
        transition: all 0.3s ease;
        background: none;
        padding: 0;
        &::before {
          position: relative;
          content: url('@/assets/img/prev-btn.svg');
          transition: all 0.3s ease;
          left: 0;
        }
        &:hover {
          transition: all 0.3s ease;
          &::before {
            transition: all 0.3s ease;
            left: rem(-4px);
          }
        }
      }
    }

    .main-screen__btn-wrapper {
      display: flex;
      flex-direction: row;
      align-items: center;
      justify-content: space-between;
      .main-screen__btn {
        margin-left: 0;
      }
    }
  }

  .main-screen.main-screen_check {
    .main-screen-main__title {
      font-size: rem(40px);
      line-height: rem(48px);
      margin-bottom: rem(17px);
    }
    .main-screen-main__text {
      font-size: rem(17px);
      line-height: rem(21px);
      font-weight: 700;
      margin-bottom: rem(55px);
    }
  }

  .multiselect {
    z-index: 10;
  }

  @media (max-height: 900px) {
    .main-screen {
      .main-screen__main {
        .main-screen-main__form {
          gap: rem(10px);
        }
        .main-screen-main__text {
          margin-bottom: rem(40px);
        }
        .main-screen-main__logo {
          padding-top: rem(15px);
        }
      }
      &.main-screen_team {
        .team-screen__list {
          max-height: rem(300px);
        }
      }
    }
    .main-screen_screens.main-screen
      .main-screen__title.main-screen__title_team {
      margin-top: rem(20px);
    }
  }

  @media (max-height: 780px) {
    .main-screen {
      .main-screen__main {
        .main-screen-main__title {
          line-height: rem(40px);
          font-size: rem(32px);
          margin-bottom: rem(12px);
        }
        .main-screen-main__text {
          margin-bottom: rem(32px);
        }
      }
      &.main-screen_team {
        .team-screen__list {
          max-height: rem(200px);
          margin-bottom: rem(20px);
        }
      }
    }

    .main-screen_screens.main-screen {
      .main-screen__text {
        margin-bottom: rem(30px);
      }
      .main-screen__title {
        line-height: rem(40px);
        font-size: rem(32px);
        margin-bottom: rem(12px);
      }
      .main-screen__list {
        row-gap: rem(10px);
        margin-bottom: rem(20px);
        &_templates {
          margin-bottom: rem(20px);
        }
      }
    }
  }

  @media (max-width: 1300px) {
    .main-screen {
      .main-screen-wrapper {
        gap: rem(45px);
        .main-screen__main {
          width: 100%;
        }
      }
    }
  }

  @media (max-width: 950px) {
    .main-screen {
      padding: rem(12px);
    }
    .error-message {
      margin-top: rem(2px);
    }
  }

  @media (max-width: 768px) {
    .logo-adaptation {
      display: block;
      padding-top: rem(12px);
      margin-bottom: rem(33px);
      margin-left: rem(24px);
    }
    .main-screen {
      padding: 0;
      max-width: 100%;
      height: 100%;
      border-radius: 0;
      &.main-screen_qr {
        .main-screen__img {
          display: none;
        }
        .logo-adaptation {
          display: block;
          padding-top: rem(12px);
          margin-bottom: auto;
          margin-left: rem(24px);
        }
        .main-screen-wrapper {
          min-height: 90vh;
          justify-content: center;
        }
      }

      .main-screen-wrapper {
        flex-direction: column-reverse;
        gap: rem(30px);
        .main-screen__main {
          .main-screen-main__title {
            font-size: rem(28px);
            line-height: rem(33px);
            margin-bottom: rem(17px);
          }
          img.main-screen-main__logo {
            display: none;
          }
          .main-screen-main__text {
            font-size: rem(17px);
            line-height: rem(21px);
            margin-bottom: rem(31px);
          }
          .main-screen-main__form {
            gap: rem(17px);
            .main-screen__form-btn {
              &_additional {
                font-size: rem(19px);
                line-height: rem(23px);
                padding-top: rem(14px);
                padding-bottom: rem(14px);
              }
            }
          }
        }
        .main-screen__main {
          padding: 0 rem(24px) rem(46px);
          width: 100%;
          max-width: 100%;
          .main-screen-main__logo {
            display: none;
          }
        }
        .main-screen__img {
          border-radius: 0;
          height: rem(250px);
          .backgorund {
            border-radius: 0;
            width: 104%;
            position: relative;
            left: -2%;
          }
          .img-wrapper {
            position: absolute;
            top: 50%;
            width: 45%;
          }
        }
      }
    }
    .main-screen_screens.main-screen {
      height: 100vh;
      padding: rem(12px) rem(24px);
      max-width: 100%;
      border-radius: 0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      .logo-adaptation {
        margin-left: 0;
        padding-top: 0;
        margin-bottom: rem(32px);
      }
      .main-screen__logo {
        display: none;
      }
      .main-screen__progress {
        margin-top: 0;
        margin-bottom: auto;
      }
      .main-screen__title {
        margin-top: 0;
        font-size: rem(26px);
        line-height: rem(31px);
        margin-bottom: rem(13px);
      }
      .main-screen__text {
        font-size: rem(17px);
        line-height: rem(21px);
        margin-bottom: rem(31px);
      }
      .main-screen__btn-wrapper {
        margin-bottom: auto;
        display: flex;
      }
      .main-screen__list {
        flex-direction: column;
        overflow-y: scroll;
        flex-wrap: nowrap;
        height: rem(500px);
        .screen {
          width: 100%;
          padding: rem(8px) rem(6px) rem(10px) rem(6px);
        }
      }
    }
    .error-message {
      margin-top: rem(2px);
    }
    .main-screen_qr .main-screen__img .main-screen-main__qr-img-text {
      font-size: rem(17px);
      line-height: rem(21px);
      top: 86%;
    }

    .swiper {
      .swiper-slide {
        opacity: 1;
        transition: opacity 0.3s ease;
      }
    }
    .swiper.swiper-main .swiper-slide .backgorund {
      border-radius: 0 !important;
    }
  }

  @media (max-width: 576px) {
    .main-screen {
      .main-screen-wrapper {
        .main-screen__img {
          .img-wrapper {
            width: 75%;
          }
        }
      }
    }
    .main-screen_screens.main-screen .main-screen__btn-wrapper {
      flex-direction: column-reverse;
      gap: rem(17px);
      width: 100%;
      align-items: center;
      .main-screen__btn {
        margin-left: 0;
        width: 85%;
        justify-content: center;
      }
    }
    .hover-btn {
      font-weight: 700;
      font-size: rem(19px) !important;
      line-height: rem(23px) !important;
      padding-top: rem(14px) !important;
      padding-bottom: rem(14px) !important;
    }
  }

  .backgorund {
    border-radius: rem(17px) !important;
    overflow: hidden !important;
    video {
      width: 100% !important;
      height: 100% !important;
      pointer-events: none !important;
      object-fit: cover;
    }
  }
}
</style>
