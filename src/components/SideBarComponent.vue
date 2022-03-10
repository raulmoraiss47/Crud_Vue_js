<template>
  <div>
    <nav class="col-md-2 d-none d-md-block bg-light sidebar">
      <div class="sidebar-sticky">
        <ul class="nav flex-column mb-2">
          <li class="nav-item">
            <router-link :to="{name: 'home'}" class="nav-link">Home</router-link>
              <span data-feather="file-text"></span>
          </li>
          <li class="nav-item">
            <router-link :to="{name: 'cadastro-clientes'}" class="nav-link">Clientes</router-link>
              <span data-feather="file-text"></span>
          </li>
        </ul>
      </div>
    </nav>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.sidebar {
  width: 120px;
  color: #222222;
  background: #bb0606;
}
</style>
