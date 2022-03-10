<template>
  <div>
    <b-modal id="form-dado-bancario"  size="lg" v-b-modal.modal-prevent-closing centered :title="title">
      <div class="form-group form-row">
        <label for="" class="col-form-label col-sm-2 text-right">Agência:</label>
        <the-mask :mask="['####', '####-#', '####-##']" class="form-control col-sm-4" v-model="conta.agencia" />
        <label for="" class="col-form-label col-sm-2 text-right">Conta:</label>
        <the-mask :mask="['###-#', '####-#', '#####-#', '######-#']" class="form-control col-sm-4" v-model="conta.conta" />
      </div>
      <div class="form-group form-row">
        <label for="" class="col-form-label col-sm-2 text-right">Banco:</label>
        <input type="text" class="form-control col-sm-4" v-model="conta.banco">
        <label for="" class="col-form-label col-sm-2 text-right">Tipo:</label>
        <input type="text" class="form-control col-sm-4" v-model="conta.tipo">
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
    conta: {
      required: true,
      type: Object,
      default: {}
    }
  },
  data() {
    return {};
  },
  methods: {
    save() {
      this.$emit("saveConta", this.conta);
    }
  }
};
</script>

<style scoped>
</style>
