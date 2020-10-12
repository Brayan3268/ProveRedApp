<template>
  <div>
    <v-container>
      <h1>Cuenta del proveedor</h1>

      <p>Subir documentos sobre mis servicios</p>
      <v-file-input
        truncate-length="15"
        label="Subir multimedia"
      ></v-file-input>

      <p>Adjuntar informacion</p>
      <v-file-input
        accept="image/png, image/jpeg, image/bmp"
        label="Fotos del producto o servicio"
        prepend-icon="mdi-camera"
      ></v-file-input>

      <v-btn @click="sendInfo">Subir informacion</v-btn>
    </v-container>

    <v-container>
      <v-text-field
        v-model="user.fullname"
        :counter="40"
        :rules="nameRules"
        label="Nombre completo"
        class="mt-md-6 px-md-6"
        required
      ></v-text-field>

      <v-text-field
        v-model="user.email"
        :rules="emailRules"
        label="E-mail"
        class="px-md-6 mx-lg-auto"
        required
      ></v-text-field>

      <v-text-field
        v-model="user.password"
        :counter="10"
        :rules="nameRules"
        label="Contraseña"
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

      <v-select
        v-model="user.entity"
        :items="entity"
        :rules="[(v) => !!v || 'Item is required']"
        label="Entidad"
        class="px-md-6 mx-lg-auto"
        required
      ></v-select>

      <v-text-field
        v-model="user.companyName"
        :counter="40"
        :rules="nameRules"
        label="Nombre de la Empresa"
        class="mt-md-6 px-md-6"
        required
      ></v-text-field>

      <v-textarea
        v-model="user.serviceDescription"
        :counter="300"
        :rules="serviceRules"
        class="px-md-6 mx-lg-auto"
        label="Describa su servicio(s)"
      ></v-textarea>

      <v-btn @click="editUser">Editar</v-btn>
    </v-container>
  </div>
</template>
<script>
export default {
  beforeMount() {
    this.loadUsers();
  },
  data: () => ({
    /*Reglas para los campos*/

    nameRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 40) || "",
    ],
    email: "",
    entity: ["Persona juridica", "Persona natural"],
    typeProviderSelect: ["Proveedor formal", "proveedor informal"],

    emailRules: [
      (v) => !!v || "E-mail es requerido",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    serviceRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 300 && v.length > 50) || "",
    ],
    users: [],
    user: {
      fullname: null,
      id: null,
      email: null,
      password: null,
      typeProvider: null,
      entity: null,
      companyName: null,
      serviceDescription: null,
    },
  }),
  methods: {
    loadUsers() {
      let users = localStorage.getItem("users");
      this.users = JSON.parse(users);
    },
    sendInfo() {},
    editUser(user) {},
  },
};
</script>