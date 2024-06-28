<template>
  <v-form @submit.prevent="handleSubmit">
    <v-container>
      <v-row>
        <v-col v-for="(field, index) in fields" :key="index" :cols="getColumnSize(field.size)">
          <v-text-field v-model="formData[field.model]" :label="field.label" :type="field.type" :required="field.required"></v-text-field>
        </v-col>
      </v-row>

      <v-btn type="submit" color="primary">{{ buttonText }}</v-btn>
    </v-container>
  </v-form>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  fields: {
    type: Array,
    required: true,
    default: () => []
  },
  buttonText: {
    type: String,
    default: 'Submit'
  }
});

const formData = ref({});
const emit = defineEmits(['form-submit']);

function handleSubmit() {
  emit('form-submit', formData.value);
  formData.value = {};
};

function getColumnSize(size){
  if (size === 'full') {
    return 12;
  } else if (size === 'half') {
    return 6;
  } else {
    return 12;
  }
};
</script>
