<template>
  <tr>
    <td>{{ item.name }}</td>
    <td>{{ item.cores }}</td>
    <td>{{ item.threads }}</td>
    <td>{{ item.baseClock }}</td>
    <td>{{ item.boostClock }}</td>
    <td>{{ item.tdp }}</td>
  </tr>
</template>

<script>
import { defineProps } from 'vue';

const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
});
</script>
