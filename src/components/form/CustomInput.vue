<template>
  <div class="test">
    <input
      type="text"
      :class="[
        'main-screen__form-input',
        'main-screen__form-item-warnings',
        $v.name.$error ? 'error' : '',
      ]"
      :placeholder="placeholderText"
      required
      v-model.trim="$v.name.$model"
      @click="resetValidation"
    />
    <div class="error-message" v-if="showError && !$v.name.minLength">
      Name must have at least {{ $v.name.$params.minLength.min }} letters.
    </div>
    <div class="error-message" v-if="showError && !$v.name.required">
      {{ defaultErrorText }}
    </div>
  </div>
</template>

<script>
import { required, minLength } from 'vuelidate/lib/validators'
export default {
  name: 'CustomInput',
  props: {
    placeholderText: {
      type: String,
      required: true,
    },
    defaultErrorText: {
      type: String,
      required: false,
    },
  },
  data() {
    return {
      name: '',
      isValid: false,
      showError: false,
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(5),
    },
  },
  mounted() {
    this.resetValidation()
  },

  methods: {
    checkValidation() {
      if (this.$v) {
        this.$v.$touch()
        if (!this.$v.$invalid) {
          this.isValid = true
        } else {
          this.isValid = false
          this.showError = true
        }
      }
      return this.isValid
    },
    resetValidation() {
      this.$v.$reset()
      this.isValid = false
      this.showError = false
    },
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}

.main-screen__form-input {
  background: #fff;
  border-radius: rem(13px);
  padding: rem(15px);
  width: 100%;
  font-weight: 500;
  font-size: rem(17px);
  line-height: rem(21px);
  color: #86868b;
  border: 1px solid #86868b80;
  &.error {
    border: 1px solid red;
  }
  &::placeholder {
    color: #86868b;
    font-weight: 500;
    font-size: rem(17px);
    line-height: rem(21px);
  }
  &:focus {
    border: 1px solid #0071e2 !important;
    color: #14121f;
    outline: none;
    box-shadow: 0px 0px 8px #0071e254; /* Добавьте нужный вам box-shadow стиль */
    transition: all 0.3s ease;
    &::after {
      content: '';
      width: 100%;
      height: 100%;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      border-radius: rem(13px);
      z-index: 1;
      transition: all 0.3s;
      box-sizing: content-box;
      background: rgba(0, 113, 226, 0.25);
      backdrop-filter: blur(5px);
    }
  }
  &:active {
    border: 1px solid #0071e2 !important;
    color: #14121f;
  }
  &:focus-visible {
    border: 1px solid #0071e2 !important;
    outline: none;
  }
}
@media (max-height: 900px) {
  .main-screen__form-input {
    padding: rem(12px);
  }
}
</style>
