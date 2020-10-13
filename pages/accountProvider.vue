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

    <v-container>
      <h1>!ELIMINAR MI CUENTA¡</h1>

      <p>
        En esta seccion podras eliminar tu cuenta de proveedor, nos borra la
        sonrisa tu partida, así que antes te pedimos que nos cuentes porque te
        vas y te deseamos el mejor de los viajes.
      </p>

      <v-text-field
        v-model="reasonForDeleteAccount.id"
        label="Cedula"
        class="px-md-6 mx-lg-auto"
        disabled
        required
      ></v-text-field>

      <v-textarea
        v-model="reasonForDeleteAccount.reason"
        :counter="300"
        :rules="serviceRules"
        class="mt-md-6 px-md-6"
        label="Cuentanos tus motivos"
      ></v-textarea>

      <v-btn class="mt-md-6 px-md-6" @click="deleteAccountProvider1()"
        >Eliminar cuenta</v-btn
      >
    </v-container>

    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-title class="headline"> Eliminacion de cuenta </v-card-title>

        <v-card-text> Cuenta eliminada con exito, muchas gracias </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="accent" text @click="dialog = false"> Close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialog2" max-width="290">
      <v-card>
        <v-card-title class="headline"> Eliminacion de cuenta </v-card-title>

        <v-card-text>
          Ocurrio un error a la hora de eliminar la cuenta, intentalo de nuevo
          más tarde
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="accent" text @click="dialog2 = false"> Close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  beforeMount() {
    this.loadInfo();
    this.reasonForDeleteAccount.id = this.userOnline.id;
    this.loadUsers();
  },

  beforeUpdate() {
    this.loadUsers();
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
      (v) => (v && v.length <= 300 && v.length > 10) || "",
    ],

    dialog: null,
    dialog2: null,

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

    users2: [],
    user2: {
      fullname: null,
      id: null,
      email: null,
      password: null,
      typeProvider: null,
      entity: null,
      companyName: null,
      serviceDescription: null,
    },
    /*La razón por la cual el usuario va a eliminar su cuenta */
    reasonsForDeleteAccounts: [],
    reasonForDeleteAccount: {
      id: null,
      reason: null,
    },
    userProviders2: [],
    userProvider2: {
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
    loadUsers() {
      let users2 = localStorage.getItem("users");
      this.users2 = JSON.parse(users2);

      let userProviders2 = localStorage.getItem("userProviders");
      this.userProviders2 = JSON.parse(userProviders2);

      let reasonsForDeleteAccounts = localStorage.getItem(
        "reasonsForDeleteAccounts"
      );
      if (reasonsForDeleteAccounts != null) {
        this.reasonsForDeleteAccounts = JSON.parse(reasonsForDeleteAccounts);
      }
    },
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
    deleteAccountProvider1() {
      debugger
      let id = this.reasonForDeleteAccount.id;
      let userProvSave = null;
      let userSave = null;
      if (id.length >= 1) {
        this.reasonsForDeleteAccounts.push(this.reasonForDeleteAccount);
        localStorage.setItem(
          "reasonsForDeleteAccounts",
          JSON.stringify(this.reasonsForDeleteAccounts)
        );

        let users2 = localStorage.getItem("users");
        users2 = JSON.parse(users2);
        userSave = users2;
        console.log(users2);
        let posUser = users2.findIndex((x) => x.id == id);
        console.log(posUser);
        let userDelete = users2.splice(posUser, 1);
        console.log(userDelete);
        localStorage.setItem("users", JSON.stringify(users2));

        let userProvider2 = localStorage.getItem("userProviders");
        userProvider2 = JSON.parse(userProvider2);
        userProvSave = userProvider2;
        console.log(userProvider2);
        let posUserProvider = userProvider2.findIndex((x) => x.id == id);
        console.log(posUserProvider);
        let userProviderDelete = userProvider2.splice(posUserProvider, 1);
        console.log(userProviderDelete);
        localStorage.setItem("userProviders", JSON.stringify(userProvider2));

        if(userSave.length > users2 && userProvSave.length > userProvider2) {
          this.dialog = true;
          this.$router.push("/");
        } else {
          this.dialog2 = true;
        }
        /*Esta validacion de posUser == userProvider2 solo sirve en el local storage, no en ningun otro lado */

        let posUser2 = users2.findIndex((x) => x.id == id);
        console.log(posUser2);
        let posUserProvider2 = userProvider2.findIndex((x) => x.id == id);
        console.log(posUserProvider2);
        /*if(posUser2 == -1 && posUserProvider2 == -1){
          localStorage.setItem("onlineUserProvider", {});
          this.$router.push("/");
          //this.dialog = true;
        }*/
      }
    },
    sendInfo() {},
  },
};
</script>