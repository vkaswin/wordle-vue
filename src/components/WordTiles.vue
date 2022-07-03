<template>
  <div :class="styles.rows">
    <template v-for="(row, index) in rows">
      <div
        v-for="i in 5"
        :key="i"
        :class="styles.card"
        data-state
        :data-row="index"
        :data-id="i"
        v-text="row[i - 1] ?? ''"
      ></div>
    </template>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
};
</script>

<script setup>
import { toRefs } from "vue";

const props = defineProps({
  rows: {
    type: Array,
    default: [],
  },
});

const { rows } = toRefs(props);
</script>

<style lang="scss" module="styles">
.rows {
  display: grid;
  grid-template-columns: repeat(5, minmax(65px, 1fr));
  gap: 10px;
  margin: 0px auto;
  .card {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 32px;
    width: 100%;
    height: 65px;
    background-color: var(--bg-color);
    border: 2px solid var(--tile-border-color);
    color: var(--text-color);
    font-weight: bold;
    font-family: "Poppins", sans-serif;
    text-transform: capitalize;
  }

  .card[data-state="present"] {
    background-color: var(--yellow-bg);
  }

  .card[data-state="absent"] {
    background-color: var(--dark-grey-bg);
  }

  .card[data-state="correct"] {
    background-color: var(--green-bg);
  }
}

@media only screen and (max-width: 600px) {
  .rows {
    display: grid;
    grid-template-columns: repeat(5, minmax(55px, 1fr));
    gap: 10px;
    margin: 0px auto;
    .card {
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 32px;
      width: 100%;
      height: 55px;
      background-color: var(--bg-color);
      border: 2px solid var(--tile-border-color);
      color: var(--text-color);
      font-weight: bold;
      font-family: "Poppins", sans-serif;
      text-transform: capitalize;
    }
  }
}
</style>
