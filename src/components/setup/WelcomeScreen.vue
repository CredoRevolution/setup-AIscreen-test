<template>
  <div class="main-screen">
    <div class="main-screen-wrapper">
      <div class="main-screen__main">
        <img
          v-if="require(`@/assets/img/logo.svg`)"
          :src="require(`@/assets/img/logo.svg`)"
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
            <CustomInput :placeholderText="'Input Text'" />
          </div>
          <div class="main-screen__form-item">
            <CustomSelect
              :options="industries"
              :default="'Industry'"
              v-model="industry"
              class="main-screen__form-select_industry main-screen__form-item-warnings"
              @getInfo="getInfo"
            />
          </div>

          <div class="main-screen__form-item">
            <CustomSelect
              :options="['USA', 'Russia', 'Australia']"
              :default="'Country'"
              v-model="selectedCountry"
              class="main-screen__form-select_country main-screen__form-item-warnings"
            />
          </div>
          <div class="main-screen__form-item" v-if="selectedCountry === 'USA'">
            <CustomSelect
              :options="['1', '2', '3']"
              :default="'State'"
              v-model="state"
              class="main-screen__form-select_state main-screen__form-item-warnings"
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
            v-if="industry && require(`@/assets/img/${industry}.png`)"
            :src="require(`@/assets/img/${industry}.png`)"
            alt="img"
            class="backgorund__img"
          />
        </div>
        <img
          v-if="industry && require(`@/assets/img/${industry}-small.png`)"
          :src="require(`@/assets/img/${industry}-small.png`)"
          alt="img"
          class="main-screen-main__small-img"
        />
      </div>
    </div>
  </div>
</template>

<script>
import CustomSelect from '@/components/form/CustomSelect.vue'
import CustomInput from '@/components/form/CustomInput.vue'
export default {
  name: 'WelcomeScreen',
  data() {
    return {
      selectedCountry: 'Country',
      industry: 'Industry',
      state: 'State',
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
      validation: false,
    }
  },
  components: {
    CustomSelect,
    CustomInput,
  },
  computed: {},
  methods: {
    checkValidation() {
      const form = this.$el.querySelector('form')
      const requiredFields = form.querySelectorAll('[required]')
      const allFields = form.querySelectorAll(
        'form .main-screen__form-item-warnings'
      )
      this.validation = true

      if (this.selectedCountry === 'Country') {
        this.validation = false
        form
          .querySelector('.main-screen__form-select_country')
          .classList.add('warning') // Add a CSS class for styling
      } else {
        form
          .querySelector('.main-screen__form-select_country')
          .classList.remove('warning')
      }
      if (this.industry === 'Industry') {
        this.validation = false
        form
          .querySelector('.main-screen__form-select_industry')
          .classList.add('warning') // Add a CSS class for styling
      } else {
        form
          .querySelector('.main-screen__form-select_industry')
          .classList.remove('warning')
      }
      if (this.selectedCountry === 'USA' && this.state === 'State') {
        this.validation = false
        form
          .querySelector('.main-screen__form-select_state')
          .classList.add('warning') // Add a CSS class for styling
      } else if (this.selectedCountry === 'USA' && this.state !== 'State') {
        form
          .querySelector('.main-screen__form-select_state')
          .classList.remove('warning')
      }

      allFields.forEach((field) => {
        field.addEventListener('click', () => {
          field.classList.remove('warning')
          field.parentNode.classList.remove('warning')
        })
      })

      requiredFields.forEach((field) => {
        if (!field.value) {
          this.validation = false
          field.parentNode.classList.add('warning') // Add a CSS class for styling
        } else {
          field.parentNode.classList.remove('warning') // Remove the CSS class if field is filled
        }
      })
    },
    nextScreen() {
      this.checkValidation()
      if (this.validation) {
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

<style lang="scss" scoped></style>
