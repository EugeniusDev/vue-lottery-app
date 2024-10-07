<script setup lang="ts">
import {computed, ref} from "vue";
import type {Gambler} from "@/gambler";

const gambler = ref<Gambler>({
  email: '',
  dateOfBirth: null,
  name: '',
  phoneNumber: ''
});

const emit = defineEmits(['add-gambler']);
const formSubmitted = ref(false);

const emailIsValid = computed(() => {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(gambler.value.email);
});
const phoneNumberIsValid = computed(() => {
  const regex = /^\+?3?8?(0\d{9})$/;
  return regex.test(gambler.value.phoneNumber);
})
const usernameIsValid = computed(() => {
  return gambler.value.name;
});
const dateOfBirthIsValid = computed(() => {
  return gambler.value.dateOfBirth && new Date(gambler.value.dateOfBirth) < new Date();
});
const onSubmit = () => {
  formSubmitted.value = true;
  let isEverythingValid = emailIsValid.value
      && phoneNumberIsValid.value
      && usernameIsValid.value
      && dateOfBirthIsValid.value;
  if(!isEverythingValid){
    return;
  }

  emit('add-gambler', {...gambler.value});
  formSubmitted.value = false;
  gambler.value.dateOfBirth = null;
  gambler.value.name = '';
  gambler.value.phoneNumber = '';
  gambler.value.email = '';
};
</script>


<template>
  <div class="pe-5 ps-5 pb-3 pt-3 rounded bg-white">
    <form @submit.prevent="onSubmit" class="needs-validation" novalidate>
      <h2 class="fw-bold text-uppercase">Registration form</h2>
      <p class="text-grey">Please fill in all the fields</p>
      <div class="mb-3">
        <label for="name" class="form-label fw-bold">Name</label>
        <input :class="{ 'is-invalid': formSubmitted && !usernameIsValid, 'is-valid': usernameIsValid}" id="name" class="form-control bg-secondary-subtle" type="text" required v-model="gambler.name" placeholder="Enter user name">
        <div class="invalid-feedback">
          Username is required.
        </div>
      </div>

      <div class="mb-3">
        <label for="dob" class="form-label fw-bold">Date of Birth</label>
        <input :class="{ 'is-invalid': formSubmitted && !dateOfBirthIsValid, 'is-valid': dateOfBirthIsValid }" id="dob" class="form-control bg-secondary-subtle" type="date" required v-model="gambler.dateOfBirth"
               placeholder="Enter date of birth">
        <div class="invalid-feedback">
          Date of Birth is required and should be less than present.
        </div>
      </div>

      <div class="mb-3">
        <label for="email" class="form-label fw-bold">Email</label>
        <input :class="{ 'is-invalid': formSubmitted && !emailIsValid, 'is-valid': emailIsValid }" pattern="" id="email" class="form-control bg-secondary-subtle" type="text" required v-model="gambler.email" placeholder="Enter email">
        <div class="invalid-feedback">
          Email is required and should be in valid format.
        </div>
      </div>

      <div class="mb-3">
        <label for="phoneNumber" class="form-label fw-bold">Phone Number</label>
        <input :class="{ 'is-invalid': formSubmitted && !phoneNumberIsValid, 'is-valid': phoneNumberIsValid }" pattern="^\+?3?8?(0\d{9})$" id="phoneNumber" class="form-control bg-secondary-subtle" type="text" required v-model="gambler.phoneNumber"
               placeholder="Enter phone number">
        <div class="invalid-feedback">
          Phone Number is required and should be in valid format for Ukraine.
        </div>
      </div>

      <div class="d-flex justify-content-end">
        <button class="btn btn-primary" type="submit">Save</button>
      </div>
    </form>
  </div>
</template>

<style scoped>
.text-grey {
  color: grey;
}
</style>