<template>
  <div>
    <table class="table table-striped table-bordered table-sm">
      <thead class="thead-light">
        <tr>
          <th>CEP</th>
          <th>Logradouro</th>
          <th>Número</th>
          <th>Complemento</th>
          <th>Bairro</th>
          <th>Cidade</th>
          <th>UF</th>
          <th>Tipo</th>
          <th style="width: 130px;"><button @click.prevent="adicionarEndereco" class="btn btn-success btn-sm">Adicionar</button></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(endereco, index) in enderecos" :key="index">
          <td>{{ endereco.cep }}</td>
          <td>{{ endereco.logradouro }}</td>
          <td>{{ endereco.numero }}</td>
          <td>{{ endereco.complemento }}</td>
          <td>{{ endereco.bairro }}</td>
          <td>{{ endereco.cidade }}</td>
          <td>{{ endereco.uf }}</td>
          <td>{{ endereco.tipo }}</td>
          <td>
            <button @click.prevent="editEndereco(endereco.id)" class="btn btn-primary btn-sm">Editar</button>
            <button @click.prevent="excluirEndereco(endereco.id)" class="btn btn-danger btn-sm">Excluir</button>
          </td>
        </tr>
        <tr  v-if="semEnderecos">
          <td colspan="9">
            Nenhum endereço para listar
          </td>
        </tr>
      </tbody>
    </table>
    <form-endereco :endereco="endereco" @saveEndereco="saving" @cancelaAdicao="cancelaAdicionarEndereco" ></form-endereco>
  </div>
</template>

<script>
import FormEndereco from "./FormEnderecoComponent";

export default {
  props: {
    enderecos: {
      type: Array,
      required: true,
      default: []
    }
  },
  data() {
    return {
      mensagens: "",
      endereco: {
        id: "",
        logradouro: "",
        numero: "",
        complemento: "",
        bairro: "",
        cidade: "",
        uf: "",
        tipo: "",
        logradouro_encontrado: false,
        bairro_encontrado: false,
        cidade_encontrada: false,
        estado_encontrado: false
      },
      idToUpdate: ""
    };
  },
  methods: {
    adicionarEndereco() {
      this.$bvModal.show("form-endereco");
    },
    editEndereco(id) {
      this.cleanForm();
      this.idToUpdate = id;
      this.endereco = this.enderecos[this.findIndex(id)];
      this.$bvModal.show("form-endereco");
    },
    excluirEndereco(id) {
      this.confirmarExclusao(id);
    },
    saving(endereco) {
      if (endereco.id === "") {
        endereco.id =
          this.enderecos.length > 0
            ? this.enderecos[this.enderecos.length - 1].id + 1
            : 1;
        this.enderecos.push(endereco);
        this.mensagens = "Endereço foi cadastrado com com êxito.";
      } else {
        this.enderecos[this.findIndex(endereco.id)] = endereco;
        this.mensagens = "Endereço foi atualizado com com êxito.";
      }
      this.$bvModal.hide("form-endereco");
      this.cleanForm();
      this.flashMessage.success({
        title: "Processo bem sucedido!",
        message: this.mensagens
      });
    },
    findIndex(id) {
      for (let index = 0; index < this.enderecos.length; index++) {
        if (this.enderecos[index].id === id) {
          return index;
        }
      }
    },
    cleanForm() {
      this.endereco = {
        id: "",
        logradouro: "",
        numero: "",
        complemento: "",
        bairro: "",
        cidade: "",
        uf: "",
        tipo: "",
        logradouro_encontrado: false,
        bairro_encontrado: false,
        cidade_encontrada: false,
        estado_encontrado: false
      };
      this.idToUpdate = "";
    },
    confirmarExclusao(id) {
      this.confirmacao = "";
      this.$bvModal
        .msgBoxConfirm("Deseja realmente excluir o endereço?", {
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
          this.cleanForm();
          this.enderecos.splice(this.findIndex(id), 1);
          this.flashMessage.success({
            title: "Processo bem sucedido!",
            message: "Endereço foi excluído com êxito"
          });
        })
        .catch(err => {
          // An error occurred
        });
    },
    cancelaAdicionarEndereco() {
      this.cleanForm();
      this.idToUpdate = "";
      this.$bvModal.hide("form-endereco");
    }
  },
  components: {
    FormEndereco
  },
  computed: {
    semEnderecos() {
      return this.enderecos.length < 1;
    }
  }
};
</script>

<style scoped>
</style>
