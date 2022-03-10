<template>
  <div>
    <footer class="footer footer-site">
      <div class="container">
        <p class="text-center">{{ title }}</p>
        <nav>
        </nav>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "© 2022 - Raul Morais"
    };
  }
};
</script>

<style scoped>
html {
  position: relative;
  min-height: 100%;
}
body {
  /* Margin bottom by footer height */
  margin-bottom: 60px;
}
.footer {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 60px; /* Set the fixed height of the footer here */
  line-height: 60px; /* Vertically center the text there */
  background-color: #f5f5f5;
}
.footer-site {
  background: #e2e2e2;
  color: #222222;
}
.footer-site p {
  position: relative;
  height: 25px;
  line-height: 40px;
}
.footer-site nav {
  border-top: 1px solid #222222;
  text-align: right;
}
.footer-site nav li {
  list-style: none;
  display: inline-block;
}
.footer-site li:not(:first-child)::before {
  content: "|";
  padding: 0 10px;
}
.footer-site li a {
  color: #222222;
}
</style>
