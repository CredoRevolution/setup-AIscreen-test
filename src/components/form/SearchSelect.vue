<template>
  <div>
    <multiselect
      v-model="value"
      :options="optionsCount"
      :custom-label="selectCountry"
      :value="value"
      placeholder="Select one"
      :allowEmpty="false"
      label="name"
      track-by="name"
      @select="checkState"
    ></multiselect>
    <!-- <pre class="language-json"><code>{{ value }}</code></pre> -->
  </div>
</template>

<script>
import Multiselect from 'vue-multiselect'

export default {
  name: 'SearchSelect',
  components: {
    Multiselect,
  },
  data() {
    return {
      value: { name: 'Select one' },
    }
  },
  props: {
    optionsCount: {},
  },

  methods: {
    nameWithLang({ name }) {
      return `${name}`
    },
    selectCountry({ name }) {
      return `${name}`
    },
    checkState() {
      if (this.value.name) {
        console.log(this.value.name)
        this.$emit('USAState', this.value.name)
      }
    },
  },
}
</script>

<style lang="scss">
@function rem($px) {
  @return ($px / 16px) + rem;
}
.multiselect {
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
  z-index: auto !important;
  box-sizing: border-box;

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
  .multiselect__select {
    all: unset;
    &::before {
      all: unset;
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
  }
  .multiselect__tags {
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
    border: 1px solid #86868b80;
    padding: rem(15px);
    z-index: 0 !important;
    .multiselect__single {
      color: #86868b;
      font-weight: 500;
      font-size: rem(17px);
      line-height: rem(21px);
      margin-bottom: 0;
      padding: 0;
    }
  }
  .multiselect__content-wrapper {
    z-index: 11 !important;
    border-bottom-left-radius: 13px;
    border-bottom-right-radius: 13px;
    box-shadow: 0px 0px 8px #0071e254;
    border: 1px solid #0071e2;
    border-top: 0;
    .multiselect__option {
      padding: rem(15px);
      font-size: rem(17px);
      line-height: rem(21px);
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      &--selected {
        background: none;
        color: #14121f;
        &::after {
          content: url(@/assets/img/check.svg);
          background: none;
          position: unset;
          line-height: unset;
          padding-right: 0;
        }
      }
      &--highlight {
        background: none;
        background: rgba(0, 0, 0, 0.05);
        color: #14121f;
        &::after {
          content: '';
        }
        &.multiselect__option--selected {
          &::after {
            content: url(@/assets/img/check.svg);
            background: none;
            padding-right: 0;
          }
        }
      }
    }
  }
  input {
    position: relative;
    width: 100%;
    text-align: left;
    outline: none;
    width: 100%;
    font-weight: 500;
    font-size: rem(17px);
    line-height: rem(21px);
    color: #14121f;
    padding: 0;
    margin: 0;
    &::placeholder {
      color: #86868b;
      font-weight: 500;
      font-size: rem(17px);
      line-height: rem(21px);
      margin-bottom: 0;
      padding: 0;
    }
  }
  &--active {
    .multiselect__tags {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
      border: 1px solid #0071e2;
      box-shadow: 0px 0px 8px #0071e254;
      border-bottom: 1px solid #86868b80;
    }
    .multiselect__select {
      &::before {
        transform: rotateX(180deg);
      }
    }
  }
}
</style>
