<template>
    <v-table hover>
      <thead>
        <tr>
          <template v-for="header in headers" :key="header.value">
            <th>
              {{ header.text }}
            </th>
          </template>
        </tr>
      </thead>
      <tbody>
        <slot name="body">
          <tr v-for="row in rows" :key="row.id">
            <td v-for="cell in row.cells" :key="cell.id">
              {{ cell.text }}
            </td>
          </tr>
        </slot>
      </tbody>
    </v-table>
</template>

<script setup>
const props = defineProps({
  headers: {
    type: Array
  },
  rows: {
    type: Array
  },
});
</script>
