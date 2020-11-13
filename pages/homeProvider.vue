<template>
  <div>
    <h1>Proveedor</h1>
  </div>
</template>

<script>
export default {
  beforeMount() {
    //this.loadPage();
  },
  data() {
    return {};
  },
  methods: {
    loadPage() {
      let token = localStorage.getItem("token");
      this.$axios.setToken(token, "Bearer");
    },
  },
};
</script>
