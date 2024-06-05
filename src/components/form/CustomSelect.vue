<template>
  <div class="custom-select-wrapper" @click="resetValidation">
    <div
      class="custom-select"
      :tabindex="tabindex"
      @blur="open = false"
      :class="{ error: $v.selected.$error }"
    >
      <div class="selected" :class="{ open: open }" @click="open = !open">
        {{ selected }}
      </div>
      <div class="items" :class="{ selectHide: !open }">
        <div
          v-for="(option, i) of options"
          :key="i"
          @click="
            selected = option
            open = false
            $emit('input', option)
            returnInfo()
          "
        >
          <div
            class="item"
            :class="{ active: selected == option }"
            @click="setSelected(option)"
          >
            {{ option }}
          </div>
        </div>
      </div>
    </div>
    <p v-if="showError && !$v.selected.mustBeSelected" class="error-message">
      {{ defaultErrorText }}
    </p>
  </div>
</template>

<script>
import { required } from 'vuelidate/lib/validators'

const mustBeSelected = (value, vm) => value !== vm.default
export default {
  name: 'CustomSelect',

  props: {
    options: {
      type: Array,
      required: true,
    },
    default: {
      type: String,
      required: false,
      default: null,
    },
    defaultErrorText: {
      type: String,
      required: false,
    },
    tabindex: {
      type: Number,
      required: false,
      default: 0,
    },
  },
  data() {
    return {
      selected: this.default
        ? this.default
        : this.options.length > 0
        ? this.options[0]
        : null,
      open: false,
      selected: '',
      showError: false,
    }
  },
  validations: {
    selected: {
      required,
      mustBeSelected,
    },
  },
  methods: {
    returnInfo() {
      this.$emit('getInfo', this.selected)
    },
    setSelected(option) {
      if (this.option === 'Industry') {
      }
      this.selected = option
    },
    checkValidation() {
      if (this.$v) {
        this.$v.$touch()
        if (!this.$v.$invalid) {
          return true
        }
        this.showError = true
        return false
      }
    },
    resetValidation() {
      this.$v.$reset()
      this.showError = false
    },
  },
  mounted() {
    this.setSelected(this.default)
    this.resetValidation()
    this.$emit('getInfo', this.selected)
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}

.custom-tabs_adaptation {
  display: none;
}
.custom-select {
  position: relative;
  width: 100%;
  text-align: left;
  outline: none;
  border-radius: rem(13px);
  width: 100%;
  font-weight: 500;
  font-size: rem(17px);
  line-height: rem(21px);
  color: #86868b;
  border: 1px solid transparent;

  &::after {
    content: '';
    width: rem(40px);
    height: rem(40px);
    position: absolute;
    top: rem(6px);
    right: rem(6px);
    background: #86868b29;
    border-radius: rem(10px);
    transition: all 0.3s ease;
  }
  &.error {
    .selected {
      border: 1px solid red;
    }
  }
}

.main-screen__form-select {
  width: 100%;
  font-weight: 500;
  font-size: rem(17px);
  line-height: rem(21px);
  color: #86868b;
  &::placeholder {
    color: #86868b;
    font-weight: 500;
    font-size: rem(17px);
    line-height: rem(21px);
  }
}

.custom-select .selected {
  background-color: #fff;
  color: #86868b;
  cursor: pointer;
  user-select: none;
  border-radius: rem(13px);
  padding: rem(15px);
  border: 1px solid #86868b80;
}

.custom-select.warning {
  .selected {
    color: #d42b2b;
  }
}

.custom-select .selected.open {
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
  border: 1px solid #0071e2;
  padding: rem(15px);
  box-shadow: 0px 0px 8px #0071e254;
  border-bottom: 1px solid #86868b80;
  &:after {
    transform: rotateX(180deg);
  }
}

.custom-select .selected:after {
  position: absolute;
  content: '';
  top: rem(6px);
  right: rem(6px);
  width: rem(40px);
  height: rem(40px);
  background: url('@/assets/img/arrow-down.svg');
  background-position: center;
  background-repeat: no-repeat;
  transition: all 0.3s ease;
  border-radius: rem(10px);
  z-index: 3;
}

.custom-select .items {
  color: #86868b;
  border-bottom-left-radius: 13px;
  border-bottom-right-radius: 13px;
  box-shadow: 0px 0px 8px #0071e254;
  overflow: hidden;
  border: 1px solid #0071e2;
  border-top: 0;
  position: absolute;
  background-color: #fff;
  left: 0;
  right: 0;
  top: 100%;
  z-index: 100;
}

.custom-select .items .item {
  color: #86868b;
  padding: rem(15px);
  cursor: pointer;
  user-select: none;
  &:hover {
    transition: all 0.3s ease;
    background: rgba(0, 0, 0, 0.05);
  }
  &.active {
    color: #14121f;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    &:after {
      content: url(@/assets/img/check.svg);
    }
  }
}

.custom-select .items div:hover {
  color: #14121f;
}

.selectHide {
  display: none;
}

@media (max-height: 900px) {
  .custom-select {
    .selected {
      padding: rem(12px);
      &.open {
        padding: rem(12px);
      }
      &::after {
        width: rem(38px);
        height: rem(38px);
      }
    }
    &::after {
      top: rem(4px);
      width: rem(38px);
      height: rem(38px);
    }
  }
}
@media (max-width: 768px) {
  .custom-tabs_adaptation {
    display: unset;
    .custom-select {
      .items {
        max-height: 280%;
        overflow-y: scroll;
      }
    }
  }
}
</style>
