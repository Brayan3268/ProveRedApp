<template>
<div style="width: 30; ">
    <v-container v-if="!isInProcessContract" style="width: 30;">
    <v-text-field 
        v-model="contract.idContract"
        label="ID del contrato"
        type = "number"
        style="width: 390px;"
        required
    ></v-text-field>

    <v-btn 
      color="primary" 
      style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px" 
      @click="searchContractInProcess()">
      Buscar
    </v-btn>

    </v-container>
    <!-- QUITAR LA NEGACION DE EL CONTAINER DE ABAJO AL MOMENTO DE PODER TRABAJAR CON CONTRATOS QUE VENGAN DEL LOCALSTORAGE -->
    <v-container v-if="!isInProcessContract">
      <h1>Mis contratos en proceso</h1>

      <p>Subir documento de el proceso (contrato)</p>
      <v-file-input
        v-model="contract.dataFile"
        truncate-length="15"
        label="Subir pdf"
        accept =".pdf"
        style="width: 410px; height: 50px;"
      ></v-file-input>

      <v-container v-if="!negotiateInProcess">
        <v-btn color="primary" @click="acceptedContract()" v-if="documentsUp">Aceptar contrato</v-btn>
        <v-btn color="primary" @click="rejectedContract()" v-if="documentsUp">Rechazar contrato</v-btn>
    
        <v-btn color="primary" style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px"  @click="upDocuments()" v-else>Subir documento</v-btn>
      </v-container>
      
      <v-btn color="primary" @click="negotiateContract()" v-if="negotiateInProcess">Negociar contrato</v-btn>
      <v-btn color="primary" @click="confirmRejectedContract()" v-if="negotiateInProcess">Comfirmar Rechazar contrato</v-btn>

    </v-container>

    <div>
    <v-dialog v-model="dialog" max-width="360" persistent>
      <v-card>
        <v-card-title class="headline"> Sin contratos en proceso </v-card-title>
        <v-card-text> No tiene contratos en proceso por el momento </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="dialog = false" color="primary"> Aceptar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialog2" max-width="460" persistent>
      <v-card>
        <v-card-title class="headline"> Contrato anteriormente aceptado </v-card-title>
        <v-card-text> Usted ya había aceptado el contrato anteriormente, 
            por favor espere la respuesta del cliente </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog2 = false"> Aceptar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogIsWantAccepted" max-width="360" persistent>
      <v-card>
        <v-card-title class="headline"> ¿Estás seguro? </v-card-title>
        <v-card-text> ¿Está seguro que desea aceptar este contrato? NO habrá vuelta atras;
            (El cliente aún no ha respuesto sobre este contrato). </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="accepted()"> Aceptar </v-btn>
          <v-btn color="primary" text @click="dialogIsWantAccepted = false"> Cancelar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialog3" max-width="360" persistent>
      <v-card>
        <v-card-title class="headline"> ¿Estás seguro? </v-card-title>
        <v-card-text> ¿Está seguro que desea aceptar este contrato? NO habrá vuelta atras;
            (El cliente ya ha aceptado este contrato). </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="accepted()"> Aceptar </v-btn>
          <v-btn color="primary" text @click="dialog3 = false"> Cancelar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogTotalRejected" max-width="360" persistent>
      <v-card>
        <v-card-title class="headline"> ¿Estás seguro? </v-card-title>
        <v-card-text> ¿Está seguro que desea rechazar este contrato? NO habrá vuelta atras;
            (El cliente ya ha aceptado este contrato). </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="rejected()"> Aceptar </v-btn>
          <v-btn color="primary" text @click="dialogTotalRejected = false"> Cancelar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogIsWantRejected" max-width="360" persistent>
      <v-card>
        <v-card-title class="headline"> ¿Estás seguro? </v-card-title>
        <v-card-text> ¿Está seguro que desea rechazar este contrato? NO habrá vuelta atras;
            (El cliente aún no ha respuesto sobre este contrato). </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="rejected()"> Aceptar </v-btn>
          <v-btn color="primary" text @click="dialogIsWantRejected = false"> Cancelar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogRejected2" max-width="460" persistent>
      <v-card>
        <v-card-title class="headline"> Contrato anteriormente rechazado </v-card-title>
        <v-card-text> Usted ya había rechazado el contrato anteriormente, 
            por favor espere la respuesta del cliente </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialogRejected2 = false"> Aceptar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialogRejected3" max-width="360" persistent>
      <v-card>
        <v-card-title class="headline"> ¿Estás seguro? </v-card-title>
        <v-card-text> ¿Está seguro que desea rechazar este contrato? NO habrá vuelta atras;
            (El cliente ya ha rechazado este contrato). </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="rejected()"> Aceptar </v-btn>
          <v-btn color="primary" text @click="dialogRejected3 = false"> Cancelar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="negotiationContractDialog" max-width="360" persistent>
      <v-card>
        <v-card-title class="headline"> Hablale </v-card-title>
        <v-card-text>Dirigite al cliente y dile que dialogen de nuevo sobre los terminos para intentar
            llegar a un nuevo acuerdo
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="negotiationContractDialog = false"> Aceptar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    </div>

    </div>
</template>
<script>
export default {
  beforeMount() {
    this.loadInfo();
    /** HACER QUE CUANDO SE ENTRE A ESTA PESTAÑA SE MIRE SI TIENE ALGUNA NOTIFICACION PENDIENTE */
  },

  beforeUpdate() {
  },
  data: () => ({
    //class="mt-md-6 px-md-6"
    contracts: [],
    contract: {
        idContract: null,
        idService: null,
        idProvider: null,
        idClient: null,

        dataFile: null,
        nameFile: null,
        pathFile: null,

        isTotalAceptedProvider: null,
        isTotalAceptedClient: null,

        isProviderNotified: null,
        isClientNotified: null,
    },

    contractsActivates: [],
    contractActivate: {
        idContract: null,
        idService: null,
        idProvider: null,
        idClient: null,

        dataFile: null,
        nameFile: null,
        pathFile: null,

        description: null,
        finDate: null,
        initDate: null,
        total: null,
    },

    dialog: false,
    dialog2: false,
    dialog3: false,
    dialogIsWantAccepted: false,
    dialogTotalRejected: false,
    dialogIsWantRejected: false,
    dialogRejected2: false,
    dialogRejected3: false,
    negotiationContractDialog: false,

    isInProcessContract: false,
    documentsUp: false,
    negotiateInProcess: false,
}),
methods: {
    loadInfo() {
        //debugger
        let contracts = localStorage.getItem("contracts");
        if (contracts != null) { 
            this.contracts = JSON.parse(contracts);   
        }else{
            this.dialog = true;
        }
    },
    searchContractInProcess(){
        if (this.contracts != null) { 
            this.dialog = true;   
        }else{
            try{
                if(this.contract.idContract != null || this.contract.idContract != undefined){
                    let idContract = this.contract.idContract;
                    for(var i = 0; i < this.contracts.lenght; i++){
                        if(idContract == this.contract.idContract){
                            this.isInProcessContract = true;
                        }
                    }
                }else{
                    alert("Llene el campo por favor");
                }
            }catch(e){
                //handleError(e, vnode.context, ("directive " + (dir.name) + " " + hook + " hook"));
                alert(e);
            }
        }
    },
    upDocuments(){
        if(this.contract.dataFile != null){
            let dataFile = this.contract.dataFile;
            this.contract.nameFile = dataFile.name;
            (dataFile.webkitRelativePath == "") ? this.contract.pathFile = " - isEmpty - " : this.contract.pathFile = dataFile.webkitRelativePath;
            this.documentsUp = true;
        }else{
            alert("Suba el documento pertinenete por favor");
        }
    },
    acceptedContract(){
        if(this.contract.isTotalAceptedProvider == null && this.contract.isTotalAceptedClient == null){
            this.dialogIsWantAccepted = true;
        }else{
            if(this.contract.isTotalAceptedProvider == true && this.contract.isTotalAceptedClient == null){
                this.dialog2 = true;
            }else{
                if( this.contract.isTotalAceptedProvider == null && this.contract.isTotalAceptedClient == true){
                    this.dialog3 = true;
                }
            }
        }
    },
    rejectedContract(){
        
        this.negotiateInProcess = true;
        //this.isInProcessContract = true;
    },
    negotiateContract(){
        //this.negotiationContractDialog = true;
        alert("Dirigite al cliente y dile que dialogen de nuevo sobre los terminos para intentar llegar a un nuevo acuerdo");
        this.$router.push("/homeProvider");
    },
    confirmRejectedContract(){
        if(this.contract.isTotalAceptedProvider == null && this.contract.isTotalAceptedClient == null){
            this.dialogIsWantRejected = true;
        }else{
            if(this.contract.isTotalAceptedProvider == false && this.contract.isTotalAceptedClient == null){
                this.dialogRejected2 = true;
            }else{
                if( this.contract.isTotalAceptedProvider == null && this.contract.isTotalAceptedClient == false){
                    this.dialogRejected3 = true;
                }
            }
        }
        //this.dialogTotalRejected = true;
    },
    accepted(){
        alert("El contrato fue aceptado con exito");
        //console.log("answer: ", this.contract.isTotalAceptedProvider);  
        this.contract.isTotalAceptedProvider = true;
        //console.log("answer: ", this.contract.isTotalAceptedProvider);

        this.dialogIsWantAccepted = false;
        if(this.contract.isTotalAceptedProvider == true && this.contract.isTotalAceptedClient == true){
            /**AQUI VA LA PARTE DEL CODIGO EN LA QUE PASA A CONTRATO ACTIVO Y SE NOTIFICA AL CLIENTE*/
        }
    },
    rejected(){
        alert("El contrato fue rechazado con exito");
        
        this.contract.isTotalAceptedProvider = false;
        this.dialogIsWantRejected = false;
        
        if(this.contract.isTotalAceptedProvider == false && this.contract.isTotalAceptedClient == false){
            /**AQUI VA LA PARTE DEL CODIGO EN LA QUE SE ELIMINA EL CONTRATO DESPUES DE NOTIFICAR AL CLIENTE*/
        }
    },
},
};
</script>