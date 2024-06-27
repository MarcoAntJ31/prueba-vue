<template>
  <v-toolbar :color="color">
    <v-app-bar-nav-icon></v-app-bar-nav-icon>

    <v-toolbar-title>{{ title }}</v-toolbar-title>

    <v-spacer></v-spacer>

    <TestButton
      v-for="(btn, index) in buttons"
      :key="index"
      :icon="btn.icon"
      @click="btn.action"
    />

    <template v-slot:extension>
      <v-tabs v-model="tab" align-tabs="title">
        <v-tab v-for="item in itemsTab" :key="item" :value="item">
          {{ item }}
        </v-tab>
      </v-tabs>
    </template>
  </v-toolbar>
</template>

<script setup>
import { ref, defineProps } from 'vue';
import TestButton from '../atoms/TestButton.vue';

const props = defineProps({
  color: {
    type: String,
    default: 'primary',
  },
  title: {
    type: String,
    required: true,
  },
  buttons: {
    type: Array,
    required: true,
  },
  itemsTab: {
    type: Array,
    required: true,
  },
  tab: {
    type: String,
    required: true,
  }
});

const tab = ref(props.tab);
</script>
