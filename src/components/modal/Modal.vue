<template>
  <Transition name="modal">
    <div v-if="show" class="modal-background">
      <div class="modal-container">
        <div class="modal-title">
          <slot name="title" />
        </div>
        <div class="modal-content">
          <slot name="body" />
        </div>
      </div>
    </div>
  </Transition>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Modal",
  props: {
    show: Boolean,
  },
});
</script>

<style lang="scss" scoped>
.modal-background {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity 0.2s ease;

  .modal-container {
    width: 432px;
    margin: 0px auto;
    background-color: #fff;
    border-radius: 16px;
    box-shadow: 0 16px 10px 0 rgba(0, 0, 0, 0.16);
    transition: all 0.3s ease;
  }

  .modal-title {
    padding: 16px 0 12.5px 16px;
  }

  .modal-content {
    border-top: solid 1px #c0c3d2;
    width: 100%;
    padding: 3.5px 0;
  }
}
</style>
