<template>
  <div id="app" >
    <Header/>
      <div class="row">
        <div class="container">
        <h3>Cadastro de clientes</h3>
        <hr>
        <grid-cliente></grid-cliente>
      </div>
    </div>
      <Footer/>
  </div>

</template>

<script>
import Header from "./components/HeaderComponent";
import Footer from "./components/FooterComponent";
import GridCliente from "./components/GridClientesComponent";

export default {
  name: "app",
  data() {
    return {};
  },
  components: {
    Header,
    GridCliente,
    Footer
  }
};
</script>

<style>
</style>
