<template>
  <div :class="styles.tiles">
    <div
      v-for="(word, index) in one"
      :key="index"
      :class="styles.card"
      data-state
    >
      <b>{{ word }}</b>
    </div>
    <div
      v-for="(word, index) in two"
      :key="index"
      :class="styles.card"
      data-state
    >
      <b>{{ word }}</b>
    </div>
    <div
      v-for="(word, index) in three"
      :key="index"
      :class="styles.card"
      data-state
    >
      <b>{{ word }}</b>
    </div>
    <div
      v-for="(word, index) in four"
      :key="index"
      :class="styles.card"
      data-state
    >
      <b>{{ word }}</b>
    </div>
    <div
      v-for="(word, index) in five"
      :key="index"
      :class="styles.card"
      data-state
    >
      <b>{{ word }}</b>
    </div>
    <div
      v-for="(word, index) in six"
      :key="index"
      :class="styles.card"
      data-state
    >
      <b>{{ word }}</b>
    </div>
  </div>
</template>

<script setup>
import { reactive, toRefs } from "vue";

const state = reactive({
  one: ["", "", "", "", ""],
  two: ["", "", "", "", ""],
  three: ["", "", "", "", ""],
  four: ["", "", "", "", ""],
  five: ["", "", "", "", ""],
  six: ["", "", "", "", ""],
});

const { one, two, three, four, five, six } = toRefs(state);
</script>

<style lang="scss" module="styles">
.tiles {
  display: grid;
  grid-template-columns: repeat(5, minmax(65px, 1fr));
  gap: 10px;
  margin: 0px auto;
  .card {
    width: 100%;
    height: 65px;
    background-color: var(--bg-color);
    border: 2px solid var(--tile-border-color);
  }
}
</style>
