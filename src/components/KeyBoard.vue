<template>
  <div :class="styles.keyboard_container">
    <div :class="styles.keyboard_row">
      <button
        v-for="(key, index) in one"
        :key="index"
        data-state
        :data-key="key.toLowerCase()"
        @click="handleClick(key)"
      >
        {{ key }}
      </button>
    </div>
    <div :class="styles.keyboard_row">
      <button
        v-for="(key, index) in two"
        :key="index"
        data-state
        :data-key="key.toLowerCase()"
        @click="handleClick(key)"
      >
        {{ key }}
      </button>
    </div>
    <div :class="styles.keyboard_row">
      <button
        v-for="(key, index) in three"
        :key="index"
        data-state
        :data-key="key.toLowerCase()"
        @click="handleClick(key)"
      >
        {{ key }}
      </button>
    </div>
  </div>
</template>

<script setup>
import keyboard from "@/utils/keyboard.json";

const emit = defineEmits(["onKeyBoard", "onEnter", "onBackSpace"]);

const handleClick = (key) => {
  if (key === "enter") {
    emit("onEnter");
  } else if (key === "backspace") {
    emit("onBackSpace");
  } else {
    emit("onKeyBoard", key);
  }
};

const { one, two, three } = keyboard;
</script>

<style lang="scss" module="styles">
.keyboard_container {
  display: flex;
  flex-direction: column;
  gap: 10px;
  .keyboard_row {
    display: flex;
    justify-content: center;
    gap: 10px;
    button {
      background-color: var(--grey-bg);
      width: 50px;
      height: 50px;
      border: 1px solid var(--tile-border-color);
      color: var(--text-color);
      border-radius: 4px;
      font-size: 16px;
      font-family: "Poppins", sans-serif;
      cursor: pointer;
      text-transform: capitalize;
    }

    button[data-key="enter"] {
      width: 100px;
      font-size: 15px;
    }

    button[data-key="backspace"] {
      width: 125px;
      font-size: 15px;
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
  }
}
</style>
