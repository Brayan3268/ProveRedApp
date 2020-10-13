<template>
  <div>
    <h1>Mis servicios activos</h1>  

    <v-data-table
      :headers="headers"
      :items="services"
      :items-per-page="10"
      class="elevation-1"
    ></v-data-table>

  </div>
</template>

<script>
export default {
  beforeMount() {
    this.loadPage();
  },
  data() {
    return {
      
      headers: [
        { text: "Cedula", value: "id" },
        { text: "Fecha de inicio", value: "initDate" },
        { text: "Fecha de finalizacion", value: "finDate" },
        { text: "Descripcion", value: "description" },
        { text: "Valor total", value: "total" },
        { text: "Actions" },
      ],

      services:[],
      service:{
        id: null,
        initDate: null,
        finDate: null,
        description: null,
        total: null,
      },
    };
  },
  methods: {
    loadPage() {
      let services = localStorage.getItem("services");
      this.services = JSON.parse(services);
      console.log(services);
    },
  },
};
</script>