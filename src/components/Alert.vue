<template>
  <teleport to="body" v-if="message.length !== 0">
    <div ref="alertRef" :class="styles.alert" v-text="message"></div>
  </teleport>
</template>

<script setup>
import { ref, toRefs, watch } from "vue";

let alertRef = ref();

let props = defineProps({ message: { type: String, default: "" } });

watch(
  () => props.message,
  (newValue) => {
    if (newValue.length === 0) return;

    setTimeout(() => {
      emit("onClose");
    }, 2000);
  }
);

let emit = defineEmits(["onClose"]);

let { message } = toRefs(props);
</script>

<style lang="scss" module="styles">
.alert {
  position: fixed;
  top: 50px;
  left: 50%;
  transform: translateX(-50%);
  min-width: 150px;
  max-width: fit-content;
  padding: 10px 15px;
  background-color: var(--alert-bg);
  color: var(--alert-text-color);
  font-size: 16px;
  border-radius: 4px;
  z-index: 999;
}
</style>
