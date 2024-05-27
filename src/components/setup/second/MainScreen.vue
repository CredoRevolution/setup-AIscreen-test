<template>
  <div class="main-screen">
    <div class="main-screen-wrapper">
      <div class="main-screen__main">
        <img
          v-if="require(`@/assets/logo.svg`)"
          :src="require(`@/assets/logo.svg`)"
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
        <form
          action="#"
          :class="[
            'main-screen-main__form',
            { 'state-active': selectedCountry === 'USA' },
          ]"
        >
          <div class="main-screen__form-item">
            <input
              type="text"
              class="main-screen__form-input"
              placeholder="Input text"
              required
            />
          </div>
          <div class="main-screen__form-item">
            <CustomSelect
              :options="industries"
              :default="'Industry'"
              v-model="industry"
              class="main-screen__form-select_industry"
              @getInfo="getInfo"
              required
            />
          </div>
          <div class="main-screen__form-item">
            <CustomSelect
              :options="['USA', 'Russia', 'Australia']"
              :default="'Country'"
              v-model="selectedCountry"
              required
            />
          </div>
          <div class="main-screen__form-item" v-if="selectedCountry === 'USA'">
            <CustomSelect
              :options="['1', '2', '3']"
              :default="'State'"
              required
            />
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
            v-if="industry && require(`@/assets/${industry}.png`)"
            :src="require(`@/assets/${industry}.png`)"
            alt="img"
            class="backgorund__img"
          />
        </div>
        <img
          v-if="industry && require(`@/assets/${industry}-small.png`)"
          :src="require(`@/assets/${industry}-small.png`)"
          alt="img"
          class="main-screen-main__small-img"
        />
      </div>
    </div>
  </div>
</template>

<script>
import CustomSelect from '@/components/form/CustomSelect.vue'
export default {
  name: 'MainScreen',
  data() {
    return {
      selectedCountry: '',
      industry: 'Industry',
      industries: [
        // 'Energy',
        // 'Events',
        // 'Education',
        'Finance',
        // 'Fitness',
        // 'Healthcare',
        // 'Hospitality/Food Beverage',
        // 'Manufacturing',
        // 'Media & Advertising',
        // 'Places Of Worship',
        // 'Real Estate',
        'Retail',
        'Digital Menu Boards',
        // 'Software & Services',
        // 'Telecommunication',
        // 'Other',
      ],
    }
  },
  components: {
    CustomSelect,
  },
  computed: {},
  methods: {
    nextScreen() {
      const form = this.$el.querySelector('form')
      const requiredFields = form.querySelectorAll('[required]')
      let isValid = true

      requiredFields.forEach((field) => {
        console.log(field.industry)
        if (field.value === '') {
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
    changeIndustry() {
      const selectElement = this.$el.querySelector(
        '.main-screen__form-select_industry'
      )
      const img = this.$el.querySelector('.main-screen-main__small-img')
      const bigImg = this.$el.querySelector('.backgorund__img')
      console.log('current industry', this.industry)

      if (this.industry) {
        console.log('123')
        if (img && bigImg) {
          bigImg.style.transition = 'all 0.2s ease-out'
          bigImg.style.opacity = 0
          img.style.transition = 'all 0.2s ease-out'
          img.style.opacity = 0
          console.log('change industry completed')
        }
      }

      setTimeout(() => {
        this.$emit('changeIndustry', this.industry)
        console.log(this.industry)
      }, 300)

      if (this.industry) {
        if (img && bigImg) {
          setTimeout(() => {
            bigImg.style.opacity = 1
            bigImg.style.transition = 'all 0.2s ease-out'
            setTimeout(() => {
              img.style.opacity = 1
              img.style.transition = 'all 0.2s ease-out'
            }, 300)
          }, 300)
        }
      }
    },
    getInfo(industry) {
      console.log('input', industry)
      this.industry = industry
      for (let i = 0; i < this.industries.length; i++) {
        if (this.industries[i] === industry) {
          this.changeIndustry()
        }
      }
    },
  },
  mounted() {
    this.changeIndustry()
  },
}
</script>

<style lang="scss" scoped>
.main-screen {
  max-height: 95%;
  max-width: 90%;
  height: 95%;
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
      .main-screen__form-item {
        position: relative;

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
      &::after {
        content: '';
        width: 110%;
        height: 110%;
        background: rgba(0, 0, 0, 0.5);
        position: absolute;
        top: 0;
        left: 0;
        border-radius: 17px;
      }
    }
  }
}
</style>
