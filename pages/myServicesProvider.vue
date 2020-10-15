<template>
  <div>
    <h1>Mis servicios activos</h1>
    <v-text-field
      v-model="servicesOnlineUserProvider.id"
      label="Cedula"
      class="mt-md-6 px-md-6"
      disabled
    ></v-text-field>

    <v-text-field
      v-model="servicesOnlineUserProvider.idService"
      label="ID del servicio"
      class="mt-md-6 px-md-6"
      disabled
    ></v-text-field>

    <v-text-field
      v-model="servicesOnlineUserProvider.initDate"
      label="Fecha de incio del evento"
      class="mt-md-6 px-md-6"
      type="date"
      required
    ></v-text-field>

    <v-text-field
      v-model="servicesOnlineUserProvider.finDate"
      label="Fecha final del evento"
      class="mt-md-6 px-md-6"
      type="date"
      required
    ></v-text-field>

    <v-textarea
      v-model="servicesOnlineUserProvider.description"
      :counter="300"
      label="Descripcion"
      class="mt-md-6 px-md-6"
      required
    ></v-textarea>

    <v-text-field
      v-model="servicesOnlineUserProvider.total"
      type="number"
      label="Total"
      class="mt-md-6 px-md-6"
      required
    ></v-text-field>
    <v-col cols="12" md="4">
      <v-btn class="md- 600 mt-6 px-md-6" @click="editService()" v-if="editing"
        >Editar servicio</v-btn
      >
    </v-col>

    <v-data-table
      class="mt-md-6 px-md-6"
      :headers="headers"
      :items="servicesOnlineUserProviders"
      :items-per-page="10"
    >
      <!--class="elevation-1" -->

      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="loadService(item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteService(item)"> mdi-delete </v-icon>
      </template>
    </v-data-table>
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
        { text: "ID del servicio", value: "idService" },
        { text: "Fecha de inicio", value: "initDate" },
        { text: "Fecha de finalizacion", value: "finDate" },
        { text: "Descripcion", value: "description" },
        { text: "Valor total", value: "total" },
        { text: "Actions", value: "actions" },
      ],

      onlineUserProvider: {},

      servicesOnlineUserProviders: [],
      servicesOnlineUserProvider: {
        id: null,
        idService: null,
        initDate: null,
        finDate: null,
        description: null,
        total: null,
      },

      services: [],
      service: {
        id: null,
        initDate: null,
        finDate: null,
        description: null,
        total: null,
      },

      //Indica si esta en un proceso de edicion o no
      editing: false,
      services3: null,
    };
  },
  methods: {
    loadPage() {
      let services = localStorage.getItem("services");
      this.services = JSON.parse(services);
      console.log(services);
      if (services != null) {
        this.services = services;
      }
      let onlineUserProvider = localStorage.getItem("onlineUserProvider");
      this.onlineUserProvider = JSON.parse(onlineUserProvider);
      console.log(onlineUserProvider);

      //debugger
      //let services2 = new Array;
      /*La variable j se usa para asignar el idService a los eventos, para poder tener un metodo e identificar cada servicio,
      ya que la cedula podría estar más de una vez*/
      // let j = 1;

      // if (services != null) {
      //   for (var i = 0; i < this.services.length; i++) {
      //     if (this.services[i].id == this.onlineUserProvider.id) {
      //       this.servicesOnlineUserProviders.push(this.services[i]);
      //       this.servicesOnlineUserProviders[i].idService = j + "";
      //       j++;
      //     }
      //   }
      //   localStorage.setItem(
      //     "services",
      //     JSON.stringify(this.servicesOnlineUserProviders)
      //   );
      // }

      console.log(this.servicesOnlineUserProviders);
    },
    loadService(service) {
      this.servicesOnlineUserProvider = service;
      this.editing = true;
      this.services3 = service;
    },
    editService() {
      let editedService = this.servicesOnlineUserProvider;
      console.log(editedService);
      let idEditedService = editedService.idService;
      console.log(idEditedService);
      for (var i = 0; i < this.servicesOnlineUserProvider.length; i++) {
        if (this.servicesOnlineUserProvider[i].idService == idEditedService) {
          this.servicesOnlineUserProvider[i] = editedService;
          break;
        }
      }
      console.log("hola", this.servicesOnlineUserProvider);
      console.log("hola2", this.servicesOnlineUserProviders);

      localStorage.setItem(
        "services",
        JSON.stringify(this.servicesOnlineUserProviders)
      );
      this.servicesOnlineUserProvider = {};
      this.editing = false;
    },
    deleteService(service) {
      let services = localStorage.getItem("services");
      this.services = JSON.parse(services);

      var posToDelete = this.services.findIndex(
        (x) => x.idService == service.idService
      );
      if (posToDelete != -1) {
        this.services.splice(posToDelete, 1);
        localStorage.setItem("services", JSON.stringify(this.services));
      }
      console.log(this.services);
      window.location.reload();
    },
  },
};
</script>