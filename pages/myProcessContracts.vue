<template>
  <div>
    <h1>Contratos disponibles</h1>
    <v-file-input accept="image/*" label="Contrato"></v-file-input>
    <v-btn color="accent" @click="accept()"> Aceptar </v-btn>
    <v-btn color="accent" @click="denied()"> Rechazar </v-btn>
  </div>
</template>

<script>
export default {
  layout: "defaultClient",
  beforeMount() {
    this.loadInfo();
  },
  data() {
    return {
      contracts: [],
      contract: {
        idContract: null,
        idService: null,
        idProvider: null,
        idClient: null,

        nameFile: null,
        pathFile: null,

        isTotalAceptedProvider: null,
        isTotalAceptedClient: null,

        isAceptedProvider: null,
        isAceptedClient: null,
        dataFile: null,
      },
    };
  },
  methods: {
    loadInfo() {
      let contracts = localStorage.getItem("contracts");
      if (contracts != null) {
        this.contracts = JSON.parse(contracts);
      }
    },
    createContract() {},
    getContractUserOnline() {},
  },
};
</script>

