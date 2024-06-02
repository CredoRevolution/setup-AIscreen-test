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
            <CustomInput :placeholderText="'Input Text'" ref="validation" />
          </div>
          <div class="main-screen__form-item">
            <CustomSelect
              :options="industries"
              :default="'Industry'"
              v-model.trim="industry"
              class="main-screen__form-select_industry main-screen__form-item-warnings"
              @getInfo="getInfo"
            />
          </div>

          <div class="main-screen__form-item">
            <SearchSelect
              :optionsCount="Countries"
              @USAState="USAState"
              required
              class="main-screen__form-item-warnings"
              v-model="selectedCountry"
            />
          </div>
          <div
            class="main-screen__form-item"
            v-if="selectedCountry === 'United States'"
          >
            <SearchSelect
              :optionsCount="States"
              required
              class="main-screen__form-item-warnings"
              v-model="state"
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
import SearchSelect from '@/components/form/SearchSelect.vue'
import { required, minLength, between } from 'vuelidate/lib/validators'
import axios from 'axios'

export default {
  name: 'WelcomeScreen',
  data() {
    return {
      selectedCountry: '',
      industry: '',
      state: '',
      name: '',
      industries: ['Finance', 'Digital Menu Boards'],
      Countries: [],
      States: [],
    }
  },
  components: {
    CustomSelect,
    CustomInput,
    SearchSelect,
  },
  computed: {},
  methods: {
    nextScreen() {
      console.log(this.$refs)
      if (this.$refs['validation'] && this.$refs['validation'].length) {
        this.$refs['validation'].filter((el) => {
          el.checkValidation()
          if (!el.checkValidation()) {
            console.log('checkValidationFalse')
          } else {
            console.log('checkValidationTrue')
          }
        })
      } else {
        this.$refs['validation'].checkValidation()
        console.log('123')
      }
      console.log('nextScreen')
      this.$emit('nextScreen')
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
    getCountries() {
      axios
        .get(
          `https://raw.githubusercontent.com/David-Haim/CountriesToCitiesJSON/master/countriesToCities.json`
        )
        .then((response) => {
          this.Countries = response.data
          this.Countries = Object.keys(response.data).map((key) => ({
            name: key,
          }))
          console.log('Countries', this.Countries)
        })
    },
    getUSStates() {
      axios
        .get(
          `https://raw.githubusercontent.com/aruljohn/us-states/master/states.json`
        )
        .then((response) => {
          this.States = response.data
          console.log('States', this.States)
          this.States = Object.keys(response.data).map((key) => ({
            name: key,
          }))
        })
    },
    getInfo(industry) {
      this.industry = industry
      for (let i = 0; i < this.industries.length; i++) {
        if (this.industries[i] === industry) {
          this.changeIndustry()
        }
      }
    },
    USAState(value) {
      this.selectedCountry = value
    },
  },
  mounted() {
    this.getCountries()
    this.getUSStates()
    this.changeIndustry()
  },
}
</script>

<style lang="scss" scoped></style>
