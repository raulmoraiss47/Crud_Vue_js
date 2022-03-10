<template>
  <div>
    <table class="table table-bordered table-striped table-sm">
      <thead class="thead-light">
        <tr>
          <th>Contato</th>
          <th>Telefone</th>
          <th>Ramal</th>
          <th>Tipo</th>
          <th>E-mail</th>
          <th>Skype</th>
          <th style="width: 130px;"><button @click.prevent="adicionarTelefone" class="btn btn-success btn-sm">Adicionar</button></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(telefone, index) in telefones" :key="index">
          <td>{{ telefone.contato }}</td>
          <td>{{ telefone.telefone }}</td>
          <td>{{ telefone.ramal }}</td>
          <td>{{ telefone.tipo }}</td>
          <td>{{ telefone.email }}</td>
          <td>{{ telefone.skype }}</td>
          <td>
            <button @click.prevent="editarTelefone(telefone.id)" class="btn btn-primary btn-sm">Editar</button>
            <button @click.prevent="excluirTelefone(telefone.id)" class="btn btn-danger btn-sm">Excluir</button>
          </td>
        </tr>
        <tr v-if="semTelefones">
          <td colspan="7">
            Nenhum contato para listar
          </td>
        </tr>
      </tbody>
    </table>
    <form-telefone @saveTelefone="saving" :telefone="telefone" :title="title"></form-telefone>
  </div>
</template>

<script>
import FormTelefone from "./FormTelefoneComponent";

export default {
  props: {
    telefones: {
      type: Array,
      required: true,
      default: []
    }
  },
  data() {
    return {
      title: "Contato",
      telefone: {
        id: "",
        contato: "",
        telefone: "",
        ramal: "",
        tipo: "",
        email: "",
        skype: ""
      }
    };
  },
  methods: {
    adicionarTelefone() {
      this.cleanForm();
      this.title = "Novo contato";
      this.$bvModal.show("form-telefone");
    },
    editarTelefone(id) {
      this.cleanForm();
      this.title = "Editar contato";
      this.telefone = this.telefones[this.findIndex(id)];
      this.$bvModal.show("form-telefone");
    },
    excluirTelefone(id) {
      this.telefones.splice(this.findIndex(id), 1);
    },
    saving(telefone) {
      if (telefone.id === "") {
        telefone.id =
          this.telefones.length > 0
            ? this.telefones[this.telefones.length - 1].id + 1
            : 1;
        this.telefones.push(telefone);
        this.mensagens = "Contato cadastrado com com êxito.";
      } else {
        this.telefones[this.findIndex(telefone.id)] = telefone;
        this.mensagens = "Contato cadastrado com com êxito.";
      }
      this.$bvModal.hide("form-telefone");
      this.cleanForm();
      this.flashMessage.success({
        title: "Processo bem sucedido!",
        message: this.mensagens
      });
    },
    cleanForm() {
      this.telefone = {
        id: "",
        contato: "",
        telefone: "",
        ramal: "",
        tipo: "",
        email: "",
        skype: ""
      };
    },
    findIndex(id) {
      for (let index = 0; index < this.telefones.length; index++) {
        if (this.telefones[index].id === id) {
          return index;
        }
      }
    },
    confirmarExclusao(id) {
      this.confirmacao = "";
      this.$bvModal
        .msgBoxConfirm("Deseja realmente excluir este contato?", {
          title: "Confirmação",
          buttonSize: "sm",
          okVariant: "danger",
          okTitle: "Sim",
          cancelTitle: "Não",
          footerClass: "p-2",
          hideHeaderClose: false,
          centered: true
        })
        .then(value => {
          if (value) {
            this.cleanForm();
            this.telefones.splice(this.findIndex(id), 1);
            this.flashMessage.success({
              title: "Processo bem sucedido!",
              message: "Contato foi excluído com êxito"
            });
          }
        })
        .catch(err => {
          // An error occurred
        });
    }
  },
  components: {
    FormTelefone
  },
  computed: {
    semTelefones() {
      return this.telefones.length < 1;
    }
  }
};
</script>

<style scoped>
</style>
