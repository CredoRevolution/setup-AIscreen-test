<template>
  <div
    class="main-screen main-screen_screens main-screen_team""
  >
    <div class="team-invite-modal" v-if="showModal">
      
      <div class="modal-wrapper">
        <div class="modal-close">
      <img
        src="@/assets/img/exit.svg"
        alt="exit"
        class="main-screen__exit-img"
        @click="showModal = false"
      />
    </div>
    <div class="logo-adaptation">
      <img
        v-if="require(`@/assets/img/logo.svg`)"
        :src="require(`@/assets/img/logo.svg`)"
        alt="aiscreen"
        class="main-screen-main__logo"
      />
    </div>
        <h3 class="team-invite-modal__title">Find the people blablabla</h3>
      <p class="team-invite-modal__text">
        Show your content on screen or in demo mode in 60 seconds. Get started
        quickly and easily.
      </p>
      <input
        type="text"
        name="name"
        class="team-screen__invite-input"
        v-model="MemberData.name"
        @keyup.enter="inviteMember"
        placeholder="Name"
        @input="resetValidation"
        :class="{
          error: $v.MemberData.name.$error && showError,
          valid: !$v.MemberData.name.$error && $v.MemberData.name.$model
        }"
      />
      <p
        v-if="showError && !$v.MemberData.name.required"
        class="error-message"
      >
        Please Fill In This Field
      </p>
      
      <input
        type="text"
        name="email"
        class="team-screen__invite-input "
        v-model="MemberData.email"
        @keyup.enter="inviteMember"
        placeholder="Email"
        @input="resetValidation"
        :class="{
          error: $v.MemberData.email.$error && showError,
          valid: !$v.MemberData.email.$error && $v.MemberData.email.$model
        }"
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
      <a
        href="#"
        class="main-screen__btn hover-btn blue-btn"
        @click="inviteMember"
        >Invite</a
      >
      <div class="prev-btn" @click="showModal = false">Cancel</div>
      </div>
    </div>
    <div class="logo-adaptation">
      <img
        v-if="require(`@/assets/img/logo.svg`)"
        :src="require(`@/assets/img/logo.svg`)"
        alt="aiscreen"
        class="main-screen-main__logo"
      />
    </div>
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
      <div class="invite-input-wrapper">
        <input
        type="text"
        name="name"
        class="team-screen__invite-input "
        v-model="MemberData.name"
        @keyup.enter="inviteMember"
        placeholder="Name"
        @input="resetValidation"
        :class="{
          error: $v.MemberData.email.$error && showError,
          valid: !$v.MemberData.email.$error && $v.MemberData.email.$model
        }"
      />
      <p
        v-if="showError && !$v.MemberData.name.required"
        class="error-message"
      >
        Please Fill In This Field
      </p>
      </div>

      <div class="invite-input-wrapper">
      <input
        type="text"
        class="team-screen__invite-input"
        v-model="MemberData.email"
        @keyup.enter="inviteMember"
        placeholder="example@company.com"
        @input="resetValidation"
        :class="{
          error: $v.MemberData.email.$error && showError,
          valid: !$v.MemberData.email.$error && $v.MemberData.email.$model
        }"
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
      </div>
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
    <a href="#" class="main-screen__btn hover-btn blue-btn" @click="nextScreen"
      >Finish</a
    >
    <button
      class="main-screen__form-btn main-screen__form-btn_additional"
      @click.prevent="showModal = true"
    >
      Invite people
    </button>
  </div>
</template>

<script>
import TeamMember from '@/components/table/TableItem.vue'
import { required, email } from 'vuelidate/lib/validators'
import CustomInput from '../form/CustomInput.vue'
export default {
  name: 'TeamScreen',
  data() {
    return {
      MemberData: {
        name: '',
        email: '',
      },
      showModal: false,
      showError: false,
      TeamMembers: [],
    }
  },
  components: {
    TeamMember,
    CustomInput,
  },
  validations: {
    MemberData: {
      email: {
        required,
        email,
      },
      name: {
        required,
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
      this.showModal = false
      this.showError = false
      if (this.MemberData.email) {
        const currentDate = new Date()
        const options = { year: 'numeric', month: 'long', day: 'numeric', hour: 'numeric', minute: 'numeric', second: 'numeric' }
        const formatedDate = currentDate.toLocaleDateString('en-US', options)
        this.TeamMembers.push({ ...this.MemberData, date: formatedDate, name: this.MemberData.name })
        this.MemberData.name = ''
        this.MemberData.email = ''
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
  &.blur {
    filter: brightness(50%);
    transition: all 0.3s ease;
  }
  position: relative;
  .main-screen__form-btn_additional {
    display: none;
  }
  .team-screen__invite {
    margin-top: rem(22px);
    position: relative;
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    gap: rem(17px);

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
      &:focus {
      border: 1px solid #0071e2 !important;
      color: #14121f;
      outline: none;
      box-shadow: 0px 0px 8px #0071e254;
      transition: all 0.3s ease;
      
    }
    &.valid {
      border: 1px solid #0071e2;
      transition: all 0.3s ease;
    }

      &::placeholder {
        color: #86868b80;
        font-weight: 500;
        font-size: rem(17px);
        line-height: rem(21px);
      }
    }

    &-btn {
      height: 100%;
      font-size: rem(19px)!important;
      line-height: rem(23px)!important;
      font-weight: 700;
      border-radius: rem(999px);
      padding: rem(15px) rem(56px) !important;
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
  .team-invite-modal {
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    position: absolute;
    background: #14121f9c;
    z-index: 11;
    .modal-close{
      position: absolute;
      background: none;
      top: rem(35px);
      right: rem(35px);
      border-radius: 0;
      width: rem(15px);
      height: rem(15px);
      img {
        padding: 0;
      }
    }
    .team-screen__invite-input.error{
      margin-bottom: rem(-17px) !important;
    }
    .modal-wrapper{
      position: absolute;
    width: 90%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 12;
    background: #fff;
    border-radius: rem(30px);
    padding: rem(31px) rem(21px);
    display: flex;
    flex-direction: column;
    gap: rem(17px);
    .logo-adaptation{
      margin-bottom: rem(13px);
    }
    .team-screen__invite-input{
      display: block  !important;
    }
      .team-invite-modal__title {
      font-size: rem(28px);
      line-height: rem(34px);
      font-weight: 700;
      color: #14121f;
    }
    .team-invite-modal__text {
      font-size: rem(17px);
      line-height: rem(21px);
      font-weight: 700;
      color: rgba(134, 134, 139, 1);
      margin-bottom: rem(7px);
    }

    .main-screen__btn {
      margin-top: rem(14px);
    }
    .prev-btn {
      color: rgba(134, 134, 139, 1);
      font-size: rem(18px);
      line-height: rem(22px);
      font-weight: 500;
      transition: opacity 0.3s ease;
      background: none;
      text-decoration: none;
      margin: 0 auto;
      text-align: center;
      &:hover {
        opacity: 0.8;
        transition: opacity 0.3s ease;
      }
    }
    }
    
  }
}

@media (max-height: 800px) {
  .main-screen.main-screen_team{
    .select-wrapper .multiselect .multiselect__content-wrapper{
      max-height: 200px!important;
    }
  }
}

@media (max-width: 768px) {
  .main-screen.main-screen_team .main-screen__exit {
    background: none;
    top: rem(23px);
    right: rem(23px);
    border-radius: 0;
    width: rem(15px);
    height: rem(15px);
    img {
      padding: 0;
    }
  }
  .main-screen_team {
    justify-content: flex-start !important;
    .team-screen__invite {
      margin-top: 0;
      display: none;
    }
    .main-screen__form-btn_additional {
      display: block;
    }
    .team-screen__invite-input,
    .team-screen__invite-btn {
      display: none;
    }
    .main-screen__btn {
      width: 100% !important;
      padding-left: 0 !important;
      padding-right: 0 !important;
      justify-content: center;
    }
    .main-screen__form-btn_additional {
      font-weight: 700;
      font-size: rem(19px);
      line-height: rem(23px);
      padding-top: rem(12px)!important;
      padding-bottom: rem(12px)!important;
      letter-spacing: 0.02em;
      margin-top: rem(7px);
      color: rgba(20, 18, 31, 1);
      border: 2px solid rgba(20, 18, 31, 1)!important;
      background-color: #fff!important;
      border-radius: rem(999px);
      transition: all 0.3s ease;
      margin-bottom: rem(34px);
      &:hover {
        background-color: rgba(20, 18, 31, 1)!important;
        color: #fff;
        transition: all 0.3s ease;
      }
    }
  }
  .main-screen.main-screen_team .team-screen__list {
    max-height: unset !important;
    margin-top: 0 !important;
    height: 100%;
  }
}
.invite-input-wrapper {
  width: 100%;
  display: flex;
  flex-direction: column;
}
</style>
