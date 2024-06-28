<template>
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
          <td v-for="(column, index) in columns" :key="index">{{ item[column.value] }}</td>
          <td>
            <slot name="actions" :item="item"></slot>
          </td>
        </tr>
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
  items: {
    type: Array,
    required: true,
  },
  columns: {
    type: Array,
    required: true,
  },
});

const search = ref('');

const filteredItems = computed(() => {
  return props.items.filter(item =>
    item.name.toLowerCase().includes(search.value.toLowerCase())
  );
});
</script>
