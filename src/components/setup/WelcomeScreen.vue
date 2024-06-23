<template>
  <div class="main-screen">
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
              :placeholderText="'Name of your company'"
              ref="validation1"
              :defaultErrorText="'This field is required'"
              :input-name="'Company'"
            />
          </div>
          <div class="main-screen__form-item">
            <SearchSelect
              :optionsCount="industries.slice(1)"
              required
              class="main-screen__form-item-warnings"
              v-model="industry"
              ref="validation2"
              :defaultText="'Industry'"
              :defaultErrorText="'Select industry'"
              :industry="true"
              :search="false"
              @getInfo="getInfo"
            />
          </div>

          <div class="main-screen__form-item">
            <SearchSelect
              @USAState="USAState"
              required
              class="main-screen__form-item-warnings"
              v-model="selectedCountry"
              ref="validation3"
              :optionsCount="Countries"
              :defaultText="'Select Country'"
              :defaultErrorText="'Select country'"
            />
          </div>
          <div
            class="main-screen__form-item"
            v-if="selectedCountry === 'United States'"
          >
            <SearchSelect
              required
              class="main-screen__form-item-warnings"
              v-model="state"
              ref="validation4"
              :optionsCount="States"
              :defaultText="'Select State'"
              :defaultErrorText="'Select state'"
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

      <div class="main-screen__img" v-if="industry">
        <swiper
          class="swiper-main"
          @swiper="onSwiper"
          :modules="[Navigation]"
          :slides-per-view="1"
          :loop="true"
          :direction="'horizontal'"
          :space-between="0"
          :speed="500"
          :allow-touch-move="false"
          ref="swiper"
        >
          <SwiperSlide
            v-for="(currentIndustry, index) in industries"
            :key="index"
          >
            <div class="backgorund">
              <img
                v-if="
                  currentIndustry.name !== 'Digital Menu Boards' &&
                  currentIndustry.name !== 'Retail'
                "
                :src="
                  industryPrev || industry
                    ? require(`@/assets/img/${currentIndustry.name}/bg.png`)
                    : null
                "
                alt="img"
                class="backgorund__img"
                ref="bigImg"
              />
              <video
                v-else-if="currentIndustry.name === 'Digital Menu Boards'"
                autoplay
                muted
                loop
                alt="img"
                class="backgorund__img"
                ref="bigImg"
                playsinline
              >
                <source
                  :src="require(`@/assets/img/${currentIndustry.name}/bg.mp4`)"
                  type="video/mp4"
                  alt="img"
                />
              </video>
              <video
                v-else-if="currentIndustry.name === 'Retail'"
                autoplay
                playsinline
                muted
                loop
                alt="img"
                class="backgorund__img"
                ref="bigImg"
              >
                <source
                  :src="require(`@/assets/img/${currentIndustry.name}/bg.mp4`)"
                  type="video/mp4"
                  alt="img"
                />
              </video>
            </div>
            <div class="img-wrapper">
              <img
                :src="
                  industry
                    ? require(`@/assets/img/${currentIndustry.name}/main.png`)
                    : null
                "
                alt="img"
                class="main-screen-main__small-img background"
                ref="smallImg"
              />
            </div>
          </SwiperSlide>
        </swiper>
      </div>
    </div>
  </div>
</template>

<script>
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation } from 'swiper/modules'
import CustomInput from '@/components/form/CustomInput.vue'
import SearchSelect from '@/components/form/SearchSelect.vue'
import axios from 'axios'

export default {
  name: 'WelcomeScreen',
  data() {
    return {
      selectedCountry: '',
      industry: '',
      industryPrev: '',
      industryPrevPrev: '',
      state: '',
      name: '',
      industries: [
        { name: 'Industry' },
        { name: 'Corporate' },
        { name: 'Education' },
        { name: 'Fitness' },
        { name: 'Healthcare' },
        { name: 'Manufacturing' },
        { name: 'Retail' },
        { name: 'Digital Menu Boards' },

        // { name: 'Energy' },
        // { name: 'Events' },
        // { name: 'Finance' },
        // { name: 'Hospitality/Food Beverage' },
        // { name: 'Media & Advertising' },
        // { name: 'Places Of Worship' },
        // { name: 'Real Estate' },
        // { name: 'Software & Services' },
        // { name: 'Telecommunications' },
        // { name: 'Other' },
      ],
      Countries: [],
      States: [],
      validationCount: 0,
    }
  },
  components: {
    CustomInput,
    SearchSelect,
    Swiper,
    SwiperSlide,
  },
  setup() {
    return {
      Navigation,
    }
  },
  computed: {},
  methods: {
    onSwiper(swiper) {
      this.swiper = swiper
    },
    handleSlideTo() {
      this.swiper.slideTo(
        this.industries.findIndex((item) => item.name === this.industry)
      )
    },
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
      this.$emit('changeIndustry', this.industry)
      this.handleSlideTo()
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
      this.industryPrev = this.industry
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

  created() {
    const images = [
      require('@/assets/img/loading.svg'),
      require('@/assets/img/Corporate/main.png'),
      require('@/assets/img/Corporate/bg.png'),
      require('@/assets/img/Education/main.png'),
      require('@/assets/img/Education/bg.png'),
      require('@/assets/img/Digital Menu Boards/main.png'),
      require('@/assets/img/Retail/main.png'),
      require('@/assets/img/Retail/bg.mp4'),
      require('@/assets/img/Healthcare/main.png'),
      require('@/assets/img/Healthcare/bg.png'),
      require('@/assets/img/Fitness/main.png'),
      require('@/assets/img/Fitness/bg.png'),
      require('@/assets/img/Manufacturing/main.png'),
      require('@/assets/img/Manufacturing/bg.png'),
      // require('@/assets/img/industries/Digital Menu Boards.png'),
      // require('@/assets/img/industries/Digital Menu Boards-small.png'),
      // require('@/assets/img/industries/Digital Menu Boards.mp4'),
    ]

    images.forEach((img) => {
      const image = new Image()
      image.src = img
    })
  },
  mounted() {
    this.getCountries()
    this.getUSStates()
    this.changeIndustry()
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}
.swiper.swiper-main {
  .swiper-slide * > div {
    border-radius: 0;
  }
  width: 100%;
  height: 100%;
  .swiper-slide {
    height: unset;
    opacity: 1;
    .backgorund {
      left: 0 !important;
      border-radius: rem(17px);
      overflow: hidden;
      video {
        width: auto;
        height: 100%;
        pointer-events: none;
      }
    }
  }
}
</style>
