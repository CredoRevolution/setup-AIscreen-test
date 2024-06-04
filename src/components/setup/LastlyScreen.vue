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
            <CustomSelect
              :options="['1', '2', '3']"
              :default="'Job function'"
              v-model="job"
              class="main-screen__form-select_job main-screen__form-item-warnings"
              ref="validation2"
              :defaultErrorText="'Select Job function'"
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
          <CustomTabs ref="validation4" :defaultErrorText="'Select Amount'" />
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
        <img :src="require(`@/assets/img/${industry}-small.png`)" alt="img" />
      </div>
    </div>
  </div>
</template>

<script>
import CustomSelect from '@/components/form/CustomSelect.vue'
import CustomInput from '@/components/form/CustomInput.vue'
import CustomTabs from '@/components/form/CustomTabs.vue'
export default {
  name: 'LastlyScreen',
  components: {
    CustomSelect,
    CustomInput,
    CustomTabs,
  },
  data() {
    return {
      job: 'Job function',
      validationCount: 0,
    }
  },
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
}
</script>
