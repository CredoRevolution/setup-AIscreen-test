<template>
  <div class="team-screen__member">
    <div class="team-screen__member-wrapper">
      <img
        src="@/assets/img/member-img.png"
        alt="avatar"
        class="team-screen__avatar"
      />
      <div class="team-screen__member-wrapper_horizontal">
        <p class="team-screen__member-name">
          {{ MemberData.name ? MemberData.name : 'John Doe' }}
        </p>
        <p class="team-screen__member-email">
          {{ MemberData.email ? MemberData.email : 'example@aiscreen.com' }}
        </p>
      </div>
    </div>
    <CustomSelect
      class="team-screen__select"
      :options="['Admin', 'User']"
      :default="'User'"
    />
    <div class="team-screen__member-date">
      {{ MemberData.date ? MemberData.date : '01.01.2020' }}
    </div>
    <div class="team-screen__member-actions">
      <div class="team-screen__member-actions-dropdown">
        <button class="team-screen__member-actions-btn" @click="showDropdown">
          <img src="@/assets/img/dots.svg" alt="dots" />
        </button>
        <div class="team-screen__member-actions-menu">
          <div
            class="team-screen__member-actions-menu-item"
            @click="$emit('deleteMember', MemberData.id), showDropdown(this)"
          >
            Delete
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CustomSelect from '@/components/form/CustomSelect.vue'
export default {
  name: 'TeamMember',
  components: {
    CustomSelect,
  },
  props: {
    MemberData: {
      type: Object,
      required: true,
    },
  },
  methods: {
    showDropdown(el) {
      if (el) {
        const menu = el.target.parentElement.parentElement.querySelector(
          '.team-screen__member-actions-menu'
        )

        menu.classList.toggle('active')
      }
    },
  },
}
</script>

<style lang="scss" scoped>
@function rem($px) {
  @return ($px / 16px) + rem;
}

.team-screen__member {
  display: flex;
  align-items: center;
  width: 100%;
  padding: rem(10px) rem(15px);
  border-radius: rem(13px);
  background-color: #fff;
  justify-content: space-between;
  &-wrapper {
    display: flex;
    flex-direction: row;
    gap: rem(17px);
    align-items: center;
    img {
      border-radius: 50%;
      width: rem(48px);
      height: rem(48px);
      object-fit: cover;
    }
    .team-screen__member-wrapper_horizontal {
      display: flex;
      flex-direction: column;
      gap: rem(5px);
      .team-screen__member-name {
        font-weight: 500;
        font-size: rem(24px);
        line-height: rem(29px);
        color: #14121f;
      }
      .team-screen__member-email {
        font-weight: 500;
        font-size: rem(17px);
        line-height: rem(21px);
        color: #86868b;
      }
    }
  }
  .team-screen__select {
    max-width: rem(240px);
  }
  &-date {
    font-weight: 700;
    font-size: rem(19px);
    line-height: rem(23px);
    color: #86868b;
  }
  &-actions {
    .team-screen__member-actions-dropdown {
      position: relative;
      .team-screen__member-actions-btn {
        background-color: transparent;
        border: 0;
        padding: 0;
        img {
          height: 100%;
        }
      }
      .team-screen__member-actions-menu {
        position: absolute;
        background-color: #fff;
        box-shadow: 0px 0px 8px #0071e254;
        border-radius: rem(13px);
        padding: rem(10px) 0;
        display: none;
        z-index: 1;
        &.active {
          display: block;
          right: 0;
          top: rem(45px);
        }
        .team-screen__member-actions-menu-item {
          font-weight: 500;
          font-size: rem(17px);
          line-height: rem(21px);
          color: #14121f;
          padding: rem(10px) rem(15px);
          cursor: pointer;
          &:hover {
            background-color: #f5f5f8;
          }
        }
      }
    }
  }
}
</style>
