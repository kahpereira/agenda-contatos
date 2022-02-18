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
        :colorContact="contact.colorContact"
        @deleteContact="isDelete(index)"
        @editContact="viewContact(index)"
        :highlight="highlight"
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
                @click.prevent="saveContact"
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
    contactsList: [] as any,
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
  },
  methods: {
    closeModal(): void {
      this.emptyContact();
      this.edit = false;
      this.delete = false;
      this.showModal = false;
    },
    saveContact(index: number): void {
      this.showModal = false;
      if (!this.edit || !this.delete) {
        this.contactsList.map((contact: any) => {
          contact.isHighlight = false;
        });
        this.contactsList.push({
          nome: this.nome,
          email: this.email,
          telefone: this.telefone,
          colorContact: {
            backgroundColor:
              "#" + Math.floor(Math.random() * 16777215).toString(16),
          },
          isHighlight: true,
        });
        localStorage.setItem("contact-list", JSON.stringify(this.contactsList));
        this.emptyContact();
      } else if (this.delete) {
        this.delete = false;
        this.deleteContact();
      } else if (this.edit) {
        // this.edit = false;
        this.editContact(index);
      }
    },
    highlight(contact: any): void {
      this.contactsList.findIndex((x: any) => {
        x === contact;
      });
      if (this.contactsList[this.contactsList.length - 1].isHighlight) {
        this.highlightBackground = { backgroundColor: "#fff3f2" };
        setTimeout(() => {
          this.highlightBackground = { backgroundColor: "#ffffff" };
        }, 10000);
      }
    },
    viewContact(index: any): void {
      this.showModal = true;
      this.edit = true;
      this.index = index;
      console.log(this.index);
      if (this.edit || this.delete) {
        this.nome = this.contactsList[index].nome;
        this.email = this.contactsList[index].email;
        this.telefone = this.contactsList[index].telefone;
      }
    },
    editContact(index: any): void {
      // index = this.index;
      // console.log(index);
      this.contactsList[index].nome = this.nome;
      this.contactsList[index].email = this.email;
      this.contactsList[index].telefone = this.telefone;
      console.log(this.contactsList);

      localStorage.setItem("contact-list", JSON.stringify(this.contactsList));

      this.emptyContact();
    },
    isDelete(index: number): void {
      this.delete = true;
      this.viewContact(index);
    },
    deleteContact(): void {
      this.contactsList.splice(this.index, 1);
      localStorage.setItem("contact-list", JSON.stringify(this.contactsList));
      this.emptyContact();
    },
    emptyContact(): void {
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
