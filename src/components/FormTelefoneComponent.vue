<template>
  <div>
    <b-modal id="form-telefone"  size="lg" v-b-modal.modal-prevent-closing centered :title="title">
      <div class="form-group form-row">
        <label for="" class="col-form-label col-sm-2 text-right">Contato:</label>
        <input type="text" class="form-control col-sm-10" v-model="telefone.contato">
      </div>
      <div class="form-group form-row">
        <label for="" class="col-form-label col-sm-2 text-right">Telefone:</label>
        <the-mask :mask="['(##) ####-####', '(##) #####-####']" class="form-control col-sm-3" v-model="telefone.telefone" />
        <label for="" class="col-form-label col-sm-1 text-right">Ramal:</label>
        <input type="text" class="form-control col-sm-2" v-model="telefone.ramal">
        <label for="" class="col-form-label col-sm-1 text-right">Tipo:</label>
        <input type="text" class="form-control col-sm-3" v-model="telefone.tipo">
      </div>
      <div class="form-group form-row">
        <label for="" class="col-form-label col-sm-2 text-right">E-mail:</label>
        <input type="text" class="form-control col-sm-10" v-model="telefone.email">
      </div>
      <div class="form-group form-row">
        <label for="" class="col-form-label col-sm-2 text-right">Skype:</label>
        <input type="text" class="form-control col-sm-10" v-model="telefone.skype">
      </div>
      <template v-slot:modal-footer="{ ok, cancel, hide }">
        <b-button size="sm" variant="success" @click="save()">
          Salvar
        </b-button>
        <b-button size="sm" variant="danger" @click="cancel()">
          Cancela
        </b-button>
      </template>
    </b-modal>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      required: true,
      type: String,
      default: ""
    },
    telefone: {
      required: true,
      type: Object,
      default: {}
    },
    title: {
      required: true,
      type: String,
      default: ""
    }
  },
  data() {
    return {};
  },
  methods: {
    save() {
      this.$emit("saveTelefone", this.telefone);
    }
  }
};
</script>

<style scoped>
</style>
