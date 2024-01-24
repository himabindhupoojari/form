<template>
  <div class="container">
    <h1>Registration Form</h1>
    <p>All required fields are manidatory.</p>
    <div class="form">
      <div class="row">
        <label>Name <span>*</span></label>
        <input
          type="text"
          placeholder="Name"
          v-model="formData.name"
          @input="handleInputChange"
          @click="enableField('name')"
          :readonly="isReadonly.name"
          v-bind:class="[isReadonly.name ? 'input_disable' : 'input_enable']"
        />
      </div>

      <div class="row">
        <label>Employee Id <span>*</span></label>
        <input
          type="text"
          placeholder="Employee ID"
          v-model="formData.empid"
          @input="handleInputChange"
          @click="enableField('empid')"
          :readonly="isReadonly.empid"
          v-bind:class="[isReadonly.empid ? 'input_disable' : 'input_enable']"
        />
      </div>

      <div class="row">
        <label>Select Country <span>*</span></label>
        <select
          v-model="formData.country"
          @change="handleSelectChange"
          @click="enableField('country')"
          :readonly="isReadonly.country"
          v-bind:class="[isReadonly.country ? 'input_disable' : 'input_enable']"
        >
          <option value="">Select Country</option>
          <option value="newyork">New York</option>
          <option value="london">London</option>
          <option value="tokyo">Tokyo</option>
        </select>
      </div>

      <div class="row">
        <label>Gender <span>*</span></label>
        <div class="radio">
          <input
            type="radio"
            v-model="formData.gender"
            id="male"
            name="male"
            @change="handleRadioChange('male')"
            @click="enableField('gender')"
            :readonly="isReadonly.gender"
            v-bind:class="[
              isReadonly.gender ? 'input_disable' : 'input_enable',
            ]"
            :checked="isMaleChecked()"
          />
          <label for="male">Male</label>

          <input
            type="radio"
            v-model="formData.gender"
            id="female"
            name="female"
            @change="handleRadioChange('female')"
            @click="enableField('gender')"
            :readonly="isReadonly.gender"
            v-bind:class="[
              isReadonly.gender ? 'input_disable' : 'input_enable',
            ]"
            :checked="isFemaleChecked()"
          />
          <label for="female">Female</label>
        </div>
      </div>

      <button @click="submitForm">Submit Form</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    const obj = JSON.parse(localStorage.getItem("formData"));
    return {
      formData: {
        name: obj && obj.name ? obj.name : "",
        empid: obj && obj.empid ? obj.empid : "",
        country: obj && obj.country ? obj.country : "",
        gender: obj && obj.gender ? obj.gender : "",
      },
      isReadonly: {
        name: false,
        empid: false,
        country: false,
        gender: false,
      },

      message: "Values will be displayed here.",
    };
  },
  methods: {
    handleInputChange() {
      this.updateMessage();
    },
    handleSelectChange() {
      this.updateMessage();
    },
    updateMessage() {
      this.message = `Name: ${this.formData.name}, Password: ${this.formData.empid}, Selected Option: ${this.formData.country}, Gender:${this.formData.gender}`;
    },
    enableField(value) {
      const storedData = JSON.parse(localStorage.getItem("formData"));

      for (let i = 0; i < Object.keys(storedData).length; i++) {
        if (Object.keys(storedData)[i] === value) {
          this.isReadonly[value] = false;
        }
      }
    },
    isMaleChecked() {
      return this.formData.gender === "male";
    },
    isFemaleChecked() {
      return this.formData.gender === "female";
    },
    handleRadioChange(value) {
      this.formData.gender = value;
    },
    submitForm() {
      if (
        !this.formData.name ||
        !this.formData.empid ||
        !this.formData.country ||
        !this.formData.gender
      ) {
        alert("Please enter all the fields");
      } else {
        localStorage.setItem("formData", JSON.stringify(this.formData));
        console.log("Form Data:", this.formData);
        alert("Hurray! form submitted successfully");
        (this.isReadonly.name = true),
          (this.isReadonly.country = true),
          (this.isReadonly.empid = true),
          (this.isReadonly.gender = true);
      }
    },
  },
};
</script>

<style>
@import "./assets/main.scss";
</style>
