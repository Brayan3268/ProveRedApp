<template>
<!--permite visualizar los campos relacionados con el perfil del proveedor, subir o adjuntar multimedia relacionada, y asi mismo poder actualizarlo-->
  <div>
    <v-form ref="formEdit" v-model="formEdit" lazy-validation>
      <v-container >
        <h1>Cuenta del proveedor</h1>

        <v-row>
        <p>Subir documentos sobre mis servicios</p>
        
        <p
        style="margin-left: 290px;">Adjuntar informacion</p>
        </v-row>

        <v-row>
        <v-file-input
          truncate-length="15"
          label="Subir multimedia"
          style="width: 410px; height: 50px;"
        ></v-file-input>

        <v-file-input
          accept="image/png, image/jpeg, image/bmp"
          label="Fotos del producto o servicio"
          prepend-icon="mdi-camera"
          style="width: 500px; height: 50px; margin-left: 40px;"
        ></v-file-input>
        </v-row>
      
        <v-btn @click="sendInfo()" style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px" color="primary">Subir ambos archivos </v-btn>
      </v-container>

      <v-container style="margin-top: 50px">
        <v-row>
        <v-text-field
          v-model="user.fullname"
          :counter="40"
          :rules="nameRules"
          label="Nombre completo"
          style="margin-left: 30px;"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.email"
          :rules="emailRules"
          label="E-mail"
          style="margin-left: 30px;"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.password"
          :counter="10"
          :rules="nameRules"
          label="Contraseña"
          style="margin-left: 30px;"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.cellphoneNumber"
          :counter="10"
          :rules="nameRules"
          label="Numero de celular"
          style="margin-left: 30px;"
          required
        ></v-text-field>
        </v-row>

        <v-row>
        <v-select
          v-model="user.typeProvider"
          :items="typeProviderSelect"
          :rules="[(v) => !!v || 'Item is required']"
          label="Tipo de proveedor"
          class="px-md-6 mx-lg-auto"
          style="width: 300px; margin-left: 0px;"
          required
        ></v-select>

        <v-select
          v-model="user.entity"
          :items="entity"
          :rules="[(v) => !!v || 'Item is required']"
          label="Entidad"
          class="px-md-0"
          style="width: 80px; margin-left: 30px;" 
          required
        ></v-select>

        <v-text-field
          v-model="user.nameCompany"
          :counter="40"
          :rules="nameRules"
          label="Nombre de la Empresa"
          style="width: 80px; margin-left: 30px;"
          required
        ></v-text-field>
        </v-row>

        <v-textarea
          v-model="user.serviceDescription"
          :counter="300"
          :rules="serviceRules"
          label="Describa su servicio(s)"
          style="width: 1000px; margin-left: 12px;"
        ></v-textarea>

        <v-btn style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px" color="primary" @click="editUser()">Editar</v-btn>
      </v-container>
    </v-form>

    <v-form style="margin-top: 50px" ref="formDialog" v-model="formDialog" lazy-validation>
      <v-container>
        <h1>!ELIMINAR MI CUENTA¡</h1>

        <p style="width: 970px;">
          En esta seccion podras eliminar tu cuenta de proveedor, nos borra la
          sonrisa tu partida, así que antes te pedimos que nos cuentes porque te
          vas y te deseamos el mejor de los viajes.
        </p>

        <v-text-field
          v-model="reasonForDeleteAccount.id"
          label="Cedula"
          style="width: 200px;"
          disabled
          required
        ></v-text-field>

        <v-textarea
          v-model="reasonForDeleteAccount.reason"
          :counter="300"
          :rules="serviceRules"
          style="width: 1000px;"
          label="Cuentanos tus motivos"
        ></v-textarea>

        <v-btn style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px" color="primary" @click="deleteAccountProvider1()"
          >Eliminar cuenta</v-btn
        >
      </v-container>
    </v-form>
    <v-dialog v-model="dialog" max-width="290" persistent>
      <v-card>
        <v-card-title class="headline"> Eliminacion de cuenta </v-card-title>

        <v-card-text> Cuenta eliminada con exito, muchas gracias </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="primary" text @click="goToMenu()"> Close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialog2" max-width="290" persistent>
      <v-card>
        <v-card-title class="headline"> Eliminacion de cuenta </v-card-title>

        <v-card-text>
          Ocurrio un error a la hora de eliminar la cuenta, intentalo de nuevo
          más tarde
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="primary" text @click="dialog2 = false"> Close </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>

//verifica que los metodos estén correctos antes de cargar, subir o actualizar la información
export default {
  beforeMount() {
    this.loadInfo();
    this.reasonForDeleteAccount.id = this.userOnline.id;
    this.loadUsers();
    //console.log(this.userOnline);
  },

  data: () => ({
    /*Reglas para los campos*/
    formEdit: null,
    formDialog: null,
    nameRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 40) || "",
    ],
    email: "",
    entity: ["Persona juridica", "Persona natural"],
    typeProviderSelect: ["Proveedor formal", "proveedor informal"],
    nameRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 40) || "",
    ],

    cellphoneNumber: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length == 10) || "",,
    ],

    emailRules: [
      (v) => !!v || "E-mail es requerido",
      (v) => /.+@.+\..+/.test(v) || "ingrese un E-mail valido",
    ],
    serviceRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 300 && v.length > 10) || "",
    ],

    dialog: false,
    dialog2: false,

    users: [],
    usersProviders: [],
    temp: {
      fullname: null,
      id: null,
      email: null,
      password: null,
      entity: null,
      rol: null,
      cellphoneNumber: null,
    },
    user: {
      fullname: null,
      id: null,
      email: null,
      password: null,
      entity: null,
      cellphoneNumber: null,
      rol: null,
    },
    userProvider: {
      id: null,
      nameCompany: null,
      typeProvider: null,
      serviceDescription: null,
    },
    userOnline: {
      fullname: null,
      cellphoneNumber: null,
      id: null,
      email: null,
      password: null,
      typeProvider: null,
      rol: null,
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
      cellphoneNumber: null,
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
      cellphoneNumber: null,
    },
  }),
  methods: {
    //carga los usuarios almacenados localmente y convierte sus datos a JSON
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
      this.user = this.userOnline;
    },

    editUser() {
      if (this.$refs.formEdit.validate() && this.formEdit) {
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

          this.temp.id = this.user.id;
          this.temp.cellphoneNumber = this.user.cellphoneNumber;
          this.temp.fullname = this.user.fullname;
          this.temp.email = this.user.email;
          this.temp.password = this.user.password;
          this.temp.entity = this.user.entity;
          this.temp.rol = this.user.rol;

          this.userProvider.id = this.user.id;
          this.userProvider.cellphoneNumber = this.user.cellphoneNumber;
          this.userProvider.nameCompany = this.user.nameCompany;
          this.userProvider.typeProvider = this.user.typeProvider;
          this.userProvider.serviceDescription = this.user.serviceDescription;

          this.userOnline.id = this.user.id;
          this.userOnline.fullname = this.user.fullname;
          this.userOnline.email = this.user.email;
          this.userOnline.cellphoneNumber = this.user.cellphoneNumber;
          this.userOnline.password = this.user.password;
          this.userOnline.entity = this.user.entity;
          this.userOnline.rol = this.user.rol;
          this.userOnline.nameCompany = this.user.nameCompany;
          this.userOnline.typeProvider = this.user.typeProvider;
          this.userOnline.serviceDescription = this.user.serviceDescription;

          this.usersProviders.splice(existIndexP, 1, this.userProvider);
          this.users.splice(existIndex, 1, this.temp);

          localStorage.setItem("users", JSON.stringify(this.users));
          localStorage.setItem(
            "userProviders",
            JSON.stringify(this.usersProviders)
          );
          localStorage.setItem(
            "onlineUserProvider",
            JSON.stringify(this.userOnline)
          );
          alert("Se edito correctamente");
        }
      } else {
        alert("ingrese todos los campos");
      }
    },
    // elimina la cuenta del proveedor, guardando su razón para eliminar la cuenta
    deleteAccountProvider1() {

      /**Eliminar los servicios del proveedor al eliminar el proveedor*/

      if (this.$refs.formDialog.validate() && this.formDialog) {
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

          if (userSave.length > users2 && userProvSave.length > userProvider2) {
            this.dialog2 = true;
          } else {
            this.dialog = true;
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
      }
    },
    sendInfo() {},
    goToMenu() {
      this.$router.push("/");
      localStorage.setItem("onlineUserProvider", {});
    },
  },
};
</script>