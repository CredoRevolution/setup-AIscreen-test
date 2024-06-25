<template>
  <div class="main-screen main-screen_qr">
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
          That’s it! Now you can see your first screen!
        </h2>
        <p class="main-screen-main__text">
          Now you can copy link below or scan QR-code
        </p>
        <form
          action="#"
          class="main-screen-main__form main-screen-main__form_qr"
        >
          <div class="main-screen__form-item">
            <CustomInput
              :placeholderText="'Link to copy'"
              ref="validation1"
              :defaultErrorText="'Please enter a link'"
              class="main-screen__input_qr"
              :input-name="'QR'"
              :initialValue="qrLink"
            />
          </div>
          <button class="main-screen__qr-btn blue-btn" @click.prevent="copy">
            Copy
          </button>
          <button
            class="main-screen__form-btn hover-btn blue-btn"
            @click.prevent="openQr"
          >
            Book live demo
          </button>
          <a
            href="#"
            class="main-screen__btn prev-btn"
            @click="closeQr"
            ref="prevBtn"
            >Close</a
          >
        </form>
      </div>
      <div class="main-screen__img">
        <div class="backgorund">
          <img
            :src="require(`@/assets/img/industries/Finance.png`)"
            alt="img"
            class="backgorund__img"
            ref="bigImg"
          />
        </div>
        <img
          :src="require(`@/assets/img/iPhone.png`)"
          alt="img"
          class="main-screen-main__small-img main-screen-main__small-img_qr"
          ref="smallImg"
        />
        <qrcode-vue
          :value="qrLink"
          :size="200"
          level="H"
          render-as="svg"
          class="main-screen-main__small-img main-screen-main__qr-img"
        ></qrcode-vue>
        <p class="main-screen-main__qr-img-text">
          Try to scan this QR-code with your phone now!
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import CustomInput from '@/components/form/CustomInput.vue'
import qrcode from 'qrcode.vue'
export default {
  name: 'QrScreen',
  data() {
    return {}
  },
  props: {
    qrLink: {
      type: String,
      default: 'https://aiscreen.com',
    },
  },
  components: {
    CustomInput,
    qrcodeVue: qrcode,
  },
  methods: {
    openQr() {
      this.$emit('openQr')
    },
    closeQr() {
      this.$emit('closeQr')
    },
    copy() {
      navigator.clipboard.writeText(this.qrLink)
    },
  },
  mounted() {
    console.log(this.qrLink)
    this.$refs.validation1.value = 'https://aiscreen.com'
  },
}
</script>

<style lang="scss">
@function rem($px) {
  @return ($px / 16px) + rem;
}
.main-screen_qr {
  .main-screen__img {
    overflow: hidden;
    .main-screen-main__qr-img {
      position: absolute;
      top: 53%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 35%;
      height: 40%;
      svg {
        width: 100% !important;
        height: 100% !important;
      }
    }
    .main-screen-main__small-img_qr {
      width: 52%;
      height: 85%;
      top: 60%;
      left: 50%;
      position: absolute;
      transform: translate(-50%, -50%);
    }
    .main-screen-main__qr-img-text {
      position: absolute;
      top: 83%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: rem(28px);
      line-height: rem(33px);
      font-weight: 700;
      color: rgba(134, 134, 139, 1);
      text-align: center;
      max-width: 35%;
      width: 100%;
    }
  }
  .main-screen-main__form.main-screen-main__form_qr {
    position: relative;
    margin-bottom: auto;
    .main-screen__form-item {
      .main-screen__input_qr {
        cursor: not-allowed;
        .main-screen__form-input {
          cursor: not-allowed;
        }
        input {
          pointer-events: none;
        }
      }
    }
    .main-screen__qr-btn {
      position: absolute !important;
      right: rem(6px) !important;
      top: rem(7px) !important;
      font-size: rem(17px) !important;
      line-height: rem(21px) !important;
      font-weight: 500 !important;
      border-radius: rem(9px) !important;
      padding: rem(10px) rem(19px) !important;
      background: rgba(0, 113, 226, 1) !important;
      color: rgba(245, 245, 248, 1);
      &::before {
        border-radius: rem(10px);
      }

      &:hover {
        &::before {
          border-radius: rem(10px);
        }
      }
    }
  }
  .main-screen-main__paragraph {
    margin-top: rem(31px);
    margin-bottom: auto;
    font-size: rem(17px);
    line-height: rem(21px);
    font-weight: 500;
    color: rgba(134, 134, 139, 1);
    text-align: left;
    a {
      text-decoration: none;
      color: rgba(20, 18, 31, 1);
    }
  }
}

@media (max-height: 900px) {
  .main-screen_qr
    .main-screen-main__form.main-screen-main__form_qr
    .main-screen__qr-btn {
    top: rem(6px) !important;
    padding: rem(7px) rem(17px) !important;
  }
}

@media (max-height: 800px) {
  .main-screen_qr .main-screen__img .main-screen-main__qr-img-text {
    top: 87%;
  }
}

@media (max-width: 756px) {
  .main-screen_qr
    .main-screen-main__form.main-screen-main__form_qr
    .main-screen__qr-btn {
    padding: rem(10px) rem(23px) !important;
    padding-top: rem(10px) !important;
    padding-bottom: rem(12px) !important;
  }
}
</style>
