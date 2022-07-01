<template>
  <div :class="styles.header">
    <div :class="styles.title">
      <b>Wordle</b>
    </div>
  </div>
  <div :class="styles.container">
    <word-tiles />
    <key-board />
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import KeyBoard from "@/components/KeyBoard.vue";
import WordTiles from "@/components/WordTiles.vue";

const theme = ref(localStorage.getItem("theme") || "dark");

onMounted(() => {
  let root = document.querySelector(":root");
  root.setAttribute("data-theme", theme.value);
});
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
  --grey-bg: #3a3a3c;
}

:root[data-theme="light"] {
  --bg-color: #ffffff;
  --text-color: black;
  --keyboard-bg: #818384;
  --tile-border-color: #d3d6da;
  --green-bg: #6aaa64;
  --grey-bg: #777c7e;
  --yellow-bg: #c9b458;
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
  max-width: 600px;
  width: 100%;
  margin: 0px auto;
}
</style>
