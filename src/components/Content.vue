<template>
  <section>
    <TopBar :isSearch="!contactList.length">
      <CreateButton
        @click="showModal = true"
        class="btn-topbar"
        v-if="contactList.length"
      />
      <div v-if="contactList.length">
        <input
          class="busca"
          type="text"
          name="busca"
          placeholder="Buscar..."
          v-model="searchContact"
        />
      </div>
    </TopBar>
  </section>
  <section v-if="contactList.length">
    <TableContact>
      <ContactList
        v-for="(contact, index) in filterContact"
        :key="index"
        :contact="contact"
        @deleteContact="isDelete(index)"
        @editContact="viewContact(index)"
        :list="contactList"
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
        <section>
          <form ref="form">
            <div class="input-form" v-if="!this.delete">
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
                v-maska="'(##) #####-####'"
              />
            </div>
            <div v-else class="text-delete">
              <p>Deseja realmente excluir o contato?</p>
            </div>
            <div class="buttons">
              <button type="button" class="button-close" @click="closeModal">
                Cancelar
              </button>
              <button
                id="btn-save"
                :class="isDisabled ? 'button-disabled' : 'button-save'"
                @click.prevent="submitButton"
                :disabled="isDisabled"
              >
                {{ textButton }}
              </button>
            </div>
          </form>
        </section>
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
import { maska } from "maska";

export default defineComponent({
  name: "Content",
  components: {
    CreateButton,
    Modal,
    ContactList,
    TopBar,
    TableContact,
  },
  directives: {
    maska,
  },
  data: () => ({
    showModal: false,
    contactList: [] as any,
    nome: "",
    email: "",
    telefone: "",
    colorContact: {},
    index: 0,
    edit: false,
    delete: false,
    disabled: false,
    highlightBackground: {},
    isHighlight: false,
    searchContact: "",
  }),
  computed: {
    textButton(): string {
      if (this.delete) {
        return "Excluir";
      } else return "Salvar";
    },
    titleModal(): string {
      if (this.delete) {
        return "Excluir contato";
      } else if (this.edit) {
        return "Editar contato";
      } else return "Criar contato";
    },
    isDisabled(): boolean {
      if (this.nome === "" && this.email === "" && this.telefone === "") {
        return true;
      } else {
        return false;
      }
    },
    filterContact(): void {
      if (this.searchContact) {
        return this.contactList.filter((contact: any) => {
          return this.searchContact
            .toLowerCase()
            .split(" ")
            .every((v) => contact.nome.toLowerCase().includes(v));
        });
      } else {
        return this.contactList;
      }
    },
  },
  methods: {
    closeModal(): void {
      this.emptyContact();
      this.edit = false;
      this.delete = false;
      this.showModal = false;
    },
    submitButton(): void {
      this.showModal = false;
      if (this.delete) {
        this.edit = false;
        this.delete = false;
        this.deleteContact();
      } else if (this.edit) {
        this.delete = false;
        this.edit = false;
        this.editContact();
      } else {
        this.saveContact();
        this.emptyContact();
      }
    },
    viewContact(index: number): void {
      this.showModal = true;
      this.edit = true;
      this.delete = true;
      this.index = index;
      if (this.edit || this.delete) {
        this.nome = this.contactList[index].nome;
        this.email = this.contactList[index].email;
        this.telefone = this.contactList[index].telefone;
      }
    },
    saveContact(): void {
      this.contactList.map((contact: any) => {
        contact.isHighlight = false;
      });
      this.contactList.push({
        nome: this.nome,
        email: this.email,
        telefone: this.telefone,
        colorContact: {
          backgroundColor:
            "#" + Math.floor(Math.random() * 16777215).toString(16),
        },
        isHighlight: true,
      });
      localStorage.setItem("contact-list", JSON.stringify(this.contactList));
    },
    editContact(): void {
      this.contactList[this.index].nome = this.nome;
      this.contactList[this.index].email = this.email;
      this.contactList[this.index].telefone = this.telefone;

      localStorage.setItem("contact-list", JSON.stringify(this.contactList));
      this.emptyContact();
    },
    isDelete(index: number): void {
      this.delete = true;
      this.viewContact(index);
    },
    deleteContact(): void {
      this.contactList.splice(this.index, 1);
      localStorage.setItem("contact-list", JSON.stringify(this.contactList));
      this.emptyContact();
    },
    emptyContact(): void {
      (this.nome = ""), (this.email = ""), (this.telefone = "");
    },
  },
  created() {
    if (localStorage.getItem("contact-list")) {
      try {
        this.contactList = JSON.parse(
          localStorage.getItem("contact-list") || "",
        );
      } catch (e) {
        console.error("Erro");
      }
    }
    this.contactList.forEach((contact: any) => {
      contact.isHighlight = false;
    });
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

.text-delete {
  height: 92.5px;
  padding: 19.5px 0 0 24px;
}

.telefone {
  margin-bottom: 22px;
  width: 128px;
}

.btn-topbar {
  height: 32px;
}
</style>
