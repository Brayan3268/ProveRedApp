<template>
  <!-- Login -->
  <v-main>
    <v-container class="fill-height" fluid>
      <v-row align="center" justify="center">
        <v-col cols="12" sm="8" md="4">
          <v-card>
            <v-toolbar color="primary" dark flat>
              <v-toolbar-title>Red de proveedores</v-toolbar-title>
            </v-toolbar>
            <v-card-text>
              <v-form>
                <v-text-field
                  label="Cedula"
                  prepend-icon=""
                  type="number"
                  v-model="id"
                ></v-text-field>

                <v-text-field
                  label="Clave"
                  prepend-icon="mdi-lock"
                  type="password"
                  v-model="password"
                ></v-text-field>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn @click="login()" color="primary">Ingresar</v-btn>
              <v-btn @click="goToRegister()" color="primary">Registrarme</v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>

<script>
import homeClientVue from "./homeClient.vue";
export default {
  layout: "blank",
  beforeMount() {
    this.loadUsers();
    this.loadUsersProviders();
    console.log(this.users);
  },
  data: () => ({
    id: null,
    password: null,
    users: [],
    userProviders: [],
    onlineUserProvider: {
      fullname: null,
      id: null,
      email: null,
      password: null,
      cellphoneNumber: null,
      entity: null,
      rol: null,
      nameCompany: null,
      typeProvider: null,
      serviceDescription: null,
    },
    onlineUserClient: {},
  }),
  methods: {
    loadUsers() {
      let users = localStorage.getItem("users");
      if (users != null) {
        this.users = JSON.parse(users);
      }
    },
    loadUsersProviders() {
      let usersProvider = localStorage.getItem("userProviders");
      if (usersProvider != null) {
        this.userProviders = JSON.parse(usersProvider);
      }
    },
    login() {
      let user = this.users.find((x) => x.id == this.id);
      if (user != undefined) {
        if (this.password == user.password) {
          if (user.rol == "Proveedor") {
            let userProvider = this.userProviders.find((x) => x.id == this.id);
            this.onlineUserProvider = user;
            this.onlineUserProvider.nameCompany = userProvider.nameCompany;
            this.onlineUserProvider.typeProvider = userProvider.typeProvider;
            this.onlineUserProvider.serviceDescription =
              userProvider.serviceDescription;
            console.log(this.onlineUserProvider);
            localStorage.setItem(
              "onlineUserProvider",
              JSON.stringify(this.onlineUserProvider)
            );
            this.$router.push("/homeProvider");
          } else {
            this.onlineUserClient = user;
            localStorage.setItem(
              "onlineUserClient",
              JSON.stringify(this.onlineUserClient)
            );
            this.$router.push("/homeClient");
          }
        } else {
          alert("La contraseña no es correcta");
        }
      } else {
        alert("El usuario no tiene una cuenta");
      }

      console.log(this.id, this.password);
    },
    goToRegister() {
      this.$router.push("/register");
    },
    getOnlineUser(user) {},
  },
};
</script>
