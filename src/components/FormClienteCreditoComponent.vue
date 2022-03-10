<template>
  <div>
    <div class="form-group form-row">
      <label for="" class="col-label-form col-sm-2 text-right">Situação:</label>
      <input type="text" class="form-control col-sm-2" v-model="credito.situacao">
     </div>
    <div class="form-group form-row">
      <label for="" class="col-label-form col-sm-2 text-right">Limite de crédito:</label>
      <input type="text" class="form-control col-sm-2" v-model="credito.limite">
       <label for="" class="col-label-form col-sm-2 text-right">Score:</label>
      <input type="text" class="form-control col-sm-2" v-model="credito.score">
    </div>
    <div class="form-group form-row">
      <label for="" class="col-label-form col-sm-2 text-right">Tabela:</label>
      <input type="text" class="form-control col-sm-10" v-model="credito.tabela">
    </div>
  </div>
</template>

<script>
export default {
  props: {
    credito: {
      type: Object,
      required: true,
      default: {}
    }
  },
  data() {
    return {};
  }
};
</script>

<style scoped>
</style>
