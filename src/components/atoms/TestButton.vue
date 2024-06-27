<template>
  <v-btn v-if="icon" icon>
    <v-icon>{{ icon }}</v-icon>
  </v-btn>
  <v-btn v-else>
    {{ text }}
  </v-btn>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  icon: {
    type: String,
    required: false,
  },
  text: {
    type: String,
    required: false,
  },
});
</script>
