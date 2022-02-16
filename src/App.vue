<template>
  <Content />
</template>

<script lang="ts">
import Content from "./components/Content.vue";
import { defineComponent } from "vue";

export default defineComponent({
  name: "App",
  components: {
    Content,
  },
});
</script>

<style lang="scss">
// #app {
// }
</style>
