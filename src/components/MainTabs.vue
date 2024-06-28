<template>
  <v-card>
    <TestHeader :color="'primary'" :title="'Tablero principal'" :buttons="buttons" :itemsTab="itemsTab"
      v-model:tab="tab" />

    <TestTabs :items="itemsTab">
      <template #Consulta>
        <v-card flat>
          <v-table hover>
            <template v-slot:top>
              <v-text-field v-model="search" class="pa-2" label="Search"></v-text-field>
            </template>
            <thead>
              <tr>
                <th v-for="header in headers" :key="header.value">{{ header.text }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in filteredItems" :key="item.name">
                <td>{{ item.name }}</td>
                <td>{{ item.cores }}</td>
                <td>{{ item.threads }}</td>
                <td>{{ item.baseClock }}</td>
                <td>{{ item.boostClock }}</td>
                <td>{{ item.tdp }}</td>
                <td>
                  <TestButton text="Ver Detalles" @click="showInformation(item)" />
                </td>
              </tr>
            </tbody>
          </v-table>
        </v-card>
      </template>

      <template #Edición>
        <v-card flat>
          <v-table hover>
            <template v-slot:top>
              <v-text-field v-model="search" class="pa-2" label="Search"></v-text-field>
            </template>
            <thead>
              <tr>
                <th v-for="header in headers2" :key="header.value">{{ header.text }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in filteredItems" :key="item.name">
                <td>{{ item.name }}</td>
                <td>{{ item.cores }}</td>
                <td>{{ item.threads }}</td>
                <td>{{ item.baseClock }}</td>
                <td>{{ item.boostClock }}</td>
                <td>{{ item.tdp }}</td>
                <td>
                  <TestButton text="Editar datos" />
                </td>
              </tr>
            </tbody>
          </v-table>
        </v-card>
      </template>

      <template #Agregar>
        <v-card flat>
          <TestModal :fields="fields" buttonText="Guardar cambios" @form-submit="addNewRecord" />
        </v-card>
      </template>
    </TestTabs>
  </v-card>
</template>

<script setup>
import { ref, computed } from 'vue';
import TestHeader from './organisms/TestHeader.vue';
import TestButton from './atoms/TestButton.vue';
import TestModal from './organisms/TestModal.vue';
import TestTabs from './organisms/TestTabs.vue';
import TestTable from './organisms/TestTable.vue';

const tab = ref('Consulta');
const itemsTab = ref(['Consulta', 'Edición', 'Agregar']);
const search = ref('');

const buttons = [
  { icon: 'mdi-magnify' },
  { icon: 'mdi-dots-vertical' },
];

const fields = ref([
  { label: 'CPU Model', model: 'name', type: 'text', required: true, size: 'full' },
  { label: 'Cores', model: 'cores', type: 'number', required: true, size: 'half' },
  { label: 'Threads', model: 'threads', type: 'number', required: true, size: 'half' },
  { label: 'Base Clock', model: 'baseClock', type: 'text', required: true, size: 'half' },
  { label: 'Boost Clock', model: 'boostClock', type: 'text', required: true, size: 'half' },
  { label: 'TDP (W)', model: 'tdp', type: 'text', required: true, size: 'half' },
]);

const headers = [
  { text: 'CPU Model', align: 'start', value: 'name' },
  { text: 'Cores', align: 'end', value: 'cores' },
  { text: 'Threads', align: 'end', value: 'threads' },
  { text: 'Base Clock', align: 'end', value: 'baseClock' },
  { text: 'Boost Clock', align: 'end', value: 'boostClock' },
  { text: 'TDP (W)', align: 'end', value: 'tdp' },
  { text: 'Actions', align: 'end', value: 'actions' },
];

const headers2 = [
  { text: 'CPU Model', align: 'start', value: 'name' },
  { text: 'Cores', align: 'end', value: 'cores' },
  { text: 'Threads', align: 'end', value: 'threads' },
  { text: 'Base Clock', align: 'end', value: 'baseClock' },
  { text: 'Boost Clock', align: 'end', value: 'boostClock' },
  { text: 'TDP (W)', align: 'end', value: 'tdp' },
  { text: 'Edit', align: 'end', value: 'edit' },
];


const items = ref([
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
]);

const filteredItems = computed(() => {
  return items.value.filter(item =>
    item.name.toLowerCase().includes(search.value.toLowerCase())
  );
});

function showInformation(item) {
  alert(`Detalles de ${item.name}: \n
         Cores: ${item.cores}\n
         Threads: ${item.threads}\n
         Base Clock: ${item.baseClock}\n
         Boost Clock: ${item.boostClock}\n
         TDP: ${item.tdp}`);
}

const newItem = ref({
  name: '',
  cores: null,
  threads: null,
  baseClock: '',
  boostClock: '',
  tdp: '',
});

const addNewRecord = (formData) => {
  console.log('Adding new record:', formData);
  items.value.push({
    name: formData.name,
    cores: formData.cores,
    threads: formData.threads,
    baseClock: formData.baseClock,
    boostClock: formData.boostClock,
    tdp: formData.tdp,
  });

  clearForm();
};

const clearForm = () => {
  newItem.value = {
    name: '',
    cores: null,
    threads: null,
    baseClock: '',
    boostClock: '',
    tdp: '',
  };
};
</script>
