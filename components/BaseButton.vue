<template>
  <component
    :class="['btn', `btn--${variant}`, buttonClass]"
    v-bind="$attrs"
    v-on="$listeners"
    :is="tag"
  >
    <slot>Найти</slot>
  </component>
</template>
<script>

export default {
  props: {
    variant: {
      type: String,
      default: 'default',
      validator: function (value) {
        return ['default', 'filter', 'primary', 'primary-large'].indexOf(value) !== -1;
      }
    },
    buttonClass: {
      type: String,
      default: ""
    },
    tag: {
      type: String,
      default: 'button'
    }
  }
}
</script>
<style lang="scss" scoped>
.btn {
  border-radius: 2px;
  font-size: 14px;
  line-height: 22px;
  border: none;
  outline: none;
  cursor: pointer;
  transition: .3s all;
  display: flex;
  justify-content: center;
  align-items: center;

  &--default {
    background: #FFCD33;
    color: #333333;
    min-width: 121px;
    height: 48px;
    padding: 16px 36px;

    &:hover {
      background: #ffcd33b3;
    }

    &:active {
      background: #FAB619;
    }
  }

  &--filter {
    border: 1px solid #9da3ac80;
    color: #9DA3AC;
    width: 86px;
    height: 32px;
    font-weight: 600;
    line-height: 19px;

    &:hover {
      border: 1px solid #71757A;
      color: #71757A;
    }

    &:active {
      background: #EEF0F5;
      border: 1px solid #71757A;
    }
  }

  &--primary,
  &--primary-large {
    border: 1px solid #007AFF;
    font-size: 14px;
    line-height: 19px;
    color: #007AFF;
    font-weight: 400;
    width: 115px;
    height: 30px;
    background: #fff;

    &:active {
      background: #007AFF;
      color: #fff;
    }
  }

  &--primary-large {
    width: 168px;
    height: 38px;
  }
}
</style>
