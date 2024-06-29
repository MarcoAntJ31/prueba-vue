<template>
  <TestHeader :color="'secondary'" :title="'Tablero principal'" :buttons="buttons" :itemsTab="itemsTab"
    v-model:tab="tab" />
  <TestTabs :items="itemsTab">
    <template #Consulta>
      <v-card>
        <v-card-text>
          <div class="table-container">
            <TestTable :headers="headers" :rows="items" :showSearch="true" :showActions="true" actionName="View Details"
              actionButtonName="Details" @action="showInformation">
              <template #body="{ rows }">
                <ItemRows v-for="item in rows" :key="item.name" :item="item" />
              </template>
            </TestTable>
          </div>
          <v-pagination :length="4"></v-pagination>
        </v-card-text>
      </v-card>
    </template>

    <template #Edición>
      <v-card>
        <v-card-text>
          <div class="table-container">
            <TestTable :headers="headers" :rows="items" :showSearch="true" :showActions="true" actionName="Edit Details"
              actionButtonName="Edit" @action="showModal">
              <template #body="{ rows }">
                <ItemRows v-for="item in rows" :key="item.name" :item="item" />
              </template>
            </TestTable>
          </div>
          <v-pagination :length="4"></v-pagination>
        </v-card-text>
      </v-card>
      <v-dialog v-model="dialog">
        <TestForm :fields="fields" :item="selectedItem" buttonText="Guardar cambios" @form-edit="updateRecord"
          @form-submit="addNewRecord" />
      </v-dialog>
    </template>
    <template #Agregar>
      <TestForm :fields="fields" buttonText="Guardar cambios" @form-submit="addNewRecord" />
    </template>
  </TestTabs>
</template>

<script setup>
import { ref } from 'vue';
import TestHeader from './organisms/TestHeader.vue';
import TestForm from './organisms/TestForm.vue';
import TestTabs from './organisms/TestTabs.vue';
import TestTable from './organisms/TestTable.vue';
import ItemRows from './molecules/ItemRows.vue';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

const tab = ref('Consulta');
const itemsTab = ref(['Consulta', 'Edición', 'Agregar']);
const dialog = ref(false);
const selectedItem = ref(null);

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
  { name: 'Intel Pentium Gold G6400', cores: 2, threads: 4, baseClock: '4.0 GHz', boostClock: '4.3 GHz', tdp: '58W' },
  { name: 'AMD Athlon 3000G', cores: 2, threads: 4, baseClock: '3.5 GHz', boostClock: '4.3 GHz', tdp: '35W' },
]);

function showInformation(item) {
  alert(`Detalles de ${item.name}: \n
         Cores: ${item.cores}\n
         Threads: ${item.threads}\n
         Base Clock: ${item.baseClock}\n
         Boost Clock: ${item.boostClock}\n
         TDP: ${item.tdp}`);
}


function addNewRecord(formData) {
  try {
    // Verificar si todos los campos están completos
    if (!formData.name || !formData.cores || !formData.threads || !formData.baseClock || !formData.boostClock || !formData.tdp) {
      throw new Error('Todos los campos son obligatorios');
    }

    // Agregar el nuevo ítem
    items.value.push({
      name: formData.name,
      cores: formData.cores,
      threads: formData.threads,
      baseClock: formData.baseClock,
      boostClock: formData.boostClock,
      tdp: formData.tdp,
    });
    clearForm();
    toast.success('Item added successfully!', {
      autoClose: 1000,
      position: 'top-right',
    });
  } catch (error) {
    toast.error(`Error adding item: ${error.message}`, {
      autoClose: 1000,
      position: 'top-right',
    });
  }
}

function updateRecord(formData) {
  try {
    // Verificar si todos los campos están completos
    if (!formData.name || !formData.cores || !formData.threads || !formData.baseClock || !formData.boostClock || !formData.tdp) {
      throw new Error('Todos los campos son obligatorios');
    }

    if (selectedItem.value) {
      const index = items.value.findIndex(item => item.name === selectedItem.value.name);

      // Actualizar el ítem en el índice encontrado
      if (index !== -1) {
        items.value[index] = {
          name: formData.name,
          cores: formData.cores,
          threads: formData.threads,
          baseClock: formData.baseClock,
          boostClock: formData.boostClock,
          tdp: formData.tdp,
        };
        clearForm();
        toast.success('Item updated successfully!', {
          autoClose: 1000,
          position: 'top-right',
        });
        dialog.value = false;
      } else {
        throw new Error('Item not found');
      }
    }
  } catch (error) {
    toast.error(`Error updating item: ${error.message}`, {
      autoClose: 1000,
      position: 'top-right',
    });
  }
}

function clearForm() {
  selectedItem.value = null;
};

function showModal(item) {
  selectedItem.value = item;
  dialog.value = true;
};

const notify = () => {
      toast.success("Wow so easy !", {
        autoClose: 1000,
        position: "top-right",
      });
    }
</script>

<style scoped>
.table-container {
  max-height: 70vh;
  overflow-y: auto;
}

.v-card {
  margin: 12px;
}
</style>
