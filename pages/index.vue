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
                  label="Correo"
                  prepend-icon="mdi-email-open"
                  type="email"
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
    console.log(this.users);
  },
  data: () => ({
    id: null,
    password: null,
    users: [],
  }),
  methods: {
    loadUsers() {
      let users = localStorage.getItem("users");
      this.users = JSON.parse(users);
    },
    login() {
      let user = this.users.find((x) => x.id == this.id);
      if (user != undefined) {
        if (this.password == user.password) {
          if (user.rol == "Proveedor") {
            this.$router.push("/homeProvider");
          } else {
            this.$router.push("/homeClient");
          }
        } else {
        }
      } else {
      }

      console.log(this.email, this.password);
    },
    goToRegister() {
      this.$router.push("/register");
    },
  },
};
</script>
