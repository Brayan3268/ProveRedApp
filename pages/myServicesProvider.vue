<template>
  <div>
    <h1>Mis servicios activos</h1>  

    <v-data-table
      :headers="headers"
      :items="servicesOnlineUserProviders"
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

      servicesOnlineUserProviders:[],
      servicesOnlineUserProvider:{
        id: null,
        initDate: null,
        finDate: null,
        description: null,
        total: null,
      },

      services:[],
      service:{
        id: null,
        initDate: null,
        finDate: null,
        description: null,
        total: null,
      },

      onlineUserProvider:{},

    };
  },
  methods: {
    loadPage() {
      let services = localStorage.getItem("services");
      this.services = JSON.parse(services);
      console.log(services);

      let onlineUserProvider = localStorage.getItem("onlineUserProvider");
      this.onlineUserProvider = JSON.parse(onlineUserProvider);
      console.log(onlineUserProvider);
      debugger
      //let services2 = new Array;
      for(var i = 0; i < this.services.length; i++){
          if(this.services[i].id == this.onlineUserProvider.id)
          this.servicesOnlineUserProviders.push(this.services[i]);
      }
      console.log(this.servicesOnlineUserProviders);
    },
  },
};
</script>