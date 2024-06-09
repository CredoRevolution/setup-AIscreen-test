<template>
  <div class="main-screen">
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
              :defaultErrorText="'Please Fill In This Field'"
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
              :defaultErrorText="'Select Job function'"
              :search="false"
            />
          </div>

          <div class="main-screen__form-item">
            <CustomInput
              :placeholderText="'Phone number'"
              :phone="true"
              ref="validation3"
              :defaultErrorText="'Please Fill In This Field'"
            />
          </div>
          <p class="main-screen__form-text">
            How many screens do you intend to connect?
          </p>
          <CustomTabs
            ref="validation4"
            :defaultErrorText="'Select Amount'"
            class="custom-tabs"
            v-if="adaptationResolution > 768"
          />
          <CustomSelect
            :options="['1-10', '11-50', '51-199', '200-499', '500+']"
            :default="'Amount of screens'"
            :defaultErrorText="'Select Amount'"
            class="custom-tabs_adaptation"
            v-model="amount"
            ref="validation4"
            v-else
          />
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
            :src="require(`@/assets/img/${industry}.png`)"
            alt="img"
            class="backgorund__img"
          />
        </div>
        <div class="img-wrapper">
          <img :src="require(`@/assets/img/${industry}-small.png`)" alt="img" />
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
          name: 'Accounting',
        },
        {
          name: 'Advertising',
        },
        {
          name: 'Education',
        },
        {
          name: 'Engineering',
        },
        {
          name: 'Finance',
        },
        {
          name: 'Healthcare',
        },
      ],
    }
  },
  methods: {
    onresize() {
      this.adaptationResolution = window.innerWidth
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
