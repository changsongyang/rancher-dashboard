<script>
import $ from 'jquery';

export default {
  props: {
    value: {
      type:    Boolean,
      default: false
    },

    label: {
      type:    [String, Boolean],
      default: false
    },

    disabled: {
      type:    Boolean,
      default: false
    },

    indeterminate: {
      type:    Boolean,
      default: false
    },

    mode: {
      type:    String,
      default: 'edit'
    }
  },

  computed: {
    isDisabled() {
      return (this.disabled || this.mode === 'view' );
    }
  },

  methods: {
    clicked(event) {
      if (!this.isDisabled) {
        const click = $.Event('click');

        click.shiftKey = event.shiftKey;
        click.altKey = event.altKey;
        click.ctrlKey = event.ctrlKey;
        click.metaKey = event.metaKey;

        this.$emit('input', !this.value);
        $(this.$el).trigger(click);
      }
    }
  }
};
</script>

<template>
  <span v-if="mode === 'view'" class="checkbox-view">
    <div class="text-label">
      <slot name="label">
        {{ label }}
      </slot>
    </div>
    <span>{{ value }}</span>
  </span>
  <label
    v-else
    class="checkbox-container"
    @keydown.enter.prevent="clicked($event)"
    @keydown.space.prevent="clicked($event)"
    @click.stop.prevent="clicked($event)"
  >
    <input
      :checked="value"
      :v-model="value"
      type="checkbox"
      :tabindex="-1"
      @click.stop.prevent
    />
    <span
      class="checkbox-custom"
      :class="{indeterminate: indeterminate}"
      :tabindex="isDisabled ? -1 : 0"
      :aria-label="label"
      :aria-checked="!!value"
      role="checkbox"
    />
    <span
      v-if="label"
      class="checkbox-label"
    >
      <slot name="label">{{ label }}</slot>
    </span>
  </label>
</template>

<style lang='scss'>
// NOTE: SortableTable depends on the names of this class, do not arbitrarily change.
.checkbox-container {
  position: relative;
  display: inline-flex;
  align-items: center;
  margin: 0;
  cursor: pointer;
  user-select: none;
  border-radius: var(--border-radius);

  .checkbox-label {
    color: var(--input-label);
    margin: 3px 10px 0px 5px;
  }

 .checkbox-custom {
    height: 14px;
    width: 14px;
    background-color: var(--body-bg);
    border-radius: var(--border-radius);
    transition: all 0.3s ease-out;
    border: 1px solid var(--border);
  }

  input {
    display: none;
  }

  input:checked ~ .checkbox-custom {
      background-color:var(--dropdown-text);
      -webkit-transform: rotate(0deg) scale(1);
      -ms-transform: rotate(0deg) scale(1);
      transform: rotate(0deg) scale(1);
      opacity:1;
      border: 1px solid var(--input-label);
      &.indeterminate{
        background-color: transparent;
        border: 1px solid var(--border)
      }
  }

  .checkbox-custom::after {
      position: absolute;
      content: "";
      left: 0px;
      top: 0px;
      height: 0px;
      width: 0px;
      border-radius: var(--border-radius);
      border: solid;
      border-color: var(--input-text);
      border-width: 0 3px 3px 0;
      -webkit-transform: rotate(0deg) scale(0);
      -ms-transform: rotate(0deg) scale(0);
      transform: rotate(0deg) scale(0);
      opacity:1;
  }

  input:checked ~ .checkbox-custom::after {
    -webkit-transform: rotate(45deg) scale(1);
    -ms-transform: rotate(45deg) scale(1);
    transform: rotate(45deg) scale(1);
    opacity:1;
    left: 4px;
    width: 4px;
    height: 10px;
    border: solid;
    border-color: var(--input-text);
    border-width: 0 2px 2px 0;
    background-color: transparent;
  }

  input:checked ~ .checkbox-custom.indeterminate::after {
    -webkit-transform:  scale(1);
    -ms-transform:  scale(1);
    transform:  scale(1);
    opacity:1;
    left: 2px;
    top:2px;
    width: 7px;
    height: 5px;
    border: solid;
    border-color: var(--dropdown-text);
    border-width: 0 0 2px 0;
    background-color: transparent;
  }

  input:disabled ~ .checkbox-custom {
    background-color: var(--disabled-bg);
  }

  .checkbox-view {
    display: flex;
    flex-direction: column;
    LABEL {
      color: var(--input-label)
    }
  }
}
</style>
