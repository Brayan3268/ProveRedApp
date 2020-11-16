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
          v-model="user.password_"
          :counter="10"
          :rules="nameRules"
          label="Contraseña"
          style="margin-left: 30px;"
          required
        ></v-text-field>

        <v-text-field
          v-model="user.cellphone"
          :counter="10"
          :rules="nameRules"
          label="Numero de celular"
          style="margin-left: 30px;"
          required
        ></v-text-field>
        </v-row>

        <v-row>
        <v-select
          v-model="user.typeprovider"
          :items="typeproviderSelect"
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
          sonrisa tu partida, así que te deseamos el mejor de los viajes.
        </p>

        <v-text-field
          v-model="user.iduser"
          label="Cedula"
          style="width: 200px;"
          disabled
          required
        ></v-text-field>

        <v-btn style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px" color="primary" @click="dialog = true"
          >Eliminar cuenta</v-btn
        >
      </v-container>
    </v-form>
    <v-dialog v-model="dialog" max-width="290" persistent>
      <v-card>
        <v-card-title class="headline"> Eliminacion de cuenta </v-card-title>

        <v-card-text> ¿Estas seguro de que quieres eliminar tú cuenta? </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="deleteAccountProvider1()"> Aceptar </v-btn>
          <v-btn color="primary" text @click="dialog = false"> Close </v-btn>
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
    typeproviderSelect: ["Proveedor formal", "Proveedor informal"],
    nameRules: [
      (v) => !!v || "El campo es requerido",
      (v) => (v && v.length <= 40) || "",
    ],

    cellphone: [
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
      password_: null,
      entity: null,
      rol: null,
      cellphone: null,
    },
    user: {
      fullname: null,
      id: null,
      email: null,
      password_: null,
      entity: null,
      cellphone: null,
      rol: null,
    },
    userProvider: {
      id: null,
      nameCompany: null,
      typeprovider: null,
      serviceDescription: null,
    },
    userOnline: {
      fullname: null,
      cellphone: null,
      id: null,
      email: null,
      password_: null,
      typeprovider: null,
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
      password_: null,
      typeprovider: null,
      entity: null,
      cellphone: null,
      companyName: null,
      serviceDescription: null,
    },
    /*La razón por la cual el usuario va a eliminar su cuenta */
    userProviders2: [],
    userProvider2: {
      id: null,
      nameCompany: null,
      typeprovider: null,
      serviceDescription: null,
      cellphone: null,
    },
  }),
  methods: {
    //carga los usuarios almacenados localmente y convierte sus datos a JSON
    loadUsers() {
      let users2 = localStorage.getItem("users");
      this.users2 = JSON.parse(users2);

      let userProviders2 = localStorage.getItem("userProviders");
      this.userProviders2 = JSON.parse(userProviders2);
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
          this.temp.cellphone = this.user.cellphone;
          this.temp.fullname = this.user.fullname;
          this.temp.email = this.user.email;
          this.temp.password_ = this.user.password_;
          this.temp.entity = this.user.entity;
          this.temp.rol = this.user.rol;

          this.userProvider.id = this.user.id;
          this.userProvider.cellphone = this.user.cellphone;
          this.userProvider.nameCompany = this.user.nameCompany;
          this.userProvider.typeprovider = this.user.typeprovider;
          this.userProvider.serviceDescription = this.user.serviceDescription;

          this.userOnline.id = this.user.id;
          this.userOnline.fullname = this.user.fullname;
          this.userOnline.email = this.user.email;
          this.userOnline.cellphone = this.user.cellphone;
          this.userOnline.password_ = this.user.password_;
          this.userOnline.entity = this.user.entity;
          this.userOnline.rol = this.user.rol;
          this.userOnline.nameCompany = this.user.nameCompany;
          this.userOnline.typeprovider = this.user.typeprovider;
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
      const url = "http://localhost:3001/api/v2/users/" + this.user.iduser;
      let token = localStorage.getItem("token");
      this.$axios.setToken(token, "Bearer");
      this.$axios.delete(url)
      .then(res => {
        console.log(res);
        //this.dialog = true;
        goToMenu();
      })
      .catch(err => {
        console.error(err);
        this.dialog2 = true;
      })
    },
    sendInfo() {},
    goToMenu() {
      this.$router.push("/");
      localStorage.setItem("onlineUserProvider", {});
    },
  },
};
</script>