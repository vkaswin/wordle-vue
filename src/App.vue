<template>
  <div :class="styles.header">
    <div :class="styles.title">
      <b>Wordle</b>
    </div>
  </div>
  <div :class="styles.container">
    <WordTiles />
    <KeyBoard @onKeyBoard="addWordInTile" />
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import KeyBoard from "@/components/KeyBoard.vue";
import WordTiles from "@/components/WordTiles.vue";
import words from "@/utils/words.json";
import alphabets from "@/utils/alphabets.json";

const theme = ref(localStorage.getItem("theme") || "dark");

const word = ref();

const getRandomWord = () => {
  return words[Math.floor(Math.random() * words.length)];
};

const alphabet = new Set(alphabets);

onMounted(() => {
  window.addEventListener("keyup", handleKeyUp);
  word.value = getRandomWord();
  let root = document.querySelector(":root");
  root.setAttribute("data-theme", theme.value);
});

const handleKeyUp = ({ key }) => {
  if (!alphabet.has(key)) return;
  addWordInTile(key.toUpperCase());
};

const addWordInTile = (word) => {
  console.log(word);
};

const clearWordInTile = (row) => {
  console.log(row);
};
</script>

<style lang="scss" module="styles">
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}

:root[data-theme="dark"] {
  --bg-color: #121213;
  --text-color: white;
  --keyboard-bg: #d4d7db;
  --tile-border-color: #3a3a3c;
  --yellow-bg: #b59f3b;
  --green-bg: #538d4e;
  --dark-grey-bg: #3a3a3c;
  --grey-bg: #818384;
}

:root[data-theme="light"] {
  --bg-color: #ffffff;
  --text-color: black;
  --keyboard-bg: #818384;
  --tile-border-color: #d3d6da;
  --green-bg: #6aaa64;
  --dark-grey-bg: #777c7e;
  --yellow-bg: #c9b458;
  --grey-bg: #d3d6da;
}

body {
  font-family: "Poppins", sans-serif;
  background-color: var(--bg-color);
  color: var(--text-color);
}

.header {
  padding: 7px 0px;
  border-bottom: 1px solid var(--tile-border-color);
  display: flex;
  justify-content: center;
  .title {
    font-size: 32px;
  }
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 20px;
  max-width: 600px;
  width: 100%;
  margin: 0px auto;
  min-height: calc(100vh - 63px);
  padding: 15px 0px;
  user-select: none;
}
</style>
