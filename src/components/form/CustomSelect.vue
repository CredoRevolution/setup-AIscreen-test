<template>
  <div class="custom-select" :tabindex="tabindex" @blur="open = false">
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
        <div class="item" :class="{ active: selected == option }">
          {{ option }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CustomSelect',
  methods: {
    returnInfo() {
      this.$emit('getInfo', this.selected)
    },
  },
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
    }
  },
  mounted() {
    this.$emit('getInfo', this.selected)
  },
}
</script>

<style lang="scss" scoped>
.custom-select {
  position: relative;
  width: 100%;
  text-align: left;
  outline: none;
  line-height: 47px;
  border-radius: 13px;
  width: 100%;
  font-weight: 500;
  font-size: 17px;
  line-height: 21px;
  color: #86868b;
  border: 1px solid transparent;
  &::after {
    content: '';
    width: 40px;
    height: 40px;
    position: absolute;
    top: 6px;
    right: 6px;
    background: #86868b29;
    border-radius: 10px;
    transition: all 0.3s ease;
  }
}
.main-screen__form-select {
  width: 100%;
  font-weight: 500;
  font-size: 17px;
  line-height: 21px;
  color: #86868b;
  &::placeholder {
    color: #86868b;
    font-weight: 500;
    font-size: 17px;
    line-height: 21px;
  }
}

.custom-select .selected {
  background-color: #fff;
  color: #86868b;
  cursor: pointer;
  user-select: none;
  border-radius: 13px;
  padding: 15px;
  border: 1px solid #86868b80;
}

.custom-select .selected.open {
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
  border: 1px solid #0071e2;
  padding: 15px;
  box-shadow: 0px 0px 8px #0071e254;
  border-bottom: 1px solid #86868b80;
  &:after {
    transform: rotateX(180deg);
  }
}

.custom-select .selected:after {
  position: absolute;
  content: '';
  top: 6px;
  right: 6px;
  width: 40px;
  height: 40px;
  background: url('@/assets/arrow-down.svg');
  background-position: center;
  background-repeat: no-repeat;
  transition: all 0.3s ease;
  border-radius: 10px;
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
  padding: 15px;
  cursor: pointer;
  user-select: none;
  &.active {
    color: #14121f;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    &:after {
      content: url(@/assets/check.svg);
    }
  }
}

.custom-select .items div:hover {
  color: #14121f;
}

.selectHide {
  display: none;
}
</style>
