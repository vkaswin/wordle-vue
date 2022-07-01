<template>
  <div :class="styles.keyboard">
    <button
      v-for="(key, index) in keyboard"
      :key="index"
      data-state
      :data-key="key.toLowerCase()"
      @click="handleClick(key)"
    >
      {{ key }}
    </button>
  </div>
</template>

<script setup>
import keyboard from "@/utils/keyboard.json";

const emit = defineEmits([""]);

const handleClick = (key) => {
  if (key === "ENTER" || key === "BACKSPACE") return;
  emit("onKeyBoard", key);
};
</script>

<style lang="scss" module="styles">
.keyboard {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  grid-template-rows: repeat(3, minmax(55px, 1fr));
  gap: 10px;
  button {
    background-color: var(--grey-bg);
    width: 100%;
    height: 100%;
    border: 1px solid var(--tile-border-color);
    color: var(--text-color);
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
  }

  button[data-state="present"] {
    background-color: var(--yellow-bg);
  }

  button[data-state="absent"] {
    background-color: var(--dark-grey-bg);
  }

  button[data-state="correct"] {
    background-color: var(--green-bg);
  }

  button[data-key="enter"] {
    grid-row: 3 / 4;
    grid-column: 1 / 3;
    font-size: 14px;
  }

  button[data-key="backspace"] {
    grid-column: 9 / 11;
    font-size: 14px;
  }

  button[data-key="z"] {
    grid-row: 3 / 4;
    grid-column: 3 / 4;
  }
}
</style>
