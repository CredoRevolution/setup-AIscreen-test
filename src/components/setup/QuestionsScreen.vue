<template>
  <div class="main-screen main-screen_screens questions-screen">
    <div class="questions-screen__logo">
      <img
        v-if="require(`@/assets/img/logo.svg`)"
        :src="require(`@/assets/img/logo.svg`)"
        alt="aiscreen"
        class="questions-screen__logo-img"
      />
    </div>
    <div v-if="step" class="questions-screen__step">Step {{ step }}/2</div>

    <div v-if="title" class="questions-screen__title">
      {{ title }}
    </div>
    <div class="questions-screen__text">
      Select to help us to curate content for you
    </div>
    <ul class="questions-screen__list">
      <li
        class="questions-screen__list-item"
        v-for="(answer, index) in answers"
        :key="index"
        @click="makeActive($event)"
        ref="answer"
      >
        {{ answer }}
      </li>
    </ul>
    <a
      href="#"
      class="main-screen__btn hover-btn blue-btn"
      :class="{ grey: !valid }"
      @click="nextScreen"
      ref="nextBtn"
      >Next</a
    >
  </div>
</template>

<script>
export default {
  name: 'QuestionsScreen',
  data() {
    return {
      activeAnswers: [],
      qustionsAnswers: {},
      valid: false,
    }
  },
  methods: {
    nextScreen() {
      if (this.checkValidation()) {
        this.$emit('nextScreen', this.qustionsAnswers)
        this.$refs.answer.forEach((element) => {
          element.classList.remove('active')
        })
        this.checkValidation()
      }
    },
    makeActive(el) {
      if (el) {
        el.target.classList.toggle('active')
      }
      this.checkValidation()
    },
    checkValidation() {
      this.activeAnswers = []
      this.qustionsAnswers = {}
      this.$refs.answer.forEach((element) => {
        if (element.classList.contains('active')) {
          this.activeAnswers.push(element.textContent)
        }
      })
      this.qustionsAnswers[this.title] = this.activeAnswers
      if (this.activeAnswers.length > 0) {
        this.valid = true
        return true
      } else {
        this.valid = false
        return false
      }
    },
  },
  props: {
    title: String,
    step: Number,
    answers: Array,
  },
  mounted() {
    this.checkValidation()
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}

.main-screen.questions-screen {
  display: flex;
  flex-direction: column;
  padding: rem(40px) rem(80px);
  height: 83.4vh !important;
  .questions-screen__logo {
    display: flex;
    align-items: center;
    margin-bottom: rem(38px);
  }
  .questions-screen__step {
    font-size: rem(17px);
    line-height: rem(29px);
    margin-bottom: rem(17px);
    font-weight: 500;
    color: rgba(134, 134, 139, 1);
  }
  .questions-screen__title {
    font-weight: 700;
    font-size: rem(40px);
    line-height: rem(48px);
    margin-bottom: rem(17px);
  }
  .questions-screen__text {
    font-size: rem(17px);
    line-height: rem(21px);
    margin-bottom: rem(55px);
    font-weight: 700;
    color: #86868b;
  }
  .questions-screen__list {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: rem(17px);
    list-style: none;
    .questions-screen__list-item {
      cursor: pointer;
      font-size: rem(24px);
      line-height: rem(29px);
      font-weight: 700;
      padding: rem(17px) rem(43px);
      color: rgba(134, 134, 139, 1);
      border: 1px solid rgba(134, 134, 139, 0.16);
      border-radius: rem(999px);
      transition: all 0.2 ease;
      &.active {
        transition: all 0.2 ease;
        border: 1px solid rgba(0, 113, 226, 1);
        color: rgba(20, 18, 31, 1);
      }
    }
  }
  .main-screen__btn {
    margin-top: auto;
  }
}

@media (max-width: 1500px) {
  .main-screen.questions-screen {
    .questions-screen__step {
      font-size: rem(17px);
      line-height: rem(29px);
      margin-bottom: rem(17px);
    }
    .questions-screen__title {
      font-size: rem(32px);
      line-height: rem(37px);
      margin-bottom: rem(17px);
    }
    .questions-screen__text {
      font-size: rem(17px);
      line-height: rem(21px);
      margin-bottom: rem(31px);
    }
    .questions-screen__list {
      margin-bottom: rem(31px);
      .questions-screen__list-item {
        font-size: rem(19px);
        line-height: rem(24px);
        padding: rem(15px) rem(25px);
      }
    }
  }
}

@media (max-width: 1200px) {
  .main-screen.questions-screen {
    overflow-y: hidden;
    .questions-screen__logo {
      margin-bottom: rem(33px);
    }
    .questions-screen__step {
      font-size: rem(17px);
      line-height: rem(29px);
      margin-bottom: rem(17px);
    }
    .questions-screen__title {
      font-size: rem(28px);
      line-height: rem(33px);
      margin-bottom: rem(17px);
    }
    .questions-screen__text {
      font-size: rem(17px);
      line-height: rem(21px);
      margin-bottom: rem(31px);
    }
    .questions-screen__list {
      margin-bottom: rem(31px);
      .questions-screen__list-item {
        font-size: rem(17px);
        line-height: rem(21px);
        padding: rem(15px) rem(20px);
      }
    }
  }
}

@media (max-width: 768px) {
  .main-screen.questions-screen {
    overflow-y: scroll;
    padding: rem(12px) rem(22px);
    height: 100% !important;
  }
}

@media (max-width: 576px) {
  .main-screen.questions-screen .main-screen__btn {
    width: 100% !important;
    justify-content: center !important;
    padding: rem(14px) 0 !important;
  }
}
</style>
