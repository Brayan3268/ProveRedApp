<template>
  <div>
    <v-form ref="formRegister" v-model="formRegister" lazy-validation>
      <!-- Se crea el form de usuario -->

      <v-text-field
        v-model="user.fullname"
        :counter="40"
        :rules="nameRules"
        label="Nombre completo"
        class="mt-md-6 px-md-6"
        required
      ></v-text-field>

      <v-text-field
        v-model="user.id"
        :counter="40"
        :rules="nameRules"
        label="Documento de identidad"
        input
        class="px-md-6 mx-lg-auto"
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
        v-model="user.entity"
        :items="entity"
        :rules="[(v) => !!v || 'Item is required']"
        label="Entidad"
        class="px-md-6 mx-lg-auto"
        required
      ></v-select>

      <v-select
        v-model="user.rol"
        :items="rol"
        :rules="[(v) => !!v || 'Item is required']"
        label="Rol"
        class="px-md-6 mx-lg-auto"
        required
      ></v-select>

      <v-checkbox
        v-model="checkbox"
        :rules="[(v) => !!v || 'You must agree to continue!']"
        label="¿Esta de acuerdo con los terminos y condiciones?"
        class="px-md-6 mx-lg-auto"
        required
      ></v-checkbox>

      <v-btn class="mr-4" @click="createUser"> Registrarse </v-btn>
    </v-form>

    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-title class="headline"> Error </v-card-title>

        <v-card-text> Llene todos los campos </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="accent" text @click="dialog = false"> Close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  layout: "blank",
  beforeMount() {
    this.loadUsers();
  },
  data: () => ({
    /*Reglas para los campos*/
    valid: true,
    formRegister: null,
    name: "",
    nameRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 40) || "",
    ],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail es requerido",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    select: null,
    entity: ["Persona juridica", "Persona natural"],
    rol: ["Proveedor", "Cliente"],
    checkbox: false,
    users: [],
    user: {
      fullname: null,
      id: null,
      email: null,
      password: null,
      entity: null,
      rol: null,
    },
    dialog: false,
  }),

  methods: {
    loadUsers() {
      let users = localStorage.getItem("users");
      if (users != null) {
        this.users = JSON.parse(users);
      }
      console.log(this.users);
    },
    createUser() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        let exist = this.users.find((x) => x.id == this.user.id);
        if (exist == undefined) {
          this.users.push(this.user);
          localStorage.setItem("users", JSON.stringify(this.users));
          if (this.user.rol == "Proveedor") {
            this.$router.push("/formProvider");
          } else {
            this.$router.push("/homeClient");
          }
        } else {
          alert("La persona que intenda crear ya esta en la tabla");
        }
      } else {
        this.dialog = true;
      }
      console.log(this.users);
    },
  },
};
</script>