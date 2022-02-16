<template>
  <section class="container-contact">
    <header class="header-tb">
      <p>Contato</p>
      <p>E-mail</p>
      <p>Telefone</p>
    </header>
    <ul class="data-tb">
      <li class="contact">
        <slot />
      </li>
    </ul>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "TableContact",
});
</script>

<style lang="scss" scoped>
.container-contact {
  background-color: #ffff;
  border-radius: 4px;
  margin: 16px;
  border: 1px solid #e1e1e1;

  .header-tb {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    padding: 16px 0 9px 47px;

    p {
      font-size: 13px;
      color: #9198af;
    }
  }

  .info-contacts {
    display: grid;
    grid-template-columns: 0.4fr 4.7fr 4.7fr 4fr 0.7fr;
    align-items: center;
    padding: 12px 0;
    font-size: 14px;
    border-top: 1px solid #e1e1e1;

    &:hover {
      background-color: #fff3f2;
    }

    .icon {
      font-size: 16px;
      width: 24px;
      height: 24px;
      border-radius: 50%;
      background-color: #fa8d68;
      color: #ffff;
      margin: 0 16px 0 8px;
      padding: 3px 5px 2px 6px;
    }

    .btn-delete {
      margin-left: 24px;
    }
  }
}
</style>
