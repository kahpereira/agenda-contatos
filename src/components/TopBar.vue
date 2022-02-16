<template>
  <section class="top-bar">
    <div class="logo">
      <img src="../assets/ic-logo.svg" alt="Logo" />
    </div>
    <div>
      <slot />
    </div>
    <div class="busca">
      <input type="search" name="busca" placeholder="Buscar..." />
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "TopBar",
});
</script>

<style lang="scss" scoped>
.top-bar {
  padding: 16px;
  display: grid;
  grid-template-columns: 2fr 1.5fr 10fr;
  align-items: center;

  .busca {
    height: 32px;
    padding: 7px 8px 6px;
    border-radius: 4px;
    background-color: var(--pale-grey);
    background-image: url("../assets/ic-search.svg");
    background-repeat: no-repeat;
    background-position: right center;

    input {
      background-color: var(--pale-grey);
    }
  }
}
</style>
