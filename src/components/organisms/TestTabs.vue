<template>
  <div>
    <v-tabs v-model="selectedTab" align-tabs="center" color="deep-purple-accent-4">
      <v-tab v-for="(item, index) in items" :key="index" :value="item">
        {{ item }}
      </v-tab>
    </v-tabs>

    <v-window v-model="selectedTab">
      <v-window-item v-for="(item, index) in items" :key="index" :value="item">
        <slot :name="item"></slot>
      </v-window-item>
    </v-window>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  items: {
    type: Array,
    required: true,
    default: () => []
  }
});

const selectedTab = ref(props.items[0]);

</script>
