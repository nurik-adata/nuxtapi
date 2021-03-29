<template>
  <div class="text-block" :class="{magnifier: magnifier}">
    <div class="text-block__field">
      <input
        :type="inputType"
        class="text-block__input"
        ref="input"
        @input="inputHandler($event.target.value)"
        :value="value"
        :class="{ error: hasErrorFeedback }"
        required
      />
      <svg class="text-block__magnifier">
        <use
          :xlink:href="require('@/assets/icons/sprite.svg') + '#magnifier'"
        ></use>
      </svg>
      <svg
        class="text-block__trash"
        @click="removeField"
        v-if="trash && value.length == 0"
      >
        <use
          :xlink:href="require('@/assets/icons/sprite.svg') + '#trash'"
        ></use>
      </svg>
      <label for="" class="text-block__label">{{ labelText }}</label>
      <svg
        class="text-block__icon"
        @click="toggleShowPassword"
        v-if="passwordShowable && !showPassword"
      >
        <use
          :xlink:href="require('@/assets/icons/sprite.svg') + '#closedEye'"
        ></use>
      </svg>
      <svg
        class="text-block__icon"
        @click="toggleShowPassword"
        v-if="passwordShowable && showPassword"
      >
        <use
          :xlink:href="require('@/assets/icons/sprite.svg') + '#openedEye'"
        ></use>
      </svg>
      <svg
        class="text-block__icon"
        @click="$emit('input', '')"
        v-if="clearable && value && value.length > 0"
      >
        <use
          :xlink:href="require('@/assets/icons/sprite.svg') + '#clearIcon'"
        ></use>
      </svg>
    </div>
    <div class="text-block__info" v-if="showFeedback">
      <div class="create-info" :class="status">
        <div class="create-info__progress">
          <div class="create-info__indicator"></div>
        </div>
        <div class="create-info__text">
          {{ text }}
        </div>

      </div>
    </div>
    <div class="text-block__error" v-if="hasErrorFeedback">
      {{ errorText }}
    </div>
    <div class="text-block__options search-options" v-if="options.length > 0">
      <PerfectScrollbar class="search-options__wrapper">
        <div
          class="search-options__item"
          v-for="(option, index) in options"
          :key="index"
          @click="optionSelectHandler(option)"
        >
          {{ option }}
        </div>
      </PerfectScrollbar>
    </div>
  </div>
</template>
<script>
import { PerfectScrollbar } from "vue2-perfect-scrollbar";

export default {
  props: {
    magnifier: {
      type: Boolean,
      default: false
    },
    signup: {
      type: Boolean,
      default: false,
    },
    hasErrorFeedback: {
      type: Boolean,
      default: false,
    },
    errorText: {
      type: String,
    },
    labelText: {
      type: String,
      required: true,
    },
    type: {
      type: String,
      default: "text",
    },
    passwordShowable: {
      type: Boolean,
      default: false,
    },
    clearable: {
      type: Boolean,
      default: false,
    },
    trash:{
      type:Boolean,
      default:false
    },
    value: {
      type: String,
      required: true,
    },
    options: {
      type: Array,
      default: () => [],
    },
    inputFields:{
      type:Array,
    },
  },
  watch: {
    options: function (values) {
      this.options = values;
    },
  },
  data() {
    return {
      showPassword: false,
      text: "",
      status: "",
      showFeedback: false,
    };
  },
  computed: {
    inputType() {
      let inputType = this.type;

      if (this.passwordShowable) {
        inputType = "password";
      }

      return inputType;
    },
  },
  methods: {
    removeField(){
      if(this.inputFields.length > 2){
        this.inputFields.pop()
      }
    },
    toggleShowPassword() {
      if (!this.showPassword) this.showPasswordHandler("text", true);
      else this.showPasswordHandler("password", false);
    },
    showPasswordHandler(type, showPassword) {
      this.$refs.input.type = type;
      this.showPassword = showPassword;
    },
    inputHandler(value) {
      this.$emit("input", value);
      if (this.signup) this.passwordValidationHandler(value);
    },
    passwordValidationHandler(value) {
      if (!this.signup) return;

      this.showFeedback = true;

      if (!value) {
        this.status = "";
        this.showFeedback = false;
        return;
      }

      if (value.length < 5) {
        this.status = "danger";
        this.text = "Низкий уровень защиты";
      } else if (value.length >= 5 && value.length <= 10) {
        this.status = "warning";
        this.text = "Средний уровень защиты";
      } else {
        this.status = "success";
        this.text = "Высокий уровень защиты";
      }
    },
    optionSelectHandler(option) {
      this.$emit("input", option);
      this.$emit("chosen");
    },
  },
  components: {
    PerfectScrollbar,
  },
};
</script>
<style lang="scss">
.text-block {
  $self: &;
  position: relative;
  width: 100%;

  &.magnifier{

    #{$self}__magnifier{
      display: block;
    }

    #{$self}__input{
      padding: 19px 40px 5px 40px;
    }

    #{$self}__label{
      left: 40px;
    }
  }

  &__field {
    position: relative;
    overflow: hidden;
  }

  &__input {
    background: #ffffff;
    border: 1px solid #c4c4c4;
    box-sizing: border-box;
    border-radius: 2px;
    width: 100%;
    height: 48px;
    font-size: 14px;
    line-height: 22px;
    display: flex;
    align-items: center;
    color: #2c3e50;
    padding: 19px 40px 5px 16px;
    transition: 0.3s all;

    &:focus ~ #{$self}__label,
    &:not(:focus):valid ~ #{$self}__label {
      top: 15px;
      font-size: 10px;
      color: #71757a;
    }

    &:focus {
      border: 1px solid #2c3e50;
    }

    &.error {
      background: #ff2e431f;
    }
  }

  &__label {
    position: absolute;
    pointer-events: none;
    left: 16px;
    top: 50%;
    transform: translateY(-50%);
    white-space: nowrap;
    overflow: hidden;
    line-height: 40px;
    transition: 0.3s;
    color: #c4c4c4;
  }

  &__icon {
    position: absolute;
    right: 16px;
    top: 50%;
    transform: translateY(-50%);
    width: 16px;
    height: 16px;
    cursor: pointer;
  }

  &__magnifier{
    position: absolute;
    left: 18px;
    top: 50%;
    transform: translateY(-50%);
    width: 16px;
    height: 16px;
    cursor: pointer;
    display: none;
  }

  &__trash{
    position: absolute;
    right: 16px;
    top: 50%;
    transform: translateY(-50%);
    width: 16px;
    height: 16px;
    cursor: pointer;
  }

  &__error {
    font-size: 10px;
    line-height: 14px;
    color: #ff2e43;
    margin-top: 6px;
  }

  &__options {
    position: absolute;
    top: calc(100% + 4px);
    left: 0;
    width: 100%;
    background: #fff;
    z-index: 1000;
    border-radius: 2px;
    border: 1px solid #c4c4c480;
    padding: 12px 0;
  }
}

.search-options {
  &__wrapper {
    max-height: 238px;
  }

  &__item {
    padding: 5px 16px;
    line-height: 16px;
    font-size: 14px;
    color: #9da3ac;
    transition: 0.3s all;
    cursor: pointer;

    &:hover {
      background: #eef0f5;
    }
  }
}

.create-info {
  $self: &;

  margin-top: 9px;
  margin-bottom: 12px;

  &.danger {
    #{$self}__indicator {
      background: #ff2e43;
      width: 33.333%;
    }

    #{$self}__text {
      color: #ff2e43;
    }
  }

  &.warning {
    #{$self}__indicator {
      background: #ffcd33;
      width: 66.666%;
    }

    #{$self}__text {
      color: #fab619;
    }
  }

  &.success {
    #{$self}__indicator {
      background: #00b92d;
      width: 100%;
    }

    #{$self}__text {
      color: #00b92d;
    }
  }

  &__progress {
    height: 1px;
    background: #c4c4c4;
    position: relative;
  }

  &__indicator {
    position: absolute;
    top: 33.333%;
    transform: translateY(-50%);
    height: 2px;
    border-radius: 10px;
  }

  &__text {
    font-style: normal;
    font-weight: normal;
    font-size: 10px;
    line-height: 14px;
    display: flex;
    align-items: center;
    margin-top: 4px;
  }
}

.ps {
  &__thumb-y {
    width: 2px;
  }
}
</style>
