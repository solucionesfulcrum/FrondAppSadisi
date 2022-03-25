<template>
  <div class="row">
    <v-row justify="center">
      <v-overlay :value="overlay">
        <v-progress-circular indeterminate size="64"></v-progress-circular>
      </v-overlay>
    </v-row>
    <div class="col-md-11 mt-5 mr-5">
      <v-system-bar color="#1973a5" dark>
        Reporte de bienes tecnologicos y usuarios
      </v-system-bar>
      <v-card elevation="2" outlined>
        <div class="row mt-2">
          <v-col cols="12" md="3" class="ml-5">
            <v-combobox
              v-model="setTipoDoc"
              filled
              outlined
              :items="tipoDoc"
              solo
            ></v-combobox>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field
              v-model="setNumDocu"
              :rules="nameRules"
              :counter="8"
              label="Número de documento"
              required
              type="number"
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="1">
            <v-btn class="" icon color="#1973a5" @click="buscarBienes">
              <v-icon>mdi-magnify</v-icon>
            </v-btn>
          </v-col>
          <v-col cols="12" md="6" class="ml-5">
            <v-autocomplete
              filled
              :items="listaPersonal"
              label="Buscar Personal por Nombre"
              solo
            ></v-autocomplete>
          </v-col>
          <v-col cols="12" md="1">
            <v-btn class="" icon color="#1973a5" @click="buscarBienes">
              <v-icon>mdi-magnify</v-icon>
            </v-btn>
          </v-col>
        </div>
      </v-card>
      <v-card class="mt-3">
        <v-system-bar color="#1973a5" dark> Lista de resultados </v-system-bar>
        <v-card-title>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table :headers="headers" :items="desserts" :search="search">
        </v-data-table>
      </v-card>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export const RUTA_SERVIDOR = process.env.VUE_APP_RUTA_API;
export default {
  name: "Home",
  components: {},
  data() {
    return {
      datoscheck: [],
      overlay: false,
      //data de datatabble
      search: "",
      headers: [
        {
          text: "Num Doc",
          align: "start",
          value: "datosPersonal.dniPer",
        },
        { text: "Ape. Pat.", value: "datosPersonal.apePatPer" },
        { text: "Ape. Mat.", value: "datosPersonal.apeMatPer" },
        { text: "Nombres", value: "datosPersonal.nomPer" },
        { text: "Cod", value: "datos_bienpat.codEti" },
        { text: "Descripcion", value: "datos_bienpat.desBien" },
      ],
      desserts: [],
      //
      setTipoDoc: "DNI",
      setNumDocu: "46184379",
      valid: false,
      nameRules: [
        (v) => !!v || "Número requerido",
        (v) => v.length <= 8 || "8 caracteres como maximo",
      ],
      tipoDoc: ["DNI", "Carnet Ext.", "Pasaporte"],
      listaPersonal: ["nombre1", "nombre2", "nombre3"],
      dialog: false,
    };
  },
  methods: {
    agregarPersonal() {
      console.log("agregando ando");
      this.dialog = true;
    },
    filterOnlyCapsText(value, search, item) {
      return (
        value != null &&
        search != null &&
        typeof value === "string" &&
        value.toString().toLocaleUpperCase().indexOf(search) !== -1
      );
    },

    buscarBienes() {
      if (this.setNumDocu != "") {
        this.overlay = true;
        axios
          .post(RUTA_SERVIDOR + "/api/token/", {
            username: "cnsr",
            password: "123456",
          })
          .then((response) => {
            this.auth = "Bearer " + response.data.access;
            axios
              .get(RUTA_SERVIDOR + "/bienPersonal/?search=" + this.setNumDocu, {
                headers: { Authorization: this.auth },
              })
              .then((res) => {
                console.log(res.data);
                this.desserts = res.data;
                this.overlay = false;
              })
              .catch((res) => {
                console.warn("Error:", res);
                this.overlay = false;
              });
          })
          .catch((response) => {
            response === 404
              ? console.warn("lo sientimos no tenemos servicios")
              : console.warn("Error:", response);
          });
      } else {
        alert("Tiene que ingresar Dni");
      }
    },

    selected(item) {
      this.datoscheck.push(item);
      console.log(this.datoscheck);
      console.log(checkbox2.toString());
    },
  },
  computed: {},
  created() {},
};
</script>
