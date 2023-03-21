<template>
  <div class="ui-input-block">
    <div
        v-if="afterText"
        class="ui-input-block__after-text"
    >
      {{ afterText }}
    </div>
    <div class="ui-input-block__field">
      <label
          class="ui-input-block__label"
          :class="{ '_focus': isFocus || modelValue }"
      >
        {{ label }}
      </label>
      <UiSpinner
          :is-loading="isLoading"
          :size="18"
          color="#00B6D0"
          :thickness="2"
          class="ui-input-block__spinner"
      />
      <textarea
          ref="inputRef"
          :value="modelValue"
          :disabled="disabled"
          class="ui-input-block__input"
          :class="{ '_error': !rules(modelValue) }"
          :style="{ minHeight: `${minHeight}px` }"
          @input="$emit('update:modelValue', $event.target.value)"
          @focus="onFocus"
          @blur="onBlur"
      ></textarea>
    </div>
    <div class="ui-input-block__system">
      <span
          v-if="!rules(modelValue)"
          class="ui-input-block__system_error"
      >
        Ошибка
      </span>
      <span>
        {{ modelValue.length }}/{{ maxLength }}
      </span>
      <span
          v-if="modelValue.length"
          class="ui-input-block__system_clear"
          @click="clear"
      >
        Очистить
      </span>
    </div>
  </div>
</template>

<script>
import UiSpinner from '@/components/UiSpinner'

export default {
  name: 'UiInput',
  components: {
    UiSpinner
  },
  props: {
    modelValue: {
      type: String,
      default: ''
    },
    label: {
      type: String,
      default: ''
    },
    afterText: {
      type: String,
      default: ''
    },
    disabled: {
      type: Boolean,
      default: false
    },
    rules: {
      type: Function,
      default: () => (true)
    },
    maxLength: {
      type: Number,
      default: null
    },
    isLoading: {
      type: Boolean,
      default: false
    },
    autogrow: {
      type: Boolean,
      default: false
    },
    minHeight: {
      type: Number,
      default: 146
    }
  },
  emits: ['onFocus', 'onBlur', 'update:modelValue'],
  data() {
    return {
      isFocus: false
    }
  },
  watch: {
    modelValue() {
      // после обновления DOM вызываем метод обновления высоты инпута
      this.autogrow && this.$nextTick(this.adjustHeight)
    }
  },
  methods: {
    onFocus(event) {
      this.isFocus = true
      this.$emit('onFocus', event)
    },
    onBlur(event) {
      this.isFocus = false
      this.$emit('blur', event)
    },
    clear() {
      this.$emit('update:modelValue', '')
    },
    adjustHeight() {
      const inp = this.$refs.inputRef
      if (inp !== null) {
        const parentStyle = inp.parentNode.style
        // chrome does not keep scroll #15498
        const { scrollTop } = inp
        // chrome calculates a smaller scrollHeight when in a .column container
        const { overflowY, maxHeight } = window.getComputedStyle(inp)
        // on firefox or if overflowY is specified as scroll #14263, #14344
        // we don't touch overflow
        // firefox is not so bad in the end
        const changeOverflow = overflowY !== void 0 && overflowY !== 'scroll'

        // reset height of textarea to a small size to detect the real height
        // but keep the total control size the same
        changeOverflow === true && (inp.style.overflowY = 'hidden')
        inp.style.height = '1px'

        inp.style.height = inp.scrollHeight + 'px'
        // we should allow scrollbars only
        // if there is maxHeight and content is taller than maxHeight
        changeOverflow === true && (inp.style.overflowY = parseInt(maxHeight, 10) < inp.scrollHeight ? 'auto' : 'hidden')
        inp.scrollTop = scrollTop
      }
    }
  }
}
</script>

<style lang="scss" scoped>
$font-family: 'Roboto', sans-serif;

$control-color: #878F97;
$text-color: #4F4F4F;
$error-color: #D6675C;
$clear-color: #00B6D0;
$input-color-bg: #F0F0F0;
$error-color-bg: #FBF0EF;


.ui-input-block {
  letter-spacing: 0.0015em;
  font-weight: 400;
  width: 100%;

  &__after-text {
    text-align: left;
    font-weight: 500;
    font-size: 18px;
    line-height: 24px;
    margin-bottom: 4px;
  }

  &__field {
    position: relative;
  }

  &__label {
    position: absolute;
    left: 12px;
    margin-top: 4px;
    transition: all .15s ease-in-out;
    display: inline-block;
    color: $control-color;
    font-size: 16px;
    line-height: 22px;
    transform-origin: left top;

    &._focus {
      transform: scale(.69);
      transition: transform ease-in .2s;
    }
  }

  &__spinner {
    position: absolute;
    right: 6px;
    margin-top: 6px;
  }

  &__input {
    resize: none;
    display: flex;
    align-items: center;
    font-family: $font-family;
    color: $text-color;
    line-height: 22px;
    font-size: 16px;
    letter-spacing: 0.0015em;
    width: 100%;
    height: 100%;
    min-height: 146px;
    padding: 20px 32px 4px 12px;
    background: $input-color-bg;
    background-clip: padding-box;
    border-radius: 8px;
    border: none;
    outline: none;
    margin-bottom: 8px;
    transition: background-color ease-in .2s;
    box-sizing: border-box;

    &._error {
      background: $error-color-bg;
    }

    &:-moz-placeholder {
      opacity: 0;
    }

    &::-webkit-input-placeholder {
      opacity: 0;
    }

    &::-moz-placeholder {
      opacity: 0;
    }

    &:disabled {
      color: rgba($text-color, 0.5);
    }
  }

  &__system {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    font-size: 14px;
    line-height: 20px;
    letter-spacing: 0.0025em;
    color: $control-color;

    &_error {
      color: $error-color;
    }

    &_clear {
      color: $clear-color;
      cursor: pointer;
    }
  }
}
</style>
