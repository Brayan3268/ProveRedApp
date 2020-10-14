<template>
  <div>
    <v-form ref="formService" v-model="formService">
      <v-textarea
        v-model="service.description"
        :counter="300"
        :rules="serviceRules"
        class="px-md-6 mx-lg-auto"
        label="Describa su servicio(s)"
      >
      </v-textarea>
      <p>Fecha de inicio y finalizacion del servicio</p>
      <v-row>
        <v-col cols="12" sm="6">
          <v-date-picker v-model="dates" range locale="es-CO"> </v-date-picker>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="dateRangeText"
            label="Fecha Inicio - Fecha Finalizacion"
            prepend-icon="mdi-calendar"
            :rules="nameRules"
            readonly
          ></v-text-field>
          <!-- model: {{ dates }} -->
        </v-col>
      </v-row>
      <v-text-field
        label="Total"
        prepend-icon=""
        type="number"
        :rules="nameRules"
        v-model="service.total"
      ></v-text-field>
    </v-form>

    <v-btn @click="createService()" color="primary">Nuevo servicio</v-btn>
  </div>
</template>
<script>
export default {
  beforeMount() {
    this.loadInfo();
    console.log(this.services);
  },
  beforeUpdate(){
    //window.location.reload();
  },
  data: () => ({
    formService: null,
    dates: [],
    serviceRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 300 && v.length > 30) || "",
    ],
    nameRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 40) || "",
    ],
    onlineUserProvider: {},
    service: {
      id: null,
      idService: null,
      initDate: null,
      finDate: null,
      description: null,
      total: null,
    },
    services: [],
  }),
  computed: {
    dateRangeText() {
      return this.dates.join(" ~ ");
    },
  },
  methods: {
    loadInfo() {
      let onlineUserProvider = localStorage.getItem("onlineUserProvider");
      if (onlineUserProvider != null) {
        this.onlineUserProvider = JSON.parse(onlineUserProvider);
      }

      let services = localStorage.getItem("services");
      if(services != null){
        this.services = JSON.parse(services);
      }

      let j = 1;

      if(this.services != null){
        for(var i = 0; i < this.services.length; i++){
          this.services[i].idService = j + "";
          j++;
        }
      }
      localStorage.setItem("services", JSON.stringify(this.services));
      console.log(this.service);
    },
    createService() {
      if (this.$refs.formService.validate() && this.formService) {
        this.service.id = this.onlineUserProvider.id;
        this.service.initDate = this.dates[0];
        this.service.finDate = this.dates[this.dates.length - 1];
        this.services.push(this.service);
        //console.log(this.services);
        localStorage.setItem("services", JSON.stringify(this.services));
        this.service = {};
      }
    },
  },
};
</script>