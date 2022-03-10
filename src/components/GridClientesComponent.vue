<template>
  <div>
    <div class="card">
      <div class="card-body">
        <div class="form-group form-row">
          <label class="col-form-label col-sm-1">Filtrar por:</label>
          <input type="text" class="form-control col-sm-2" placeholder="Código" v-model="filters.codigo" >
          <span class="p-2"></span>
          <input type="text" class="form-control col-sm-2" placeholder="CNPJ ou RG" v-model="filters.cnpj_cpf" >
          <span class="p-2"></span>
          <input type="text" class="form-control col-sm-2" placeholder="Nome ou Razão Social" v-model="filters.nome_razao" >
          <label class="col-sm-4 col-form-label text-center">Registros: <strong>{{ linhasExibidas }}</strong></label>
        </div>
      </div>
    </div>
    <br>
    <table class="table table-bordered table-striped table-sm">
      <thead class="thead-light">
        <tr>
          <th>Situação</th>
          <th>Código</th>
          <th>CNPJ / RG</th>
          <th>Tipo</th>
          <th>Nome / Razão Social</th>
          <th style="width: 130px;"><button @click.prevent="adicionarCliente" class="btn btn-success btn-sm">Adicionar</button></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(cliente, index) in filteredItems" :key="index">
          <td>{{ cliente.ativo ? 'Ativo' : 'Inativo' }}</td>
          <td>{{ cliente.codigo }}</td>
          <td>{{ cliente.cnpj_cpf }}</td>
          <td>{{ cliente.tipo_pessoa === 1 ? 'Física' : 'Jurídica' }}</td>
          <td>{{ cliente.nome_razao }}</td>
          <td>
            <button @click.prevent="editarCliente(cliente.id)" class="btn btn-primary btn-sm">Editar</button>
            <button @click.prevent="excluirCliente(cliente.id)" class="btn btn-danger btn-sm">Excluir</button>
          </td>
        </tr>
        <tr v-if="semClientes">
          <td colspan="6">
            Nenhum cliente para listar
          </td>
        </tr>
      </tbody>
    </table>
    <modal-form-cliente :existeClientes="existeClientes" @copiarCliente="copiar" @sairCliente="cancela" @saveCliente="saving" @excluirCliente="excluirCliente" :cliente="cliente" :title="title"></modal-form-cliente>
    <flash-message :position="'right top'"></flash-message>
  </div>
</template>

<script>
import ModalFormCliente from "./FormModalClienteComponent";

export default {
  data() {
    return {
      mensagens: "",
      title: "Cliente",
      filters: {
        codigo: "",
        cnpj_cpf: "",
        nome_razao: ""
      },
      clientes: [],
      cliente: {
        id: "",
        ativo: false,
        codigo: "",
        cnpj_cpf: "",
        tipo_pessoa: "",
        nome_razao: "",
        nome_fantasia: "",
        rg_ie: "",
        isento_ie: "",
        nascimento_inicio: "",
        observacoes: "",
        enderecos: [],
        telefones: [],
        contas: [],
        credito: {}
      }
    };
  },
  methods: {
    adicionarCliente() {
      this.cleanForm();
      this.title = "Cadastrar novo cliente";
      this.$bvModal.show("modal-form-cliente");
    },
    editarCliente(id) {
      this.title = "Editar cadastro de cliente";
      this.cleanForm();
      this.cliente = this.clientes[this.findIndex(id)];
      this.$bvModal.show("modal-form-cliente");
    },
    excluirCliente(id) {
      this.confirmarExclusao(id);
    },
    saving(cliente) {
      if (cliente.id === "") {
        cliente.id =
          this.clientes.length > 0
            ? this.clientes[this.clientes.length - 1].id + 1
            : 1;
        this.clientes.push(cliente);
        this.mensagens = "Cliente cadastrado com com êxito.";
      } else {
        this.mensagens = "Cadastro de cliente atualizado com êxito.";
        this.clientes[this.findIndex(cliente.id)] = cliente;
      }
      this.flashMessage.success({
        title: "Processo bem sucedido!",
        message: this.mensagens
      });
      this.errors.clear();
      this.$validator.reset();
    },
    copiar(id) {
      this.cleanForm();
      this.cliente = this.clientes[this.findIndex(id)];
      this.cliente.id = "";
      this.$bvModal.show("modal-form-cliente");
      this.flashMessage.info({
        title: "Informação",
        message: "O registro foi copiado, você deve salvar para ter efeito"
      });
    },
    cancela() {
      this.$bvModal.hide("modal-form-cliente");
      this.cleanForm();
    },
    cleanForm() {
      this.errors.clear();
      this.$validator.reset();
      this.cliente = {
        id: "",
        ativo: false,
        codigo: "",
        cnpj_cpf: "",
        tipo_pessoa: "",
        nome_razao: "",
        nome_fantasia: "",
        rg_ie: "",
        isento_ie: "",
        nascimento_inicio: "",
        observacoes: "",
        enderecos: [],
        telefones: [],
        contas: [],
        credito: {}
      };
    },
    findIndex(id) {
      for (let index = 0; index < this.clientes.length; index++) {
        if (this.clientes[index].id === id) {
          return index;
        }
      }
    },
    confirmarExclusao(id) {
      this.$bvModal
        .msgBoxConfirm("Deseja realmente excluir este cadastro de cliente?", {
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
            this.clientes.splice(this.findIndex(id), 1);
            this.flashMessage.success({
              title: "Processo bem sucedido!",
              message: "Cadastro de cliente foi excluído com êxito"
            });
          }
        })
        .catch(err => {
          // An error occurred
        });
    }
  },
  components: {
    ModalFormCliente
  },
  computed: {
    semClientes() {
      return this.clientes.length < 1;
    },
    existeClientes() {
      return this.clientes.length > 1;
    },
    linhasExibidas() {
      return this.clientes.length;
    },
    filteredItems() {
      let that = this;
      if (this.filters.codigo !== "") {
        return this.clientes.filter(data => {
          return (
            data.codigo
              .toLowerCase()
              .indexOf(that.filters.codigo.toLowerCase()) > -1
          );
        });
      } else if (this.filters.cnpj_cpf !== "") {
        return this.clientes.filter(data => {
          return (
            data.cnpj_cpf
              .toLowerCase()
              .indexOf(that.filters.cnpj_cpf.toLowerCase()) > -1
          );
        });
      } else if (this.filters.nome_razao !== "") {
        return this.clientes.filter(data => {
          return (
            data.nome_razao
              .toLowerCase()
              .indexOf(that.filters.nome_razao.toLowerCase()) > -1
          );
        });
      }
      return this.clientes;
    }
  }
};
</script>

<style scoped>
</style>
