<template>
  <div class="info-contacts" :style="highlightBackground">
    <p class="icon" :style="contact.colorContact">
      {{ contact.nome.charAt(0) }}
    </p>
    <p>{{ contact.nome }}</p>
    <p>{{ contact.email }}</p>
    <p>{{ contact.telefone }}</p>
    <div class="buttons-contact">
      <button class="btn-edit" @click="this.$emit('editContact')">
        <img src="../assets/ic-edit.svg" alt="Editar" />
      </button>
      <button class="btn-delete">
        <img
          src="../assets/ic-delete.svg"
          alt="Excluir"
          @click="this.$emit('deleteContact')"
        />
      </button>
    </div>
  </div>
  <slot />
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "ContactList",
  emits: ["deleteContact", "editContact"],
  data: () => ({
    highlightBackground: { backgroundColor: "#ffffff" },
  }),
  props: {
    contact: {
      type: Object,
    },
    list: {
      type: Array,
      required: true,
    },
  },
  methods: {
    checkBackgroundColor(): void {
      if (this.contact?.isHighlight) {
        this.highlightBackground = { backgroundColor: "#fff3f2" };
        setTimeout(() => {
          this.highlightBackground = { backgroundColor: "#ffffff" };
        }, 10000);
      }
    },
  },
  created() {
    this.checkBackgroundColor();
  },
});
</script>

<style lang="scss" scoped>
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
    color: #ffff;
    margin: 0 16px 0 8px;
    padding: 3px 5px 2px 6px;
  }

  .btn-delete {
    margin-left: 24px;
  }
}
</style>
