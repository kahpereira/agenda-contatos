<template>
  <section>
    <form ref="form">
      <div class="input-form" v-if="!this.delete">
        <label for="nome">Nome</label>
        <input type="text" name="nome" :v-model="nome" />
        <label for="email">E-mail</label>
        <input type="email" name="email" :v-model="email" />
        <label for="telefone">Telefone</label>
        <input
          type="tel"
          name="telefone"
          :v-model="telefone"
          class="telefone"
        />
      </div>
      <div v-else class="text-delete">
        <p>Deseja realmente excluir o contato?</p>
      </div>
      <div class="buttons">
        <slot />
        <!-- <button
          type="button"
          class="button-close"
          @click="this.$emit('closeModal')"
        >
          Cancelar
        </button>
        <button
          id="btn-save"
          :class="isDisabled ? 'button-disabled' : 'button-save'"
          @click="this.$emit('saveContact')"
          :disabled="this.$emit('isDisabled')"
        >
          {{ textButton }}
        </button> -->
      </div>
    </form>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "Form",
  emits: ["closeModal", "saveContact", "isDisabled"],
  props: {
    delete: {
      type: Boolean,
    },
    dataNome: {
      type: String,
    },

    dataemail: {
      type: String,
    },

    datatelefone: {
      type: String,
    },
  },
});
</script>
