<template>
  <div>
    <v-container>
      <v-text-field
        label="ID del servicio"
        type="number"
        style="width: 390px"
        required
      ></v-text-field>

      <v-btn
        color="primary"
        style="width: 300px; height: 40px; margin-left: 0px; margin-top: 16px"
        @click="mostrarDialgPuntuc()"
      >
        Seleccionar
      </v-btn>
    </v-container>

    <v-card
      class="elevation-16 mx-auto"
      width="300"
      heigth="350"
      v-model="toPuntuaction"
    >
      <v-card-title class="headline"> Puntuación de servicio </v-card-title>
      <v-card-text>
        Señor Cliente, en este espacio podrá calificar el servicio seleccionado
        realizado por un proveedor en especifico, por favor seleccione la
        cantidad de estrellas que considere corresponden al puntaje de dicho
        servicio, y si lo desea puede dejar un comentario de Retroalimentación

        <div class="text-center mt-12">
          <v-rating
            color="primary"
            empty-icon="mdi-star-outline"
            full-icon="mdi-star"
            half-icon="mdi-star-half-full"
            half-increments
            hover
            length="5"
            size="35"
            value="3.5"
          ></v-rating>
        </div>
      </v-card-text>
      <v-divider></v-divider>
      <v-textarea
        :counter="300"
        class="px-md-6 mx-lg-auto"
        label="Retroalimentación del servicio"
      >
      </v-textarea>
      <v-card-actions class="justify-space-between">
        <v-btn text> Cancelar </v-btn>
        <v-btn color="primary" text> Puntuar </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  layout: "defaultClient",
  data() {
    return {
      toPuntuaction: false,
    };
  },
  methods: {
    mostrarDialgPuntuc() {
      this.toPuntuaction = true;
    },
  },
};
</script>

<style>
</style>