<template>
  <div class="main-screen">
    <div class="main-screen-wrapper">
      <div class="main-screen__main">
        <img
          src="@/assets/logo.svg"
          alt="aiscreen"
          class="main-screen-main__logo"
        />
        <h2 class="main-screen-main__title">And lastly...</h2>
        <p class="main-screen-main__text">
          Show your content on screen or in demo mode in 60 seconds. Get started
          quickly and easily.
        </p>
        <form action="#" class="main-screen-main__form">
          <div class="main-screen__form-item">
            <input
              type="text"
              class="main-screen__form-input"
              placeholder="Job Title"
              required
            />
          </div>
          <div class="main-screen__form-item">
            <select
              class="main-screen__form-select main-screen__form-select_industry"
              required
            >
              <option value="plug" disabled selected>Job function</option>
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
            </select>
          </div>
          <div class="main-screen__form-item">
            <input
              type="text"
              class="main-screen__form-input"
              placeholder="Phone number"
              required
            />
          </div>
          <p class="main-screen__form-text">
            How many screens do you intend to connect?
          </p>
          <div class="main-screen__form-switch">
            <button
              class="main-screen__form-switch-btn active"
              type="button"
              @click.prevent="makeActive"
            >
              1-10
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              11-50
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              51-199
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              200-499
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              500+
            </button>
          </div>
          <button
            class="main-screen__form-btn hover-btn"
            @click.prevent="nextScreen"
          >
            Next step
          </button>
        </form>
      </div>
      <div class="main-screen__img">
        <div class="backgorund">
          <img
            :src="require(`@/assets/${industry}.png`)"
            alt="img"
            class="backgorund__img"
          />
        </div>
        <img :src="require(`@/assets/${industry}-small.png`)" alt="img" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MainScreen',
  methods: {
    nextScreen() {
      const form = this.$el.querySelector('form')
      const requiredFields = form.querySelectorAll('[required]')
      let isValid = true

      requiredFields.forEach((field) => {
        console.log(field.value)
        if (field.value == 'plug' || !field.value) {
          isValid = false
          field.classList.add('warning') // Add a CSS class for styling
        } else {
          field.classList.remove('warning') // Remove the CSS class if field is filled
        }
      })

      if (isValid) {
        this.$emit('nextScreen')
        console.log('nextScreen')
      } else {
        console.log('Please fill in all required fields')
      }
    },
    makeActive(e) {
      const buttons = this.$el.querySelectorAll('.main-screen__form-switch-btn')
      buttons.forEach((button) => {
        button.classList.remove('active')
      })
      e.target.classList.add('active')
    },
  },
  data() {
    return {}
  },
  props: {
    industry: {
      type: String,
      default: 'plug',
    },
  },
}
</script>

<style lang="scss" scoped>
.main-screen {
  max-height: 95%;
  height: 95%;
  max-width: 90%;
  background: #fff;
  border-radius: 30px;
  margin: 0 auto;
  padding: 13px 13px 14px 80px;
  &-wrapper {
    display: flex;
    flex-direction: row;
    gap: 90px;
    height: 100%;
  }
  &__main {
    display: flex;
    max-width: 426px;
    flex-direction: column;
    width: 70%;
    .main-screen-main__logo {
      margin-bottom: 10%;
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
      font-weight: 700;
      color: #86868b;
    }
    .main-screen-main__form {
      display: flex;
      flex-direction: column;
      gap: 8px;
      .warning {
        border: 1px solid red !important;
      }
      &.state-active {
      }
      .main-screen__form-btn {
        margin-top: 14px;
        width: 100%;
        text-align: center;
        display: unset;
        padding: 17px 0;
      }
      .main-screen__form-text {
        font-family: Satoshi-variable, sans-serif;
        margin-top: 14px;
        margin-bottom: -5px;
        font-weight: 700;
        font-size: 17px;
        line-height: 21px;
        color: #14121f;
      }
      .main-screen__form-switch {
        display: flex;
        flex-direction: row;
        padding: 3px;
        border: 1px solid #86868b80;
        border-radius: 999px;

        .main-screen__form-switch-btn {
          padding: 12px 17px;
          background: #fff;
          border-radius: 999px;
          font-weight: 500;
          font-size: 17px;
          line-height: 21px;
          color: #86868b;
          border: 1px solid transparent;
          &:not(:last-child) {
            border-right: 1px solid #86868b80;
            border-radius: 0;
          }
          &.active {
            border: 1px solid #141414;
            background: #141414;
            border-radius: 999px;
            color: #fff;
          }
        }
      }
      .main-screen__form-item {
        .main-screen__form-input {
          background: #fff;
          border-radius: 13px;
          padding: 15px;
          width: 100%;
          font-weight: 500;
          font-size: 17px;
          line-height: 21px;
          color: #86868b;
          border: 1px solid #86868b80;
          &::placeholder {
            color: #86868b;
            font-weight: 500;
            font-size: 17px;
            line-height: 21px;
          }
        }
        .main-screen__form-select {
          background: #fff;
          border-radius: 13px;
          padding: 15px;
          width: 100%;
          font-weight: 500;
          font-size: 17px;
          line-height: 21px;
          color: #86868b;
          border: 1px solid #86868b80;
          &::placeholder {
            color: #86868b;
            font-weight: 500;
            font-size: 17px;
            line-height: 21px;
          }
        }
      }
    }
  }
  &__img {
    width: 100%;
    position: relative;
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
      border-radius: 17px;
    }
    img {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 75%;
      height: 50%;
    }
  }
}
</style>
