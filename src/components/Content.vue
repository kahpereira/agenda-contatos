<template>
  <section>
    <TopBar>
      <CreateButton
        v-if="contactsList.length > 0"
        @click="showModal = true"
        class="btn-topbar"
      />
    </TopBar>
  </section>
  <section v-if="contactsList.length">
    <TableContact>
      <ContactList
        v-for="(contact, index) in contactsList"
        :key="index"
        :nome="contact.nome"
        :email="contact.email"
        :telefone="contact.telefone"
        @deleteContact="deleteContact(index)"
        @editContact="editContact(index)"
      />
    </TableContact>
  </section>
  <section v-else class="empty">
    <img src="../assets/ic-book.svg" alt="Book" />
    <p class="pa-6">Nenhum contato foi criado ainda.</p>
    <CreateButton @click="showModal = true" />
  </section>

  <Teleport to="body">
    <modal :show="showModal">
      <template #title>
        <p>{{ titleModal }}</p>
      </template>
      <template #body>
        <form ref="form">
          <div class="input-form">
            <label for="nome">Nome</label>
            <input type="text" name="nome" v-model="nome" />
            <label for="email">E-mail</label>
            <input type="email" name="email" v-model="email" />
            <label for="telefone">Telefone</label>
            <input
              type="tel"
              name="telefone"
              v-model="telefone"
              class="telefone"
            />
          </div>
          <div class="buttons">
            <button type="button" class="button-close" @click="closeModal">
              Cancelar
            </button>
            <button id="btn-save" class="button-save" @click="saveContact">
              {{ textButton }}
            </button>
          </div>
        </form>
      </template>
    </modal>
  </Teleport>
</template>

<script lang="ts">
import CreateButton from "./CreateButton.vue";
import Modal from "./modal/Modal.vue";
import { defineComponent } from "vue";
import ContactList from "@/components/ContactList.vue";
import TopBar from "@/components/TopBar.vue";
import TableContact from "@/components/TableContact.vue";

export default defineComponent({
  name: "Content",
  components: {
    CreateButton,
    Modal,
    ContactList,
    TopBar,
    TableContact,
  },
  data: () => ({
    showModal: false,
    contactsList: [] as any,
    nome: "",
    email: "",
    telefone: "",
    index: null,
    edit: false,
    delete: false,
  }),
  computed: {
    textButton(): string {
      if (this.delete) {
        return "Excluir";
      } else return "Salvar";
    },
    titleModal(): string {
      return "Criar contato";
    },
  },
  methods: {
    closeModal() {
      this.limparContato();
      this.edit = false;
      this.delete = false;
      this.showModal = false;
    },
    saveContact(index: any) {
      if (!this.edit && !this.delete) {
        this.contactsList.push({
          nome: this.nome,
          email: this.email,
          telefone: this.telefone,
        });

        localStorage.setItem("contact-list", JSON.stringify(this.contactsList));
      } else if (this.delete) {
        this.remove();
      } else if (this.edit) {
        this.editar(index);
      }
    },
    viewContact(index: number) {
      if (this.edit || this.delete) {
        this.nome = this.contactsList[index].nome;
        this.email = this.contactsList[index].email;
        this.telefone = this.contactsList[index].telefone;
      }
    },
    editContact(index: any) {
      this.index = index;
      this.showModal = true;
      this.edit = true;
      this.viewContact(index);
    },
    deleteContact(index: any) {
      this.index = index;
      this.showModal = true;
      this.delete = true;
      this.viewContact(index);
    },
    editar(index: any) {
      index = this.index;
      this.contactsList[index].nome = this.nome;
      this.contactsList[index].email = this.email;
      this.contactsList[index].telefone = this.telefone;

      localStorage.setItem("contact-list", JSON.stringify(this.contactsList));
    },
    remove() {
      this.contactsList.splice(this.index, 1);
      localStorage.setItem("contact-list", JSON.stringify(this.contactsList));
    },
    limparContato() {
      (this.nome = ""), (this.email = ""), (this.telefone = "");
    },
  },
  created() {
    if (localStorage.getItem("contact-list")) {
      try {
        this.contactsList = JSON.parse(
          localStorage.getItem("contact-list") || "",
        );
      } catch (e) {
        console.error("Erro");
      }
    }
  },
});
</script>

<style lang="scss" scoped>
.empty {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: calc(100vh - 68px);
  width: 100vw;
}

.input-form {
  display: flex;
  flex-direction: column;
  font-size: 14px;
  padding: 0 24px;

  input {
    border-radius: 4px;
    border: solid 1px #c0c3d2;
    height: 32px;
    padding-left: 10px;
  }

  label {
    padding-top: 16px;
  }
}

.telefone {
  margin-bottom: 22px;
  width: 176px;
}

.btn-topbar {
  height: 32px;
}
</style>
