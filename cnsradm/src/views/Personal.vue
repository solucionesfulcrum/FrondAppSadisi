<template>
  <div class="row">
    <v-row justify="center">
      <v-overlay :value="overlay">
        <v-progress-circular indeterminate size="64"></v-progress-circular>
      </v-overlay>
      <v-dialog v-model="dialog" persistent max-width="100vh">
        <v-card>
          <v-card-title>
            <span class="text-h5">Agregar Personal</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="6">
                  <v-select
                    :items="['DNI', 'CART. EXT']"
                    v-model="setTipoDoc"
                    label="Tipo Dni"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" sm="6" md="6">
                  <v-text-field
                    v-model="setNumDoc"
                    label="Num. Documento"
                    type="password"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field label="Ape. Paterno"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field label="Ape. Materno"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field label="Nombres"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-select
                    :items="['Masculino', 'Femenino']"
                    label="Sexo"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-select
                    :items="['Masculino', 'Femenino']"
                    label="Sexo"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field
                    label="Cod. Planilla"
                    type="number"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-select
                    :items="['728', '276', 'Cas', 'Externo']"
                    label="Regimen Personal"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-select
                    :items="['Medico', 'Jefe División', 'Jefe Oficina']"
                    label="Cargo Personal"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-select
                    :items="['P1', 'T2']"
                    label="Nivel Personal"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field
                    label="Num. Teléfono"
                    type="number"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="3">
                  <v-text-field label="Correo" type="email"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="6">
                  <v-text-field label="Direccion Personal"></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="6">
                  <v-select
                    :items="['Dep Admin', 'DSI']"
                    label="Dependencia"
                    required
                  ></v-select>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false">
              Cancelar
            </v-btn>
            <v-btn color="blue darken-1" text @click="dialog = false">
              Guardar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
    <div class="col-md-11 mt-5 mr-5">
      <v-system-bar color="#1973a5" dark>
        Personal Asistencial y Administrativo
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
              v-model="numDocu"
              :rules="nameRules"
              :counter="8"
              label="Número de documento"
              required
              type="number"
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="1">
            <v-btn class="" icon color="#1973a5" @click="buscarPersonal">
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
            <v-btn class="" icon color="#1973a5" @click="buscarPersonal">
              <v-icon>mdi-magnify</v-icon>
            </v-btn>
          </v-col>
          <v-col cols="12" md="3">
            <v-btn
              block
              elevation="2"
              color="#1973a5"
              icon
              @click="agregarPersonal"
              large
              text
              tile
              x-large
              ><v-icon dark> mdi-hospital </v-icon>Agregar Personal</v-btn
            >
          </v-col>
        </div>
      </v-card>
      <v-card class="mt-3">
        <v-system-bar color="#1973a5" dark> Personal </v-system-bar>
        <v-card-title>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table :headers="headers" :items="desserts" :search="search"
          ><template v-slot:[`item.actions`]="{ item }">
            <v-icon small class="mr-2" @click="editItem(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
          </template>
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
      overlay: false,
      //data de datatabble
      search: "",
      headers: [
        {
          text: "Num Doc",
          align: "start",
          value: "dniPer",
        },
        { text: "Ape. Pat.", value: "apePatPer" },
        { text: "Ape. Mat.", value: "apeMatPer" },
        { text: "Nombres", value: "nomPer" },
        { text: "Cargo", value: "cargoPer" },
        { text: "Dependencia", value: "datosDependencia.descDep" },
        { text: "Acciones", value: "actions", sortable: false },
      ],
      desserts: [],
      //
      setTipoDoc: "DNI",
      setNumDoc: "",
      numDocu: "",
      valid: false,
      nameRules: [
        (v) => !!v || "Número requerido",
        (v) => v.length <= 8 || "8 caracteres como maximo",
      ],
      tipoDoc: ["DNI", "Carnet Ext.", "Pasaporte"],
      listaPersonal: ["nombre1", "nombre2", "nombre3"],
      dialog: false,
      getTipoDoc: "DNI",
    };
  },
  methods: {
    buscarPersonal() {
      console.log("buscar personal");
    },
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
    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
      console.log("edit",item)
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },
  },
  computed: {},
  created() {
      this.overlay=true
    axios
      .post(RUTA_SERVIDOR + "/api/token/", {
        username: "cnsr",
        password: "123456",
      })
      .then((response) => {
        this.auth = "Bearer " + response.data.access;
        axios
          .get(RUTA_SERVIDOR + "/personal/", {
            headers: { Authorization: this.auth },
          })
          .then((res) => {
            console.log(res.data);
            this.desserts = res.data;
            this.overlay=false
          })
          .catch((res) => {
            console.warn("Error:", res);
            this.overlay=false
          });
      })
      .catch((response) => {
        response === 404
          ? console.warn("lo sientimos no tenemos servicios")
          : console.warn("Error:", response);
      });
  },
};
</script>
