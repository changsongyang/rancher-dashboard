<script>
export default {
  props: {
    // The name of the input, for grouping
    name: {
      type:    String,
      default: ''
    },

    // The value for this option
    val: {
      required:  true,
      validator: x => true,
    },

    // The selected value...
    value: {
      required:  true,
      validator: x => true,
    },

    // The label shown next to the radio
    label: {
      type:    String,
      default: ''
    },

    disabled: {
      type:    Boolean,
      default: false
    },

    mode: {
      type:    String,
      default: 'edit',
    }
  },

  data() {
    return { isChecked: this.value === this.val };
  },

  watch: {
    value(neu) {
      this.isChecked = this.val === neu;
      if ( this.isChecked ) {
        this.$refs.custom.focus();
      }
    }
  },

  methods: {
    clicked() {
      if (!this.disabled) {
        this.$emit('input', this.val);
      }
    },
  }
};
</script>

<template>
  <span v-if="mode === 'view'" class="radio-view">
    <div class="text-label">
      <slot name="label">
        {{ label }}
      </slot>
    </div>
    <span>{{ val }}</span>
  </span>
  <label
    v-else
    class="radio-container"
    @keydown.enter.prevent="clicked($event)"
    @keydown.space.prevent="clicked($event)"
    @click.stop.prevent="clicked($event)"
  >
    <input
      :disabled="disabled"
      :name="name"
      :value="''+val"
      :checked="isChecked"
      type="radio"
      :tabindex="-1"
      @click.stop.prevent
    />
    <span
      ref="custom"
      class="radio-custom"
      :tabindex="disabled ? -1 : 0"
      :aria-label="label"
      :aria-checked="isChecked"
      role="radio"
    />
    <span
      v-if="label"
      class="radio-label"
      v-html="label"
    >
      <slot name="label">{{ label }}</slot>
    </span>
  </label>
</template>

<style lang='scss'>
.radio-view {
  display: flex;
  flex-direction: column;
  LABEL {
    color: var(--input-label)
  }
}

.radio-container {
  position: relative;
  display: inline-flex;
  align-items: center;
  margin: 0;
  cursor: pointer;
  user-select: none;
  border-radius: var(--border-radius);

  .radio-label {
    color: var(--input-label);
    margin: 3px 10px 0px 5px;
  }

 .radio-custom {
    height: 14px;
    width: 14px;
    background-color: var(--body-bg);
    border-radius: 50%;
    transition: all 0.3s ease-out;
    border: 1px solid var(--border);

    &:focus {
      outline: none;
      border-radius: 50%;
    }
  }

  input {
    display: none;
  }

  input:checked ~ .radio-custom {
      background-color: var(--dropdown-text);
      -webkit-transform: rotate(0deg) scale(1);
      -ms-transform: rotate(0deg) scale(1);
      transform: rotate(0deg) scale(1);
      opacity:1;
      border: 1px solid var(--input-label);
  }

  input:disabled ~ .radio-custom {
    background-color: var(--disabled-bg);
  }
}
</style>
