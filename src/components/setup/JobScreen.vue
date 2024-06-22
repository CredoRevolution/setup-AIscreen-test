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
          src="@/assets/img/logo.svg"
          alt="aiscreen"
          class="main-screen-main__logo"
        />
        <h2 class="main-screen-main__title">To sum it all up...</h2>
        <p class="main-screen-main__text">
          Digital Signage has never been that easy.
        </p>
        <form action="#" class="main-screen-main__form">
          <div class="main-screen__form-item">
            <CustomInput
              :placeholderText="'Job Title'"
              ref="validation1"
              :defaultErrorText="'This field is required'"
              :input-name="'Job'"
            />
          </div>
          <div class="main-screen__form-item">
            <!-- <CustomSelect
              :options="['1', '2', '3']"
              :default="'Job function'"
              v-model="job"
              class="main-screen__form-select_job main-screen__form-item-warnings"
              ref="validation2"
              :defaultErrorText="'Select Job function'"
            /> -->
            <SearchSelect
              :optionsCount="jobFunctions"
              required
              class="main-screen__form-item-warnings"
              ref="validation2"
              :defaultText="'Job function'"
              :defaultErrorText="'Select job function'"
              :search="false"
            />
          </div>

          <div class="main-screen__form-item">
            <CustomInput
              :placeholderText="'Phone number'"
              :phone="true"
              ref="validation3"
              :defaultErrorText="'This field is required'"
              :input-name="'Phone'"
            />
          </div>
          <p class="main-screen__form-text">
            How many screens do you intend to connect?
          </p>
          <CustomTabs
            ref="validation4"
            :defaultErrorText="'Select amount'"
            class="custom-tabs"
          />
          <button
            class="main-screen__form-btn hover-btn blue-btn"
            @click.prevent="nextScreen"
          >
            Next step
          </button>
          <a
            href="#"
            class="main-screen__btn prev-btn"
            @click.prevent="prevScreen"
            ref="prevBtn"
            >Previous step</a
          >
        </form>
      </div>
      <div class="main-screen__img">
        <div class="backgorund">
          <img
            v-if="industry !== 'Digital Menu Boards' && industry !== 'Retail'"
            :src="require(`@/assets/img/${industry}/bg.png`)"
            alt="img"
            class="backgorund__img"
          />
          <video
            v-else-if="industry === 'Digital Menu Boards'"
            autoplay
            muted
            loop
            alt="img"
            class="backgorund__img"
            ref="bigImg"
            @click.prevent
          >
            <source
              :src="require(`@/assets/img/${industry}/bg.mp4`)"
              type="video/mp4"
              alt="img"
            />
          </video>
          <video
            v-else-if="industry === 'Retail'"
            autoplay
            muted
            loop
            controls="false"
            alt="img"
            class="backgorund__img"
            ref="bigImg"
            @click.prevent
          >
            <source
              :src="require(`@/assets/img/${industry}/bg.mp4`)"
              type="video/mp4"
              alt="img"
            />
          </video>
        </div>
        <div class="img-wrapper">
          <img :src="require(`@/assets/img/${industry}/main.png`)" alt="img" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CustomInput from '@/components/form/CustomInput.vue'
import CustomTabs from '@/components/form/CustomTabs.vue'
import SearchSelect from '../form/SearchSelect.vue'
export default {
  name: 'LastlyScreen',
  components: {
    CustomInput,
    CustomTabs,
    SearchSelect,
  },
  data() {
    return {
      job: 'Job function',
      validationCount: 0,
      amount: 0,
      adaptationResolution: 0,
      jobFunctions: [
        {
          name: 'Data & Analytics',
        },
        {
          name: 'Execute Board',
        },
        {
          name: 'Finance',
        },
        {
          name: 'HR',
        },
        {
          name: 'Internal Comms',
        },
        {
          name: 'IT',
        },
        {
          name: 'Marketing',
        },
        {
          name: 'Operations',
        },
        {
          name: 'Other',
        },
      ],
    }
  },

  methods: {
    onresize() {
      this.adaptationResolution = window.innerWidth
    },
    prevScreen() {
      this.$emit('prevScreen')
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
      if (this.validationCount === validations.length) {
        return true
      }
    },
    nextScreen() {
      if (this.checkAllValidations()) this.$emit('nextScreen')
    },
  },
  props: {
    industry: {
      type: String,
      default: 'plug',
    },
  },
  created() {
    this.onresize()
  },
}
</script>
