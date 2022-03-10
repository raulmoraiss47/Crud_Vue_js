<template>
  <div>
    <div class="d-flex flex-column flex-md-row align-items-center p-3 px-md-4 mb-3 bg-white border-bottom shadow-sm">
      <h5 class="my-0 mr-md-auto font-weight-normal"><strong>{{ title }}</strong></h5>
      <nav class="my-2 my-md-0 mr-md-3">
        <span class="p-2 text-dark">
          <router-link :to="{name: 'home'}">Home</router-link>
        </span>
        <span class="p-2 text-dark">
          <router-link :to="{name: 'cadastro-clientes'}">Clientes</router-link>
        </span>
      </nav>
      <a class="btn btn-outline-primary" href="#">Login</a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "Clientes"
    };
  }
};
</script>

<style scoped>
nav span a {
  color: #222222;
}
</style>
