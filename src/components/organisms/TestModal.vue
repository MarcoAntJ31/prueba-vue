<template>
  <v-form @submit.prevent="handleSubmit">
    <v-container>
      <v-row>
        <!-- Itera sobre los campos recibidos por props -->
        <v-col v-for="(field, index) in fields" :key="index" :cols="getColumnSize(field.size)">
          <v-text-field v-model="formData[field.model]" :label="field.label" :type="field.type" :required="field.required"></v-text-field>
        </v-col>
      </v-row>

      <v-btn type="submit" color="primary">{{ buttonText }}</v-btn>
    </v-container>
  </v-form>
</template>

<script>
export default {
  props: {
    fields: {
      type: Array,
      required: true,
      default: () => []
    },
    buttonText: {
      type: String,
      default: 'Submit'
    }
  },
  data() {
    return {
      formData: {}
    };
  },
  methods: {
    handleSubmit() {
      this.$emit('form-submit', this.formData);
      this.formData = {};
    },
    getColumnSize(size) {
      if (size === 'full') {
        return 12;
      } else if (size === 'half') {
        return 6;
      } else {
        return 12;
      }
    }
  }
};
</script>
