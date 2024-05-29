<template>
  <div class="main-screen">
    <div class="main-screen-wrapper">
      <div class="main-screen__main">
        <img
          src="@/assets/img/logo.svg"
          alt="aiscreen"
          class="main-screen-main__logo"
        />
        <h2 class="main-screen-main__title">And lastly...</h2>
        <p class="main-screen-main__text">
          Show your content on screen or in demo mode in 60 seconds. Get started
          quickly and easily.
        </p>
        <form action="#" class="main-screen-main__form state-active">
          <div class="main-screen__form-item">
            <CustomInput :placeholderText="'Job Title'" />
          </div>
          <div class="main-screen__form-item">
            <CustomSelect
              :options="['1', '2', '3']"
              :default="'Job function'"
              v-model="job"
              class="main-screen__form-select_job main-screen__form-item-warnings"
            />
          </div>

          <div class="main-screen__form-item">
            <CustomInput :placeholderText="'Phone number'" />
          </div>
          <p class="main-screen__form-text">
            How many screens do you intend to connect?
          </p>
          <div class="main-screen__form-switch main-screen__form-item-warnings">
            <button
              class="main-screen__form-switch-btn passive"
              type="button"
              @click.prevent="makeActive"
            >
              1-10
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              11-50
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              51-199
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              200-499
            </button>
            <button
              class="main-screen__form-switch-btn"
              type="button"
              @click.prevent="makeActive"
            >
              500+
            </button>
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

export default {
  name: 'LastlyScreen',
  components: {
    CustomSelect,
    CustomInput,
  },
  data() {
    return {
      job: 'Job function',
      validation: false,
    }
  },
  methods: {
    checkValidation() {
      const form = this.$el.querySelector('form')
      const requiredFields = form.querySelectorAll('[required]')
      const allFields = form.querySelectorAll(
        'form .main-screen__form-item-warnings'
      )
      const switchBtn = this.$el.querySelectorAll(
        '.main-screen__form-switch-btn'
      )
      const switchBtnWrapper = this.$el.querySelector(
        '.main-screen__form-switch'
      )

      this.validation = true

      switchBtn.forEach((btn) => {
        if (btn.classList.contains('passive')) {
          this.validation = false
          switchBtnWrapper.classList.add('warning')
        }
      })
      if (this.job === 'Job function') {
        this.validation = false
        form
          .querySelector('.main-screen__form-select_job')
          .classList.add('warning') // Add a CSS class for styling
      } else {
        form
          .querySelector('.main-screen__form-select_job')
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
    makeActive(e) {
      const buttons = this.$el.querySelectorAll('.main-screen__form-switch-btn')
      buttons.forEach((button) => {
        button.classList.remove('active')
        button.classList.remove('passive')
      })
      e.target.classList.add('active')
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
