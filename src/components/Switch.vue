<template>
  <div class="switch">
    <input
      @input="
        emit('update:modelValue', modelValue === 'dark' ? 'light' : 'dark')
      "
      type="checkbox"
      :checked="modelValue === 'dark'"
      :data-theme="modelValue"
    />
    <label>
      {{ label }}
    </label>
  </div>
</template>

<script setup>
import { toRefs } from "vue";

let props = defineProps({
  label: { type: String, default: "" },
  modelValue: { type: String, default: "dark" },
});

let emit = defineEmits(["update:modelValue"]);

let { label, modelValue } = toRefs(props);
</script>

<style lang="scss">
.switch {
  display: flex;
  align-items: center;
  gap: 10px;
  input[type="checkbox"] {
    appearance: none;
    background: var(--switch-bg);
    border: none;
    width: 37px;
    height: 18px;
    border-radius: 20px;
    outline: none;
    position: relative;
    transition: background-color 0.2s ease;
    cursor: pointer;
    &::before {
      content: "";
      position: absolute;
      top: -2px;
      left: 0px;
      background-color: #fff;
      box-shadow: rgba(0, 0, 0, 0.2) 0px 2px 1px -1px,
        rgba(0, 0, 0, 0.14) 0px 1px 1px 0px, rgba(0, 0, 0, 0.12) 0px 1px 3px 0px;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      transition: left 0.2s ease, background-color 0.2s ease;
    }
    &:checked {
      background: #878a8c;
    }
    &:checked::before {
      left: calc(100% - 20px);
      background-color: #fff;
    }
  }
  input[type="checkbox"][data-theme="dark"] {
    background-color: #538d4e;
  }
  input[type="checkbox"][data-theme="light"] {
    background-color: #878a8c;
  }
  label {
    user-select: none;
    margin: 0px;
  }
}
</style>
