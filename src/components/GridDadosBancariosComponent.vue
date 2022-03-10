<template>
  <div>
    <table class="table table-bordered table-striped table-sm">
      <thead class="thead-light">
        <tr>
          <th>Agência</th>
          <th>Conta</th>
          <th>Banco</th>
          <th>Tipo</th>
          <th style="width: 130px;"><button @click.prevent="adicionarDadoBancario" class="btn btn-success btn-sm">Adicionar</button></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(conta, index) in contas" :key="index">
          <td>{{ conta.agencia }}</td>
          <td>{{ conta.conta }}</td>
          <td>{{ conta.banco }}</td>
          <td>{{ conta.tipo }}</td>
          <td>
            <button @click.prevent="editarConta(conta.id)" class="btn btn-primary btn-sm">Editar</button>
            <button @click.prevent="excluirConta(conta.id)" class="btn btn-danger btn-sm">Excluir</button>
          </td>
        </tr>
        <tr v-if="semContas">
          <td colspan="5">
            Nenhuma conta para listar
          </td>
        </tr>
      </tbody>
    </table>
    <form-dado-bancario @saveConta="saving" :conta="conta" :title="title"></form-dado-bancario>
  </div>
</template>

<script>
import FormDadoBancario from "./FormDadoBancarioComponent";

export default {
  props: {
    contas: {
      type: Array,
      required: true,
      default: []
    }
  },
  data() {
    return {
      title: "Conta bancária",
      conta: {
        id: "",
        agencia: "",
        conta: "",
        banco: "",
        tipo: ""
      }
    };
  },
  methods: {
    adicionarDadoBancario() {
      this.cleanForm();
      this.$bvModal.show("form-dado-bancario");
    },
    editarConta(id) {
      this.cleanForm();
      this.conta = this.contas[this.findIndex(id)];
      this.$bvModal.show("form-dado-bancario");
    },
    excluirConta(id) {
      this.confirmarExclusao(id);
    },
    saving(conta) {
      if (conta.id === "") {
        conta.id =
          this.contas.length > 0
            ? this.contas[this.contas.length - 1].id + 1
            : 1;
        this.contas.push(conta);
        this.mensagens = "Conta bancária foi cadastrada com com êxito.";
      } else {
        this.contas[this.findIndex(id)] = conta;
        this.mensagens = "Conta bancária foi atualizada com com êxito.";
      }
      this.$bvModal.hide("form-dado-bancario");
      this.cleanForm();
      this.flashMessage.success({
        title: "Processo bem sucedido!",
        message: this.mensagens
      });
    },
    cleanForm() {
      this.conta = {
        id: "",
        agencia: "",
        conta: "",
        banco: "",
        tipo: ""
      };
    },
    findIndex(id) {
      for (let index = 0; index < this.contas.length; index++) {
        if (this.contas[index].id === id) {
          return index;
        }
      }
    },
    confirmarExclusao(id) {
      this.$bvModal
        .msgBoxConfirm("Deseja realmente excluir esta conta bancária?", {
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
            if (value) {
              this.cleanForm();
              this.contas.splice(this.findIndex(id), 1);
              this.flashMessage.success({
                title: "Processo bem sucedido!",
                message: "Conta bancária foi excluída com êxito"
              });
            }
          }
        })
        .catch(err => {
          // An error occurred
        });
    }
  },
  components: {
    FormDadoBancario
  },
  computed: {
    semContas() {
      return this.contas.length < 1;
    }
  }
};
</script>

<style scoped>
</style>
