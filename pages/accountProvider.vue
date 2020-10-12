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
        v-model="userOnline.fullname"
        :counter="40"
        :rules="nameRules"
        label="Nombre completo"
        class="mt-md-6 px-md-6"
        required
      ></v-text-field>

      <v-text-field
        v-model="userOnline.email"
        :rules="emailRules"
        label="E-mail"
        class="px-md-6 mx-lg-auto"
        required
      ></v-text-field>

      <v-text-field
        v-model="userOnline.password"
        :counter="10"
        :rules="nameRules"
        label="Contraseña"
        class="px-md-6 mx-lg-auto"
        required
      ></v-text-field>

      <v-select
        v-model="userOnline.typeProvider"
        :items="typeProviderSelect"
        :rules="[(v) => !!v || 'Item is required']"
        label="Tipo de proveedor"
        class="px-md-6 mx-lg-auto"
        required
      ></v-select>

      <v-select
        v-model="userOnline.entity"
        :items="entity"
        :rules="[(v) => !!v || 'Item is required']"
        label="Entidad"
        class="px-md-6 mx-lg-auto"
        required
      ></v-select>

      <v-text-field
        v-model="userOnline.nameCompany"
        :counter="40"
        :rules="nameRules"
        label="Nombre de la Empresa"
        class="mt-md-6 px-md-6"
        required
      ></v-text-field>

      <v-textarea
        v-model="userOnline.serviceDescription"
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
    this.loadInfo();
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
    usersProviders: [],
    user: {},
    userProvider: {},
    userOnline: {
      fullname: null,
      id: null,
      email: null,
      password: null,
      typeProvider: null,
      roll: null,
      entity: null,
      nameCompany: null,
      serviceDescription: null,
    },
  }),
  methods: {
    loadInfo() {
      let onlineUserProvider = localStorage.getItem("onlineUserProvider");
      let users = localStorage.getItem("users");
      let usersProvider = localStorage.getItem("userProviders");
      this.users = JSON.parse(users);
      this.usersProviders = JSON.parse(usersProvider);
      this.userOnline = JSON.parse(onlineUserProvider);
    },
    editUser() {
      let user = this.users.find((x) => x.id == this.userOnline.id);
      let userProvider = this.usersProviders.find(
        (x) => x.id == this.userOnline.id
      );

      if (user != undefined && userProvider != undefined) {
        let existIndex = this.users.findIndex(
          (x) => x.id == this.userOnline.id
        );
        let existIndexP = this.usersProviders.findIndex(
          (x) => x.id == this.userOnline.id
        );
        this.user = user;
        this.user.fullname = this.userOnline.fullname;
        this.user.email = this.userOnline.email;
        this.user.password = this.userOnline.password;
        this.user.typeProvider = this.userOnline.typeProvider;

        this.userProvider = userProvider;
        this.userProvider.nameCompany = this.userOnline.nameCompany;
        this.userProvider.typeProvider = this.userOnline.typeProvider;
        this.userProvider.serviceDescription = this.userOnline.serviceDescription;

        this.usersProviders.splice(existIndexP, 1, this.userProvider);
        this.users.splice(existIndex, 1, this.user);

        localStorage.setItem("users", JSON.stringify(this.users));
        localStorage.setItem(
          "userProviders",
          JSON.stringify(this.usersProviders)
        );
      }
    },
    sendInfo() {},
  },
};
</script>