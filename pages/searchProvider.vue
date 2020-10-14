<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="servicesUserProviders"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mdi-phone" @click="loadUser(item)">
          mdi-phone
        </v-icon>
      </template>
    </v-data-table>
    <v-dialog v-model="dialogContact" max-width="600px">
      <v-card>
        <v-card-title class="headline"
          >Que opcion de contactos desea usar</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="cancelDialog()"
            >Cancel</v-btn
          >
          <v-btn color="blue darken-1" text>OK</v-btn>
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  layout: "defaultClient",
  beforeMount() {
    this.loadInfo();
  },
  data() {
    return {
      servicesUserProviders: [],
      services: [],
      userProviders: [],
      userProvider: {},
      service: {},
      headers: [
        { text: "nombre del proovedor", value: "id" },
        { text: "Descripcion del servicio", value: "description" },
        { text: "Rango de disponibilidad", value: "initDate" },
        { text: "Rando de disponiblidad", value: "finDate" },
        { text: "Valor total", value: "total" },
        { text: "Actions", value: "actions" },
      ],
      dialogContact: false,
    };
  },
  methods: {
    loadInfo() {
      let services = localStorage.getItem("services");
      let userProvider = localStorage.getItem("userProviders");

      if (services != null && userProvider != null) {
        this.services = JSON.parse(services);
        this.userProviders = JSON.parse(userProvider);
        this.servicesUserProviders = this.services;
      }
    },
    loadUser(user) {
      this.dialogContact = true;
    },
    cancelDialog() {
      this.dialogContact = false;
    },
  },
};
</script>

