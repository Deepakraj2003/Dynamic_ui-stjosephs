<template>
  <div class="formm">
    <form @submit.prevent="handleSubmit">
      <div v-for="field in formFields" :key="field.id" class="form-field">
        <label :for="field.id" class="label">{{ field.label }}</label>
        <input
          :type="field.type"
          :id="field.id"
          v-model="field.value"
          class="input-field"
        />
      </div>
      <button type="submit" class="submit-button">Submit</button>
    </form>
    {{ formd }}
  </div>
</template>

<style>
.formm {
  top: 0%;
}
.form-field {
  margin-bottom: 10px;
}

.label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

.input-field {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 3px;
  width: 200px;
}

.submit-button {
  margin-top: 10px;
  padding: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}
</style>

<script>
import json from "./form.json";
export default {
  data() {
    return {
      formFields: json.formFields,
      formd: "",
    };
  },
  methods: {
    handleSubmit() {
      // Access the form field values here
      const formData = this.formFields.reduce((data, field) => {
        data[field.id] = field.value;
        return data;
      }, {});

      // Do something with the form data (e.g., send it to a server)
      console.log(formData);
      this.formd = formData;
    },
  },
};
</script>
