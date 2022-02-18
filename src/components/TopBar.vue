<template>
  <section :class="classDinamica">
    <div class="logo">
      <img src="../assets/ic-logo.svg" alt="Logo" />
    </div>
    <slot />
    <div v-if="isSearch">
      <input
        class="busca"
        type="text"
        name="busca"
        placeholder="Buscar..."
        :disabled="isSearch"
      />
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "TopBar",
  props: {
    isSearch: {
      type: Boolean,
    },
  },
  computed: {
    classDinamica() {
      return { topbar2: this.isSearch, topbar: !this.isSearch };
    },
  },
});
</script>

<style lang="scss" scoped>
.topbar {
  padding: 16px;
  display: grid;
  grid-template-columns: 2fr 1.5fr 10fr;
  align-items: center;
}

.topbar2 {
  padding: 16px;
  display: grid;
  grid-template-columns: 2.5fr 10fr;
  align-items: center;
}
</style>
