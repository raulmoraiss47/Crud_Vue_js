<template>
  <div>
    <b-modal id="modal-form-cliente"  size="xl" v-b-modal.modal-prevent-closing centered :title="title">
      <form ref="form" @submit.stop.prevent="save">
      <div>
        <div class="form-group form-row">
          <div class="col-sm-2"></div>
          <div class="col-md-offset-2 col-md-8">
            <div class="form-check form-check-inline">
              <input class="form-check-input" type="checkbox" id="inlineCheckbox1" value="1" v-model="cliente.ativo">
              <label class="form-check-label" for="inlineCheckbox1">Ativo</label>
            </div>
          </div>
        </div>
        <div class="form-group form-row">
          <label for="" class="col-form-label col-sm-2 text-right">Código:</label>
          <the-mask mask="##########" class="form-control col-sm-2" v-model="cliente.codigo"
          id="codigo" name="codigo" v-validate="'required|numeric'" :class="{'danger-border' : errors.has('codigo')}" />
          <div class="col-sm-5 text-center" :class="{'danger-border' : errors.has('tipo_pessoa')}">
            <div class="form-check form-check-inline" style="position: relative; top:8px;" >
              <input class="form-check-input" type="radio"
                name="tipo_pessoa" value="1"
                v-model="cliente.tipo_pessoa" v-validate="'required'">
              <label class="form-check-label" >Pessoa Física</label>
            </div>
            <div class="form-check form-check-inline" style="position: relative; top:8px;">
              <input class="form-check-input" type="radio"
                name="tipo_pessoa" value="2"
                v-model="cliente.tipo_pessoa" v-validate="'required'">
              <label class="form-check-label" >Pessoa Jurídica</label>
            </div>
          </div>
          <label for="" class="col-form-label col-sm-1 text-right">CNPJ/CPF:</label>
           <the-mask :mask="['###.###.###-##', '##.###.###/####-##']" class="form-control col-sm-2" name="cnpj_cpf"
          v-model="cliente.cnpj_cpf" v-validate="'required'" :class="{'danger-border' : errors.has('cnpj_cpf')}" />
        </div>
        <div class="form-group form-row">
          <ul v-if="errors.any()" class="avisos">
            <li v-if="errors.has('codigo')">
              {{ errors.first('codigo') }}
            </li>
            <li v-if="errors.has('tipo_pessoa')">
              {{ errors.first('tipo_pessoa') }}
            </li>
            <li v-if="errors.has('cnpj_cpf')">
              {{ errors.first('cnpj_cpf') }}
            </li>
          </ul>
        </div>
        <div class="form-group form-row">
          <label for="" class="col-form-label col-sm-2 text-right">Nome / Razão Social:</label>
          <input type="text" class="form-control col-sm-4" v-model="cliente.nome_razao"
            v-validate="'required|min:3'"
            :class="{'danger-border' : errors.has('nome_razao')}"
            name="nome_razao">
          <label for="" class="col-form-label col-sm-2 text-right">Nome Fantasia:</label>
          <input type="text" class="form-control col-sm-4" v-model="cliente.nome_fantasia">
        </div>
        <div class="form-group form-row">
          <ul v-if="errors.any()" class="avisos">
            <li v-if="errors.has('nome_razao')" class="text-center">
              {{ errors.first('nome_razao') }}
            </li>
          </ul>
        </div>
        <div class="form-group form-row">
          <label for="" class="col-form-label col-sm-2 text-right">RG/IE:</label>
          <input type="text" class="form-control col-sm-2" v-model="cliente.rg_ie">
          <div class="col-md-offset-2 col-sm-2">
            <div class="form-check form-check-inline" style="position: relative; top:8px;">
              <input class="form-check-input" type="checkbox" id="inlineCheckbox1" value="true" v-model="cliente.isento_ie">
              <label class="form-check-label" for="inlineCheckbox1">Isento IE</label>
            </div>
          </div>
          <label for="" class="col-form-label col-sm-4 text-right">Nascimento / Inicio Operações:</label>
          <input type="date" class="form-control col-sm-2" v-model="cliente.nascimento_inicio">
        </div>
        <div class="form-row form-group">
          <label for="" class="col-form-label col-sm-2 text-right">Observações:</label>
          <textarea name="" id="" cols="30" rows="2" class="form-control col-sm-10" style="resize: none;" v-model="cliente.observacoes"></textarea>
        </div>
        <div class="form-group form-row">
          <ul class="nav nav-tabs col-sm-12">
            <li class="nav-item"><a href="#" @click.prevent="tab = 1" class="nav-link" :class="tab == 1 ? 'active' : ''">Endereços</a></li>
            <li class="nav-item"><a href="#" @click.prevent="tab = 2" class="nav-link" :class="tab == 2 ? 'active' : ''">Contatos</a></li>
            <li class="nav-item"><a href="#" @click.prevent="tab = 3" class="nav-link" :class="tab == 3 ? 'active' : ''">Dados bancários</a></li>
            <li class="nav-item"><a href="#" @click.prevent="tab = 4" class="nav-link" :class="tab == 4 ? 'active' : ''">Crédito</a></li>
          </ul>
        </div>
        <div style="min-height: 160px;">
          <div v-show="tab == 1">
            <grid-enderecos :enderecos="cliente.enderecos" ></grid-enderecos>
          </div>
          <div v-show="tab == 2">
            <grid-telefones :telefones="cliente.telefones" ></grid-telefones>
          </div>
          <div v-show="tab == 3">
            <grid-dados-bancarios :contas="cliente.contas" ></grid-dados-bancarios>
          </div>
          <div v-show="tab == 4">
            <form-cliente-credito :credito="cliente.credito" ></form-cliente-credito>
          </div>
        </div>
        <flash-message :position="'right top'"></flash-message>
      </div>
      </form>
      <template v-slot:modal-footer>
        <b-button size="sm" variant="secondary" @click="sair()">
          Sair
        </b-button>
        <b-button size="sm" variant="secondary" @click="anterior()" v-if="existeClientes">
          Anterior
        </b-button>
        <b-button size="sm" variant="secondary" @click="proximo()" v-if="existeClientes">
          Próximo
        </b-button>
        <b-button size="sm" variant="primary" @click="copiar()" v-if="cliente.id !== ''">
          Copiar
        </b-button>
        <b-button size="sm" variant="success" @click="save()">
          {{ cliente.id !== '' ? 'Atualizar' : 'Salvar' }}
        </b-button>
        <b-button size="sm" variant="danger" @click="excluir()" v-if="cliente.id !== ''">
          Excluir
        </b-button>
      </template>
    </b-modal>
  </div>
</template>

<script>
import GridEnderecos from "./GridEnderecoComponent";
import GridTelefones from "./GridTelefonesComponent";
import GridDadosBancarios from "./GridDadosBancariosComponent";
import FormClienteCredito from "./FormClienteCreditoComponent";

export default {
  props: {
    title: {
      required: true,
      type: String,
      default: ""
    },
    cliente: {
      required: true,
      type: Object,
      default: {}
    },
    existeClientes: {
      required: true,
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      tab: 1
    };
  },
  methods: {
    save() {
      this.$validator.validateAll().then(result => {
        if (result) {
          this.$emit("saveCliente", this.cliente);
        } else {
          this.flashMessage.error({
            title: "Registro não pode ser salvo!",
            message: "Você deve verificar alguns campos necessários"
          });
        }
      });
    },
    excluir() {
      this.$emit("excluirCliente", this.cliente.id);
    },
    copiar() {
      this.$emit("copiarCliente", this.cliente.id);
    },
    sair() {
      this.$emit("sairCliente");
    },
    anterior() {
      alert("anterior");
    },
    proximo() {
      alert("proximo");
    }
  },
  components: {
    GridEnderecos,
    GridTelefones,
    GridDadosBancarios,
    FormClienteCredito
  },
  computed: {}
};
</script>

<style scoped>
.avisos {
  font-family: "Courier New", Courier, monospace;
  color: #ff0000;
  font-weight: bold;
  font-size: 12px;
}
.danger-border {
  border: 2px solid #ff0000;
}
.success-border {
  border: 2px solid #009900;
}
</style>
