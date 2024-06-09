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
            <CustomInput
              :placeholderText="'Input Text'"
              ref="validation1"
              :defaultErrorText="'Please Fill In This Field'"
            />
          </div>
          <!-- <div class="main-screen__form-item">
            <CustomSelect
              :options="industries"
              :default="'Industry'"
              v-model.trim="industry"
              class="main-screen__form-select_industry main-screen__form-item-warnings"
              @getInfo="getInfo"
              ref="validation2"
              :defaultErrorText="'Select Industry'"
            />
          </div> -->
          <div class="main-screen__form-item">
            <SearchSelect
              :optionsCount="industries"
              required
              class="main-screen__form-item-warnings"
              v-model="industry"
              ref="validation2"
              :defaultText="'Industry'"
              :defaultErrorText="'Select Industry'"
              :industry="true"
              :search="false"
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
              ref="validation3"
              :defaultText="'Select Country'"
              :defaultErrorText="'Select Country'"
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
              ref="validation4"
              :defaultText="'Select State'"
              :defaultErrorText="'Select State'"
            />
          </div>
          <button
            class="main-screen__form-btn hover-btn blue-btn"
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
            ref="bigImg"
          />
        </div>
        <div class="img-wrapper">
          <img
            v-if="industry && require(`@/assets/img/${industry}-small.png`)"
            :src="require(`@/assets/img/${industry}-small.png`)"
            alt="img"
            class="main-screen-main__small-img background"
            ref="smallImg"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CustomInput from '@/components/form/CustomInput.vue'
import SearchSelect from '@/components/form/SearchSelect.vue'
import axios from 'axios'

export default {
  name: 'WelcomeScreen',
  data() {
    return {
      selectedCountry: '',
      industry: '',
      state: '',
      name: '',
      industries: [
        { name: 'Finance' },
        { name: 'Digital Menu Boards' },
        { name: 'Retail' },
      ],
      Countries: [],
      States: [],
      validationCount: 0,
    }
  },
  components: {
    CustomInput,
    SearchSelect,
  },
  computed: {},
  methods: {
    checkAllValidations() {
      this.validationCount = 0
      const validations = [
        this.$refs.validation1,
        this.$refs.validation2,
        this.$refs.validation3,
        this.$refs.validation4,
      ]
      validations.forEach((item) => {
        if (item) {
          item.checkValidation()
        } else {
          return
        }
        if (item.checkValidation()) {
          this.validationCount++
        }
      })
      console.log(this.validationCount, 'validations of', validations.length)
      if (this.selectedCountry !== 'United States') {
        if (this.validationCount === validations.length - 1) {
          return true
        }
      }
      if (this.validationCount === validations.length) {
        return true
      }
    },
    nextScreen() {
      if (this.checkAllValidations()) this.$emit('nextScreen')
    },
    changeIndustry() {
      const img = this.$refs.smallImg
      const bigImg = this.$refs.bigImg
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
        })
    },
    getUSStates() {
      axios
        .get(
          `https://raw.githubusercontent.com/aruljohn/us-states/master/states.json`
        )
        .then((response) => {
          this.States = response.data
          this.States = Object.keys(response.data).map((key) => ({
            name: key,
          }))
        })
    },
    getInfo(industry) {
      this.industry = industry
      for (let i = 0; i < this.industries.length; i++) {
        if (this.industries[i].name === industry) {
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
