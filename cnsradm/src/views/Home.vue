<template>
  <v-card color="grey lighten-4" flat height="100%" tile>
    <v-toolbar dense>
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title
        >Sistema de Administración de la Division de Soporte Informatico
        (SADISI)</v-toolbar-title
      >

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-toolbar>
    <div class="row">
      <div class="col-md-2 mt-1">
        <v-card height="800%" width="256px" class="mx-auto">
          <v-navigation-drawer permanent>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title class="text-h6"> CNSR </v-list-item-title>
                <v-list-item-subtitle> Usuario </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list dense nav>
              <v-list-item-group v-model="selectedItem" color="primary">
                <v-list-item v-for="item in items" :key="item.title" link>
                  <v-list-item-icon>
                    <v-icon>{{ item.icon }}</v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title>{{ item.title }}</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-navigation-drawer>
        </v-card>
      </div>
      <div v-if="selectedItem == 0" class="col-sm-10"><Personal /></div>
      <div v-if="selectedItem == 8" class="col-sm-10"><Reportes /></div>
    </div>
  </v-card>
</template>

<script>
// @ is an alias to /src
import Personal from "./Personal.vue";
import Reportes from './Reportes.vue';

export default {
  name: "Home",
  components: {
    Personal,
    Reportes,
  },
  data() {
    return {
      items: [
        { title: "Personal", icon: "mdi-account-multiple-check" },
        { title: "Bien", icon: "mdi-lan-check" },
        { title: "BIen - Software", icon: "mdi-content-save" },
        { title: "Personal - Bien", icon: "mdi-account-multiple-check" },
        { title: "Personal - Vpn", icon: "mdi-lan" },
        { title: "Personal - Correo", icon: "mdi-email-open-multiple-outline" },
        { title: "Per - Cert. Digital", icon: "mdi-eslint" },
        { title: "Entrega - Cargo", icon: "mdi-account-convert" },
        { title: "Reportes", icon: "mdi-file-chart" },
      ],
      selectedItem: null,
    };
  },
};
</script>
