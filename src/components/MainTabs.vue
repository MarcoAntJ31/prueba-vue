<template>
  <v-card>
    <v-toolbar color="primary">
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>Tablero principal</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>

      <template v-slot:extension>
        <v-tabs
          v-model="tab"
          align-tabs="title"
        >
          <v-tab
            v-for="item in itemsTab"
            :key="item"
            :value="item"
          >
            {{ item }}
          </v-tab>
        </v-tabs>
      </template>
    </v-toolbar>

    <v-window v-model="tab">
      <v-window-item
        v-for="item in itemsTab"
        :key="item"
        :value="item"
      >
        <v-card flat>

          <v-data-table
            :headers="headers"
            :items="items"
            :search="search"
            item-value="name"
          >
            <template v-slot:top>
              <v-text-field
                v-model="search"
                class="pa-2"
                label="Search"
              ></v-text-field>
            </template>

            <template v-slot:item.actions="{ item }">
              <v-btn @click="showInformation(item)">Ver Detalles</v-btn>
            </template>

          </v-data-table>

        </v-card>
      </v-window-item>
    </v-window>
  </v-card>
</template>

<script setup>
import { ref } from 'vue';

const tab = ref(null);
const itemsTab = ref(['Consulta', 'Edición', 'Agregar registro']);
const search = ref('');

const headers = [
  { text: 'CPU Model', align: 'start', value: 'name' },
  { text: 'Cores', align: 'end', value: 'cores' },
  { text: 'Threads', align: 'end', value: 'threads' },
  { text: 'Base Clock', align: 'end', value: 'baseClock' },
  { text: 'Boost Clock', align: 'end', value: 'boostClock' },
  { text: 'TDP (W)', align: 'end', value: 'tdp' },
  { text: 'Actions', align: 'end', value: 'actions' },
];

const items = [
  { name: 'Intel Core i9-11900K', cores: 8, threads: 16, baseClock: '3.5 GHz', boostClock: '5.3 GHz', tdp: '125W' },
  { name: 'AMD Ryzen 9 5950X', cores: 16, threads: 32, baseClock: '3.4 GHz', boostClock: '4.9 GHz', tdp: '105W' },
  { name: 'Intel Core i7-10700K', cores: 8, threads: 16, baseClock: '3.8 GHz', boostClock: '5.1 GHz', tdp: '125W' },
  { name: 'AMD Ryzen 5 5600X', cores: 6, threads: 12, baseClock: '3.7 GHz', boostClock: '4.6 GHz', tdp: '65W' },
  { name: 'Intel Core i5-10600K', cores: 6, threads: 12, baseClock: '4.1 GHz', boostClock: '4.8 GHz', tdp: '125W' },
  { name: 'AMD Ryzen 7 5800X', cores: 8, threads: 16, baseClock: '3.8 GHz', boostClock: '4.7 GHz', tdp: '105W' },
  { name: 'Intel Core i3-10100', cores: 4, threads: 8, baseClock: '3.6 GHz', boostClock: '4.3 GHz', tdp: '65W' },
  { name: 'AMD Ryzen 3 3300X', cores: 4, threads: 8, baseClock: '3.8 GHz', boostClock: '4.3 GHz', tdp: '65W' },
  { name: 'Intel Pentium Gold G6400', cores: 2, threads: 4, baseClock: '4.0 GHz', tdp: '58W' },
  { name: 'AMD Athlon 3000G', cores: 2, threads: 4, baseClock: '3.5 GHz', tdp: '35W' },
];

function showInformation(item) {
  alert(`Detalles de ${item.name}: \n
         Cores: ${item.cores}\n
         Threads: ${item.threads}\n
         Base Clock: ${item.baseClock}\n
         Boost Clock: ${item.boostClock}\n
         TDP: ${item.tdp}`);
}
</script>
