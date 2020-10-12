<template>
  <div>
    <h2>
      Para continuar con el proceso de creacion de cuenta, por favor rellene los
      siguientes campos de acuerdo a su informacion como proveedor
    </h2>

    <!-- Se crea el form de proveedor -->

    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="users.length"
        :counter="40"
        :rules="nameRules"
        label="Documento de identidad"
        disabled
        class="mt-md-6 px-md-6"
        required
      ></v-text-field>

      <v-text-field
        v-model="userProvider.nameCompany"
        :counter="40"
        :rules="nameRules"
        label="Nombre de la empresa"
        class="px-md-6 mx-lg-auto"
        required
      ></v-text-field>

      <v-select
        v-model="userProvider.typeProvider"
        :items="typeProviderSelect"
        :rules="[(v) => !!v || 'Item is required']"
        label="Tipo de proveedor"
        class="px-md-6 mx-lg-auto"
        required
      ></v-select>

      <!-- Aqui el proveedor va a dar detalles sobre su servicio o servicios -->
      <v-textarea
        v-model="userProvider.serviceDescription"
        :counter="300"
        :rules="serviceRules"
        class="px-md-6 mx-lg-auto"
        label="Describa su servicio(s)"
      >
      </v-textarea>

      <v-checkbox
        v-model="checkbox"
        :rules="[(v) => !!v || 'You must agree to continue!']"
        label="¿Esta de acuerdo?"
        class="px-md-6 mx-lg-auto"
        required
      ></v-checkbox>

      <v-btn :disabled="!valid" class="mr-4" @click="finishFormProvider">
        Continuar
      </v-btn>
    </v-form>
  </div>
</template>
<script>
export default {
  layout: "blank",
  beforeMount() {
    this.loadUser();
    console.log(this.users);
  },
  beforeUpdate() {
    this.loadUser();
  },
  data: () => ({
    /*Reglas para los campos*/
    valid: true,
    name: "",
    nameRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 40) || "",
    ],
    serviceRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 300 && v.length > 50) || "",
    ],
    select: null,
    typeProviderSelect: ["Proveedor formal", "proveedor informal"],
    checkbox: false,
    users: [],
    userProviders: [],
    userProvider: {
      /*Estos datos deberan ser almacenados en la bd junto con los nuevos datos que se adquieren en esta pagina*/
      /*fullname: null,
      email: null,
      password: null,
      entity: null,
      rol: null,*/
      id: null,
      nameCompany: null,
      typeProvider: null,
      serviceDescription: null,
    },
  }),

  methods: {
    loadUser() {
      let users = localStorage.getItem("users");
      this.users = JSON.parse(users);
    },

    finishFormProvider() {
      let exist = this.userProviders.find((x) => x.id == this.userProvider.id);
      if (exist == undefined) {
        this.userProviders.push(this.userProvider);
        localStorage.setItem(
          "userProviders",
          JSON.stringify(this.userProviders)
        );
      } else {
        alert("La persona que intenda crear ya existe");
      }
      console.log(this.userProviders);
      if (
        this.userProvider.id != null &&
        this.userProvider.nameCompany != null &&
        this.userProvider.typeProvider != null &&
        this.userProvider.serviceDescription != null
      ) {
        this.$router.push("/homeProvider");
      } else {
        alert("Llene todos los campos");
      }
    },
  },
};
</script>