<template>
  <div class="main-screen main-screen_screens main-screen_team">
    <div class="main-screen__exit">
      <img
        src="@/assets/img/exit.svg"
        alt="exit"
        class="main-screen__exit-img"
        @click="closeScreen"
      />
    </div>
    <img src="@/assets/img/logo.svg" alt="aiscreen" class="main-screen__logo" />
    <h2 class="main-screen__title main-screen__title_team">Invite your team</h2>
    <p class="main-screen__text">
      Show your content on screen or in demo mode in 60 seconds. Get started
      quickly and easily.
    </p>
    <div class="team-screen__invite">
      <input
        type="text"
        class="team-screen__invite-input"
        v-model="MemberData.email"
        @keyup.enter="inviteMember"
        placeholder="example@company.com"
        @input="resetValidation"
        :class="{ error: $v.MemberData.email.$error && showError }"
      />
      <p v-if="showError && !$v.MemberData.email.email" class="error-message">
        Please enter a valid email
      </p>
      <p
        v-if="showError && !$v.MemberData.email.required"
        class="error-message"
      >
        Please Fill In This Field
      </p>
      <button
        class="team-screen__invite-btn hover-btn blue-btn"
        @click.prevent="inviteMember"
      >
        Invite
      </button>
    </div>
    <ul class="team-screen__list">
      <TeamMember
        v-for="(TeamMember, index) in TeamMembers"
        :key="index"
        :MemberData="TeamMember"
        @deleteMember="deleteMember(index)"
      />
    </ul>
    <div class="main-screen__btn-wrapper">
      <a
        href="#"
        class="main-screen__btn hover-btn blue-btn"
        @click="prevScreen"
        >Prev</a
      ><a
        href="#"
        class="main-screen__btn hover-btn blue-btn"
        @click="nextScreen"
        >Finish</a
      >
    </div>
  </div>
</template>

<script>
import TeamMember from '@/components/table/TableItem.vue'
import { required, email } from 'vuelidate/lib/validators'
export default {
  name: 'TeamScreen',
  data() {
    return {
      MemberData: {
        email: '',
      },
      showError: false,
      TeamMembers: [],
    }
  },
  components: {
    TeamMember,
  },
  validations: {
    MemberData: {
      email: {
        required,
        email,
      },
    },
  },
  methods: {
    inviteMember() {
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.showError = true
        return false
      }

      this.showError = false
      if (this.MemberData.email) {
        const currentDate = new Date()
        const dateOptions = {
          year: 'numeric',
          month: 'long',
          day: 'numeric',
          hour: 'numeric',
          minute: 'numeric',
          second: 'numeric',
        }
        const formatedDate = currentDate.toLocaleDateString(
          'en-US',
          dateOptions
        )
        this.TeamMembers.push({ ...this.MemberData, date: formatedDate })
        this.MemberData.email = ''
        console.log(this.TeamMembers)
      }
    },
    deleteMember(index) {
      this.TeamMembers.splice(index, 1)
      const allMenus = document.querySelectorAll(
        '.team-screen__member-actions-menu'
      )
      allMenus.forEach((menu) => {
        menu.classList.remove('active')
      })
    },
    nextScreen() {
      this.$emit('nextScreen')
    },
    prevScreen() {
      this.$emit('prevScreen')
    },
    closeScreen() {
      this.$emit('closeScreen')
    },
    resetValidation() {
      this.$v.$reset()
      this.showError = false
    },
  },
  mounted() {
    this.resetValidation()
  },
}
</script>

<style lang="scss">
@function rem($px) {
  @return ($px / 16px) + rem;
}

.main-screen_team {
  .team-screen__invite {
    margin-top: rem(22px);
    position: relative;

    &-input {
      width: 100%;
      border-radius: rem(13px);
      border: 1px solid #86868b80;
      padding: 15px;
      font-weight: 500;
      font-size: rem(17px);
      line-height: rem(21px);
      color: #14121f;

      &.error {
        border: 1px solid red;
      }

      &::placeholder {
        color: #86868b80;
        font-weight: 500;
        font-size: rem(17px);
        line-height: rem(21px);
      }
    }

    &-btn {
      position: absolute;
      right: rem(6px);
      top: rem(6px);
      font-size: rem(17px);
      line-height: rem(21px);
      font-weight: 500;
      border-radius: rem(10px);
      padding: rem(10px) rem(19px);
      &::before {
        border-radius: rem(10px);
      }

      &:hover {
        &::before {
          border-radius: rem(10px);
        }
      }
    }
  }

  .team-screen__list {
    position: relative;
    overflow-y: scroll;
    display: flex;
    flex-direction: column;
    align-items: center;
    background: #f5f5f8;
    border-radius: rem(30px);
    height: rem(392px);
    margin-bottom: rem(32px);
    margin-top: rem(31px);
    gap: rem(17px);
    padding: rem(17px);
    padding-bottom: 0;
    &::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: rem(17px);
      background: linear-gradient(
        to bottom,
        rgba(245, 245, 248, 1),
        rgba(245, 245, 248, 0)
      );
    }
    &::after {
      content: '';
      position: sticky;
      bottom: 0;
      left: 0;
      right: 0;
      height: rem(17px);
      width: 100%;
      background: linear-gradient(
        to top,
        rgba(245, 245, 248, 1),
        rgba(245, 245, 248, 0)
      );
    }
    &::-webkit-scrollbar {
      width: 8px;
      left: -15px;

      height: 60%;
    }
    &::-webkit-scrollbar-track {
      background: transparent;
      height: 60%;
    }
    &::-webkit-scrollbar-thumb {
      background-color: rgba(235, 235, 236, 0.8);
      border-radius: rem(13px);
      border: 1px solid transparent;
      background-clip: content-box;
    }
    &::-webkit-scrollbar-thumb:hover {
      background: rgba(235, 235, 236, 1);
    }
  }
}

@media (max-width: 1050px) {
}
</style>
