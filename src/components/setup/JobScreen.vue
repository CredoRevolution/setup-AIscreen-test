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
              :input-name="'Job'"
              ref="validation1"
              :defaultErrorText="'This field is required'"
              @getData="getData"
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
              @getData="getData"
            />
          </div>

          <div class="main-screen__form-item">
            <!-- <CustomInput
              ref="validation3"
              :placeholderText="'Phone number'"
              :phone="true"
              :defaultErrorText="'This field is required'"
              :input-name="'Phone'"
              @getData="getData"
            /> -->
            <VuePhoneNumberInput
              v-model="phone"
              fetch-country
              class="main-screen__form-input"
              :class="{ error: phoneValid && isPhoneDirty && phone !== '' }"
              :required="true"
              :placeholder="'Phone number'"
              @update="checkPhone"
              ref="validation3"
              :error="!phoneValid && isPhoneDirty"
              :borderRadius="13"
              :errorColor="'red'"
              :validColor="'#0071e2'"
              :color="'#0071e2'"
              :size="'lg'"
            />
          </div>
          <p class="main-screen__form-text">
            How many screens do you intend to connect?
          </p>
          <CustomTabs
            ref="validation4"
            class="custom-tabs"
            :defaultErrorText="'Select amount'"
            :definition="'amount of screens'"
            @getData="getData"
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
            playsinline
            ref="bigImg"
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
            playsinline
            muted
            loop
            alt="img"
            class="backgorund__img"
            ref="bigImg"
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
import { data } from 'qrcode.vue'

import VuePhoneNumberInput from 'vue-phone-number-input'
import 'vue-phone-number-input/dist/vue-phone-number-input.css'

export default {
  name: 'LastlyScreen',
  components: {
    CustomInput,
    CustomTabs,
    SearchSelect,
    VuePhoneNumberInput,
  },
  data() {
    return {
      job: 'Job function',
      validationCount: 0,
      amount: 0,
      adaptationResolution: 0,
      data: {},
      isPhoneDirty: false,
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
      phone:
        this.countryCode && this.values && this.values.Phone
          ? '+' + this.countryCode + ' ' + this.values.Phone
          : '',
      phoneValid: true,
    }
  },

  methods: {
    onresize() {
      this.adaptationResolution = window.innerWidth
    },
    checkPhone() {
      this.phoneValid = this.$refs.validation3.results.isValid
      this.isPhoneDirty = true
      console.log(this.phoneValid)
      if (this.phoneValid) {
        this.data.phone = this.$refs.validation3.results.formattedNumber
        return true
      }
    },
    prevScreen() {
      this.$emit('prevScreen')
    },
    getData(fieldName, value) {
      this.data[fieldName] = value
    },
    checkAllValidations() {
      this.validationCount = 0
      const validations = [
        this.$refs.validation1,
        this.$refs.validation2,
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
      if (this.validationCount === validations.length) {
        this.getData()
        return true
      }
    },
    nextScreen() {
      if (this.checkAllValidations() && this.checkPhone())
        this.$emit('nextScreen', this.data)
    },
  },
  props: {
    industry: {
      type: String,
      default: 'Industry',
    },
  },
  created() {
    this.onresize()
  },
  mounted() {
    console.log(this.$refs.validation3)
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return $px / 16px + 0rem;
}

.input-tel.lg .input-tel__input {
  font-size: rem(17px);
  line-height: rem(21px);
  font-weight: 500;
  font-family: Satoshi-variable, sans-serif;
}
</style>
