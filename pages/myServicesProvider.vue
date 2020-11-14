<template>
<!-- Muestra los servicios actuales de un proveedor y permite editar un servicio del proveedor especificando sus diferentes campos o atributos, y activarlo-->
  <div>
    <v-form ref="formEdit" v-model="formEdit" lazy-validation>
      <h1>Mis servicios activos</h1>

      <v-row>
        
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
        :rules="nameRules"
      ></v-text-field>

      <v-text-field
        v-model="servicesOnlineUserProvider.finDate"
        label="Fecha final del evento"
        class="mt-md-6 px-md-6"
        type="date"
        required
        :rules="nameRules"
      ></v-text-field>

      </v-row>

      <v-textarea
        v-model="servicesOnlineUserProvider.description"
        :counter="300"
        label="Descripcion"
        style="width: 1000px; margin-left: 12px"
        required
        :rules="nameRules"
      ></v-textarea>

      <v-text-field
        v-model="servicesOnlineUserProvider.total"
        type="number"
        label="Total"
        style="width: 200px; margin-left: 12px"
        required
        :rules="nameRules"
      ></v-text-field>

      <v-col cols="12" md="4">
        <v-btn
          style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px"
          @click="editService()"
          v-if="editing"
          color="primary"
          >Editar servicio</v-btn
        >
      </v-col>
    </v-form>

    <v-data-table
      style="margin-left: 12px"
      :headers="headers"
      :items="servicesOnlineUserProviders"
      :items-per-page="10"
    >
      <!--class="elevation-1" -->

      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="loadService(item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="dialogD = true"> mdi-delete </v-icon>

        <v-dialog v-model="dialogD" persistent>
          <v-card>
            <v-card-title class="headline">
              Eliminacion de servicio</v-card-title
            >
            <v-card-text> ¿Seguro que quieres eliminar este servicio?</v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn color="primary" text @click="deleteService(item)">
                Aceptar
              </v-btn>
              <v-btn color="primary" text @click="dialogD = false">
                Cancelar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </template>
    </v-data-table>

    <v-dialog v-model="dialog" max-width="290" persistent>
      <v-card>
        <v-card-text> Servicio editado con exito, muchas gracias </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="primary" text @click="dialog = false"> Close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogR" max-width="290">
      <v-card>
        <v-card-title class="headline"> Error </v-card-title>

        <v-card-text> Llene todos los campos </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="primary" text @click="dialogR = false"> Cerrar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
 
<script>
export default {
  beforeMount() {
    this.loadPage();
  },
  //Carga  los datos del servicio requeridos en el dataTable 
  data() {
    return {
      formEdit: null,
      dialogR: false,
      dialog: false,
      dialogD: false,
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
        idService: null,
        initDate: null,
        finDate: null,
        description: null,
        total: null,
        
      },

      //Indica si esta en un proceso de edicion o no
      editing: false,
      services3: null,
      nameRules: [
        (v) => !!v || "El campo es requerido",
        (v) => (v && v.length <= 40) || "",
      ],
    };
  },
  methods: {
    //Carga y muestra la pagina con los servicios del proveedor 
    loadPage() {
      let services = localStorage.getItem("services");

      if (services != null) {
        this.services = JSON.parse(services);
      }
      let onlineUserProvider = localStorage.getItem("onlineUserProvider");
      if (onlineUserProvider != null) {
        this.onlineUserProvider = JSON.parse(onlineUserProvider);
      }

      for (var i = 0; i < this.services.length; i++) {
        if (this.services[i].id == this.onlineUserProvider.id) {
          this.servicesOnlineUserProviders.push(this.services[i]);
        }
      }

      console.log(this.servicesOnlineUserProviders);
    },
    loadService(service) {
      this.servicesOnlineUserProvider = service;
      this.editing = true;
      this.services3 = service;
    },
    //permite editar un servicio seleccionado, previamente cargado en los campos de edición
    editService() {
      if (this.$refs.formEdit.validate() && this.formEdit) {
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

        localStorage.setItem(
          "services",
          JSON.stringify(this.servicesOnlineUserProviders)
        );
        this.servicesOnlineUserProvider = {};
        this.editing = false;
        this.dialog = true;
      } else {
        this.dialogR = true;
      }
    },
    deleteService(service) {
      if (true) {
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
      }
    },
  },
};
</script>