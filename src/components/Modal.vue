<template>
  <teleport to="body">
    <div v-if="show">
      <div :class="['modal', { show: isOpen }]" @click="isOpen && toggle()">
        <div class="modal_dialog" @animationend="handleAnimationEnd">
          <div class="modal_content" @click.stop>
            <slot></slot>
          </div>
        </div>
      </div>
      <div
        :class="['overlay', { show: isOpen }]"
        @click="isOpen && toggle()"
      ></div>
    </div>
  </teleport>
</template>

<script setup>
import { toRefs, ref, watchEffect } from "vue";

let props = defineProps({
  isOpen: { type: Boolean, default: false },
  toggle: { type: Function, default: () => {} },
});

let show = ref(false);

watchEffect(() => {
  let { isOpen } = props;

  if (!isOpen) return;

  show.value = isOpen;
});

const handleAnimationEnd = ({ animationName }) => {
  if (animationName == "hide_modal" || animationName == "slideOutBottom") {
    show.value = false;
  }
};

let { isOpen, toggle } = toRefs(props);
</script>

<style lang="scss" scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1050;
  .modal_dialog {
    position: relative;
    display: flex;
    .modal_content {
      width: 100%;
      background-color: white;
      padding: 30px 20px;
      margin: 15px 0px;
      border-radius: 4px;
    }
  }
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1049;
  animation: fadeOut 0.3s linear forwards;
  animation-fill-mode: forwards;
  &:is(.show) {
    animation: fadeIn 0.3s linear forwards;
  }
}

@media only screen and (min-width: 600px) {
  .modal {
    overflow-y: auto;
    &:is(.show) {
      .modal_dialog {
        animation: show_modal 0.3s ease-in-out forwards;
      }
    }
    .modal_dialog {
      min-height: 100vh;
      justify-content: center;
      align-items: center;
      animation: hide_modal 0.3s ease-in-out forwards;
    }
    .modal_content {
      max-width: 550px;
    }
  }
}

@media only screen and (max-width: 600px) {
  .modal {
    overflow-y: hidden;
    &:is(.show) {
      .rc-modal-dialog {
        animation: slideInBottom 0.3s linear forwards;
      }
    }
    .modal_dialog {
      height: 100%;
      justify-content: center;
      align-items: flex-end;
      animation: slideOutBottom 0.3s linear forwards;
      .modal_content {
        max-height: 80vh;
        overflow-y: auto;
        margin: 0px;
        border-radius: 10px 10px 0px 0px;
      }
    }
  }
}
</style>
