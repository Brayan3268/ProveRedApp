<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="services"
      :items-per-page="10"
      class="elevation-1"
    >
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mdi-phone" @click="getService(item)">
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

          <v-row>
            <v-col cols="12" sm="6">
              <a
                href="https://api.whatsapp.com/send?phone=573137364711&text=Bienvenido"
                target="_blank"
              >
                <v-icon class="mdi-whatsapp">mdi-whatsapp </v-icon>
              </a>
            </v-col>
          </v-row>
          <v-spacer></v-spacer>
          <v-row>
            <v-col cols="12" sm="6">
              <v-btn color="blue darken-1" text @click="cancelDialog()"
                >Cancel</v-btn
              >
            </v-col>
          </v-row>

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
    console.log(this.contracts);
  },
  data() {
    return {
      servicesUserProviders: [],
      services: [],
      userProviders: [],
      userProvider: {},
      onlineUserClient: {},
      service: {},
      contracts: [],
      contract: {
        idContract: null,
        idService: null,
        idProvider: null,
        idClient: null,

        nameFile: null,
        pathFile: null,

        isTotalAceptedProvider: null,
        isTotalAceptedClient: null,

        isAceptedProvider: null,
        isAceptedClient: null,
        dataFile: null,
      },
      headers: [
        { text: "nombre del proovedor", value: "idService" },
        { text: "Descripcion del servicio", value: "description" },
        { text: "Rango de disponibilidad", value: "initDate" },
        { text: "Rando de disponiblidad", value: "finDate" },
        { text: "Valor total", value: "total" },
        { text: "Opciones", value: "actions" },
      ],
      dialogContact: false,
    };
  },
  methods: {
    loadInfo() {
      let services = localStorage.getItem("services");

      let onlineUserClient = localStorage.getItem("onlineUserClient");
      let contracts = localStorage.getItem("contracts");

      if (onlineUserClient != null) {
        this.onlineUserClient = JSON.parse(onlineUserClient);
      }
      if (services != null) {
        this.services = JSON.parse(services);
      }
      if (contracts != null) {
        this.contracts = JSON.parse(contracts);
      }
    },
    getService(service) {
      //validacion exitencia
      this.dialogContact = true;
      this.contract.idService = service.idService;
      this.contract.idProvider = service.id;
      this.contract.idClient = this.onlineUserClient.id;

      if (this.contracts.length != 0) {
        for (var i = 0; i < this.contracts.length; i++) {
          if (this.contracts.length - 1 == i) {
            this.contract.idContract = i + 2;
          }
        }
      } else {
        this.contract.idContract = 1;
      }

      console.log(this.contract);
      this.contracts.push(this.contract);
      localStorage.setItem("contracts", JSON.stringify(this.contracts));
      this.contract = {
        nameFile: null,
        pathFile: null,

        isTotalAceptedProvider: null,
        isTotalAceptedClient: null,

        isAceptedProvider: null,
        isAceptedClient: null,
        dataFile: null,
      };
    },
    cancelDialog() {
      this.dialogContact = false;
    },
  },
};
</script>

