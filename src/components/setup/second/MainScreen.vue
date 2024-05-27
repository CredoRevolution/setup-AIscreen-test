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

<style lang="scss" scoped></style>
