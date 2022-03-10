<template>
  <div>
    <b-modal id="form-endereco" size="xl" v-b-modal.modal-prevent-closing centered title="Endereço">
      <div class="form-group form-row">
        <div class="form-group form-row col-sm-12">
          <label for="" class="col-form-label col-sm-2 text-right">CEP:</label>
          <input type="text" v-mask="'#####-###'" class="form-control col-sm-2"
            name="cep" v-model="endereco.cep" v-validate="'required'"
            @keyup="iniciaBusca"
            :class="{'danger-border' : errors.has('cep')}" />
        </div>
        <div class="form-group form-row col-sm-12">
          <label for="" class="col-form-label col-sm-2 text-right">Logradouro:</label>
          <input type="text" class="form-control col-sm-7"
            name="logradouro" v-model="endereco.logradouro" v-validate="'required'"
            :class="{'danger-border' : errors.has('logradouro')}"
            :disabled="endereco.logradouro_encontrado == 1">
          <label for="" class="col-form-label col-sm-2 text-right">Número:</label>
          <input type="text" class="form-control col-sm-1" v-model="endereco.numero">
        </div>
        <div class="form-group form-row">
          <ul v-if="errors.any()" class="avisos">
            <li v-if="errors.has('cep')">{{ errors.first('cep') }}</li>
            <li v-if="errors.has('logradouro')">{{ errors.first('logradouro') }}</li>
          </ul>
        </div>
        <div class="form-group form-row col-sm-12">
          <label for="" class="col-form-label col-sm-2 text-right">Complemento:</label>
          <input type="text" class="form-control col-sm-4" v-model="endereco.complemento">
          <label for="" class="col-form-label col-sm-2 text-right">Bairro:</label>
          <input type="text" class="form-control col-sm-4" v-model="endereco.bairro"
          :disabled="endereco.bairro_encontrado == 1">
        </div>
        <div class="form-group form-row col-sm-12">
          <label for="" class="col-form-label col-sm-2 text-right">Cidade:</label>
          <input type="text" class="form-control col-sm-4"
            name="cidade" v-model="endereco.cidade" v-validate="'required'"
            :class="{'danger-border' : errors.has('cidade')}" :disabled="endereco.cidade_encontrada == 1">
          <label for="" class="col-form-label col-sm-2 text-right">UF:</label>
          <input type="text" class="form-control col-sm-1"
            name="uf" v-model="endereco.uf" v-validate="'required'"
            :class="{'danger-border' : errors.has('uf')}" :disabled="endereco.estado_encontrado == 1">
        </div>
        <div class="form-group form-row">
          <ul v-if="errors.any()" class="avisos">
            <li v-if="errors.has('cidade')">{{ errors.first('cidade') }}</li>
            <li v-if="errors.has('uf')">{{ errors.first('uf') }}</li>
          </ul>
        </div>
      </div>
      <template v-slot:modal-footer>
        <b-button size="sm" variant="success" @click="salvar()">
          Salvar
        </b-button>
        <b-button size="sm" variant="danger" @click="cancelar()">
          Cancela
        </b-button>
      </template>
    </b-modal>
    <flash-message :position="'right top'"></flash-message>
  </div>
</template>

<script>
export default {
  props: {
    endereco: {
      required: true,
      type: Object,
      default: {}
    }
  },
  data() {
    return {};
  },
  methods: {
    iniciaBusca() {
      if (this.timer) {
        clearTimeout(this.timer);
        this.timer = null;
      }
      this.timer = setTimeout(() => {
        if (this.endereco.cep.length > 7) {
          this.localizaCep(this.endereco.cep);
        }
        if (this.endereco.cep.length === 0) {
          this.cleanForm();
        }
      }, 900);
    },
    localizaCep(cep) {
      this.$http.get("https://api.postmon.com.br/v1/cep/" + cep).then(
        response => {
          console.log(response);
          this.endereco.logradouro = response.body.logradouro;
          if (this.endereco.logradouro !== undefined) {
            this.endereco.logradouro.toUpperCase();
            this.endereco.logradouro.trim();
            this.endereco.logradouro_encontrado =
              this.endereco.logradouro.length > 0 ? true : false;
          }
          this.endereco.bairro = response.body.bairro;
          if (this.endereco.bairro !== undefined) {
            this.endereco.bairro.toUpperCase();
            this.endereco.bairro.trim();
            this.endereco.bairro_encontrado =
              this.endereco.bairro.length > 0 ? true : false;
          }
          this.endereco.cidade = response.body.cidade;
          if (this.endereco.cidade !== undefined) {
            this.endereco.cidade.toUpperCase();
            this.endereco.cidade.trim();
            this.endereco.cidade_encontrada =
              this.endereco.cidade.length > 0 ? true : false;
          }
          this.endereco.uf = response.body.estado;
          if (this.endereco.uf !== undefined) {
            this.endereco.uf.toUpperCase();
            this.endereco.uf.trim();
            this.endereco.estado_encontrado =
              this.endereco.uf.length > 0 ? true : false;
          }
        },
        error => {
          this.cleanForm();
          this.flashMessage.error({
            title: "CEP não foi encontrado!",
            message: "Você deve verificar se o CEP digitado está correto"
          });
          console.log(error);
        }
      );
    },
    salvar() {
      this.$validator.validateAll().then(result => {
        if (result) {
          this.$emit("saveEndereco", this.endereco);
        } else {
          this.flashMessage.error({
            title: "Registro não pode ser salvo!",
            message: "Você deve verificar alguns campos necessários"
          });
        }
      });
    },
    cleanForm() {
      this.$validator.reset();
      this.endereco.logradouro = "";
      this.logradouro_encontrado = false;
      this.endereco.bairro = "";
      this.bairro_encontrado = false;
      this.endereco.cidade = "";
      this.cidade_encontrada = false;
      this.endereco.uf = "";
      this.estado_encontrado = false;
    },
    cancelar() {
      this.$emit("cancelaAdicao");
    }
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
