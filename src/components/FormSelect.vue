<!-- eslint-disable prettier/prettier -->
<template>
  <div class="form-select">
    <select
      v-model="selectedOptions"
      class="form-select__select"
      :multiple="multiple"
    >
      <option
        class="form-select__option"
        disabled
        value=""
      >
        {{ `Выберите вариант ${multiple ? 'или варианты' : ''}` }}
      </option>
      <option
        v-for="(option, i) in options"
        :key="i"
        class="form-select__option"
        :value="typeof option === 'string' ? option : option.value"
      >
        {{ typeof option === 'string' ? option : option.text }}
      </option>
    </select>
    <VIcon
      :href="'expand-more'"
      class="form-select__icon"
    />
  </div>
</template>
<!-- eslint-enable -->

<script>
  export default {
    props: {
      inputData: {
        type: Object,
        required: false,
        default: () => {},
      },
      options: {
        type: Array,
        required: false,
        default: () => [],
      },
      selectedValueData: {
        type: Object,
        required: false,
        default: () => {},
      },
      selectedValueDataKey: {
        type: String,
        required: false,
        default: '',
      },
      multiple: {
        type: Boolean,
        required: false,
        default: false,
      },
    },
    computed: {
      selectedOptions: {
        get() {
          return this.inputData.value;
        },
        set(value) {
          this.$evBus.$emit('changed-value-or-values', {
            dataKey: this.inputData.idAndDataKey,
            newValue: value,
          });
        },
      },
    },
  };
</script>

<style lang="scss">
  .form-select {
    position: relative;
    display: flex;
    align-items: center;
    width: 100%;

    &__select {
      display: block;
      flex: 1 1 0;
      padding: 15px;
      border: 1px solid $border-grey;
      border-radius: 5px;
      transition: border-color 0.3s, color 0.3s;

      &:hover,
      &:focus {
        border-color: $border-black;
        transition: border-color 0.3s, color 0.3s;
      }

      @at-root .validation-wrapper.error & {
        color: $text-red;
        border-color: $border-red;
        transition: color 0.3s, border-color 0.3s;
      }
    }

    &__icon {
      position: absolute;
      top: 50%;
      right: 10px;
      z-index: -1;
      width: 25px;
      height: 25px;
      transform: translateY(-50%);

      @at-root .form-select__select[multiple] + & {
        @media (min-width: $break-lg) {
          display: none;
        }
      }
    }
  }
</style>
