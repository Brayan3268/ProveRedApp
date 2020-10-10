<template>
  <div>
    <v-form ref="form" v-model="valid" lazy-validation>

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
        input type = "number"
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
        label="¿Esta de acuerdo?"
        class="px-md-6 mx-lg-auto"
        required
      ></v-checkbox>

      <v-btn :disabled="!valid" class="mr-4" @click="createUser">
        Registrarse
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
  }),

  methods: {
    createUser() {
      let exist = this.users.find((x) => x.id == this.user.id);
      if (exist) {
        this.users.push(this.user);
      }

      /*Validacion para que no permita ingresar con campos vacios*/

      if(this.user.fullname != null && this.user.id != null && this.user.email != null && this.user.password != null &&
          this.user.entity != null && this.user.rol != null){
        if (this.user.rol == "Proveedor") { 
          this.$router.push("/formProvider");
        } else {
          this.$router.push("/homeClient");
        }
      }else{
        alert("Llene todos los campos")
      }
      console.log(this.users);
    },
  },
};
</script>