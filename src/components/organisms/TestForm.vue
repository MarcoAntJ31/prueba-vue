<template>
  <v-card>
    <v-card-title>
      {{ title }}
    </v-card-title>
    <v-card-text>
      <v-form @submit.prevent="handleSubmit">
        <v-container>
          <v-row>
            <v-col v-for="(field, index) in fields" :key="index" :cols="getColumnSize(field.size)" class="form-field">
              <v-text-field v-model="formData[field.model]" :label="field.label" :type="field.type" :required="field.required"></v-text-field>
            </v-col>
          </v-row>
          <v-btn type="submit" color="primary" class="submit-button">{{ buttonText }}</v-btn>
        </v-container>
      </v-form>
    </v-card-text>
  </v-card>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  fields: {
    type: Array,
    required: true,
    default: () => []
  },
  buttonText: {
    type: String,
    default: 'Submit'
  },
  title: {
    type: String,
    default: 'Add Item'
  },
  item: {
    type: Object,
    default: null
  }
});

const formData = ref({});


if (props.item) {
  formData.value = { ...props.item };
}

const emit = defineEmits(['form-submit', 'form-edit']);

function handleSubmit() {
  if (props.item) {
    emit('form-edit', formData.value);
  } else {
    emit('form-submit', formData.value);
  }
  formData.value = {};
};

function getColumnSize(size) {
  if (size === 'full') {
    return 12;
  } else if (size === 'half') {
    return 6;
  } else {
    return 12;
  }
};


watch(() => props.item, (newItem) => {
  formData.value = { ...newItem };
});
</script>

<style scoped>
.form-field {
  margin-bottom: 15px;
}

.v-input__control {
  background-color: white;
  border-radius: 5px;
}

.v-label {
  color: #000;
}

.v-container {
  max-width: 100vw;
}

.v-input__append-inner,
.v-input__prepend-inner {
  color: #000;
}

.submit-button {
  background-color: #1976d2;
  color: white;
}

.submit-button:hover {
  background-color: #1565c0;
}
</style>
