<template>
  <v-card flat>
    <v-table hover density="compact">
      <template v-if="showSearch" v-slot:top>
        <v-text-field v-model="search" class="pa-2 mt-1" label="Search"></v-text-field>
      </template>
      <thead>
        <tr>
          <template v-for="header in headers" :key="header.value">
            <th>
              {{ header.text }}
            </th>
          </template>
          <th v-if="showActions"> {{ actionName }}</th>
        </tr>
      </thead>
      <tbody>
        <slot :rows="filteredRows">
          <tr v-for="row in filteredRows" :key="row.id">
            <td v-for="(value, key) in row" v-if="key !== 'id'" :key="key">
              {{ value }}
            </td>
            <td v-if="showActions">
              <v-btn @click="handleAction(row)">{{ actionButtonName }}</v-btn>
            </td>
          </tr>
        </slot>
      </tbody>
    </v-table>
  </v-card>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  headers: {
    type: Array,
    required: true,
  },
  rows: {
    type: Array,
    required: true,
    default: () => [],
  },
  showSearch: {
    type: Boolean,
    default: false,
  },
  showActions: {
    type: Boolean,
    default: false,
  },
  actionName: {
    type: String,
    default: 'Actions',
  },
  actionButtonName: {
    type: String,
    default: 'Action',
  },
});

const search = ref('');
const emit = defineEmits(['action']);

const filteredRows = computed(() => {
  if (!props.showSearch) {
    return props.rows;
  }

  return props.rows.filter(row =>
    Object.values(row).some(value =>
      value.toString().toLowerCase().includes(search.value.toLowerCase())
    )
  );
});

function handleAction(row) {
  emit('action', row);
}
</script>
