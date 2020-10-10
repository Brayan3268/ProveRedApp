<template>
  
  <div>
    <h2>Para continuar con el proceso de creacion de cuenta, por favor 
      rellene los siguientes campos de acuerdo a su informacion como proveedor</h2>

    <!-- Se crea el form de proveedor -->

    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="user.id"
        :counter="40"
        :rules="nameRules"
        label="Documento de identidad"
        input type = "number"
        class="mt-md-6 px-md-6"
        required
      ></v-text-field>

      <v-text-field
        v-model="user.nameCompany"
        :counter="40"
        :rules="nameRules"
        label="Nombre de la empresa"
        class="px-md-6 mx-lg-auto"
        required
      ></v-text-field>  

      <v-select
        v-model="user.typeProvider"
        :items="typeProviderSelect"
        :rules="[(v) => !!v || 'Item is required']"
        label="Tipo de proveedor"
        class="px-md-6 mx-lg-auto"
        required
      ></v-select>

      <!-- Aqui el proveedor va a dar detalles sobre su servicio o servicios -->
      <v-textarea
        v-model="user.serviceDescription"
        :counter="300"
        :rules="serviceRules"
        class="px-md-6 mx-lg-auto"
        label="Describa su servicio(s)">
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
    user: {
      /*Estos datos deberan ser almacenados en la bd junto con los nuevos datos que se adquieren en esta pagina*/
      /*fullname: null,
      id: null,
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
    finishFormProvider() {
      let exist = this.users.find((x) => x.id == this.user.id);
      if (exist) {
        this.users.push(this.user);
      }

      if(this.user.id != null && this.user.nameCompany != null && this.user.typeProvider != null &&
          this.user.serviceDescription != null){
        this.$router.push("/homeProvider");
      }else{
        alert("Llene todos los campos")
      }
      console.log(this.users);
    },
  },
};
</script>