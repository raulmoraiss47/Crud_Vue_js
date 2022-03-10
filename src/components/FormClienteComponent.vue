<template>
  <div class="form-group">
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
      <input type="text" class="form-control col-sm-2" v-model="cliente.codigo"
      id="codigo" name="codigo" v-validade="'required|numeric'">
      <div class="col-sm-5 text-center">
        <div class="form-check form-check-inline" style="position: relative; top:8px;">
          <input class="form-check-input" type="radio" name="pessoa_tipo" value="1" v-model="cliente.tipo_pessoa">
          <label class="form-check-label" for="inlineRadio1">Pessoa Física</label>
        </div>
        <div class="form-check form-check-inline" style="position: relative; top:8px;">
          <input class="form-check-input" type="radio" name="pessoa_tipo" value="2" v-model="cliente.tipo_pessoa">
          <label class="form-check-label" for="inlineRadio2">Pessoa Jurídica</label>
        </div>
      </div>
      <label for="" class="col-form-label col-sm-1 text-right">CNPJ/CPF:</label>
      <input type="text" class="form-control col-sm-2" id="cnpj_cpf" name="cnpj_cpf"
      v-model="cliente.cnpj_cpf">
    </div>
    <div class="form-group form-row">
      <p v-if="errors.has('codigo')">
        {{ errors.first('codigo') }}
      </p>
    </div>
    <div class="form-group form-row">
      <label for="" class="col-form-label col-sm-2 text-right">Nome / Razão Social:</label>
      <input type="text" class="form-control col-sm-4" v-model="cliente.nome_razao" id="nome_razao"
       name="nome_razao">
      <label for="" class="col-form-label col-sm-2 text-right">Nome Fantasia:</label>
      <input type="text" class="form-control col-sm-4" v-model="cliente.nome_fantasia">
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
</template>

<script>
import GridEnderecos from "./GridEnderecoComponent";
import GridTelefones from "./GridTelefonesComponent";
import GridDadosBancarios from "./GridDadosBancariosComponent";
import FormClienteCredito from "./FormClienteCreditoComponent";

export default {
  props: {
    cliente: {
      required: true,
      type: Object,
      default: {}
    }
  },
  data() {
    return {
      tab: 1
    };
  },
  components: {
    GridEnderecos,
    GridTelefones,
    GridDadosBancarios,
    FormClienteCredito
  }
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
