<template>
  <button class="create-button">
    <img src="../assets/ic-plus.svg" alt="Plus" />
    <p>Criar contato</p>
  </button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "CreateButton",
});
</script>

<style lang="scss">
.create-button {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 12px 0;
  background-color: var(--yellow);
  border-radius: 20px;
  width: 144px;
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.16),
    0 0 0 0.5px rgba(0, 0, 0, 0.08), inset 0 0 0 0.5px rgba(0, 0, 0, 0.08),
    0 2px 4px 0.5px rgba(0, 0, 0, 0.16);

  p {
    padding-left: 8px;
    color: var(--red);
    font-size: 14px;
  }
}
</style>
