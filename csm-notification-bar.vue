<template>
  <div
          class='notification-bar'
  >
    <transition-group name="notifications">
      <div class="notification-bar__content"
           v-for="(message, index) in messages"
           :class="message.type"
           :key="message.id"
      >
        <div class="content__text-content align-baseline">
          <span>
  {{message.name}}
</span>
          <i
                  v-if="!leftBtnText && !rightBtnText"
                  class="material-icons"
                  :class="message.type"
          >
          </i>
        </div>
      </div>
    </transition-group>
  </div>
</template>

<script>
  export default {
    name: "notification-bar",
    props: {
      notificationType: {
        type: String,
        default: ''
      },
      permanent: {
        type: Boolean,
        default() {
          return false;
        }
      },
      timeOut: {
        type: Number,
        default: 3000
      },
      messages: {
        type: Array,
        default() {
          return []
        }
      },
      leftBtnText: {
        type: String,
        default: ''
      },
      rightBtnText: {
        type: String,
        default: ''
      }

    },
    computed: {
      className() {
        return {
          'success': this.notificationType === 'success',
          'error': this.notificationType === 'error',
          'warning': this.notificationType === 'warning',
        }
      },
    },
    methods: {
      leftBtnClick() {
        this.$emit('leftBtnClick');
      },
      rightBtnClick(index) {
        this.messages.splice(index, 1);
        this.$emit('rightBtnClick');
      },
      removeNotificationMsg() {
        if (this.messages.length) {
          let vm = this;
          setTimeout(function () {
            vm.messages.splice(vm.messages.length - 1, 1);
          }, vm.timeOut)
        }
      }
    },
    watch: {
      messages() {
        this.removeNotificationMsg();
      }
    },
    mounted() {
      this.removeNotificationMsg();
    }
  }
</script>

<style lang="scss">
  .notification-bar {
    position: fixed;
    top: 16px;
    right: 16px;
    z-index: 10;
  }

  .notification-bar__content {
    border: solid 1px transparent;
    border-radius: 4px;
    height: 50px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding: $padding*2;
    margin-bottom: 16px;
    color: #ffffff;
    box-shadow: 0 11px 15px -7px rgba(0, 0, 0, .2), 0 24px 38px 3px rgba(0, 0, 0, .14), 0 9px 46px 8px rgba(0, 0, 0, .12);

    &.align-baseline {
      align-items: baseline;
    }

    &.check_circle {
      background-color: green;
    }

    &.error {
      background-color: red;
    }

    &.warning {
      background-color: orange;
    }

    & .material-icons {
      font-size: 14px;
      margin-left: 32px;
      border-radius: 50%;
      padding: 2px;
    }
  }

  /*notification animation*/
  .notifications {
    &-enter {
      transform: translateX(120px);
      opacity: 0;
    }

    &-enter-active {
      transition: all .6s ease;
    }

    &-enter-to {
      opacity: 1;
    }

    &-leave {
      height: 50px;
      opacity: 1;
    }

    &-leave-active {
      transition: transform .6s ease,
      opacity .6s,
      height .6s .2s;
    }

    &-leave-to {
      height: 0;
      transform: translateX(120px);
      opacity: 0;

      &:first-child {
        height: 50px;
      }
    }

    &-move {
      transition: transform .6s ease;
    }
  }
</style>
