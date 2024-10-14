<template>
  <div class="pe-5 ps-5 pb-3 pt-3 rounded bg-white">
    <form @submit.prevent="onSubmit" @keyup.enter="onSubmit" class="needs-validation" novalidate>
      <h2 class="fw-bold text-uppercase">{{formTitle}}</h2>
      <p class="text-grey">Please fill in all the fields</p>

      <InputField
          id="name"
          label="Name"
          type="text"
          v-model="gambler.name"
          :invalid="formSubmitted && !usernameIsValid"
          :valid="usernameIsValid"
          required
          placeholder="Enter user name"
          errorMessage="Username is required."
      />

      <InputField
          id="dob"
          label="Date of Birth"
          type="date"
          v-model="gambler.dateOfBirth"
          :invalid="formSubmitted && !dateOfBirthIsValid"
          :valid="dateOfBirthIsValid"
          required
          placeholder="Enter date of birth"
          errorMessage="Date of Birth is required and should be less than present."
      />

      <InputField
          id="email"
          label="Email"
          type="email"
          v-model="gambler.email"
          :invalid="formSubmitted && !emailIsValid"
          :valid="emailIsValid"
          required
          placeholder="Enter email"
          errorMessage="Email is required and should be in valid format."
      />

      <InputField
          id="phoneNumber"
          label="Phone Number"
          type="tel"
          v-model="gambler.phoneNumber"
          :invalid="formSubmitted && !phoneNumberIsValid"
          :valid="phoneNumberIsValid"
          required
          placeholder="Enter phone number"
          errorMessage="Phone Number is required and should be in valid format for Ukraine."
      />

      <div class="d-flex justify-content-end">
        <AppButton variant="success" size="lg" block type="submit">
          {{ submitButtonText }}
        </AppButton>
      </div>
    </form>

    <ErrorMessage :message="errorMessage" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import type { Gambler } from '@/gambler';
import InputField from './InputField.vue';
import ErrorMessage from './ErrorMessage.vue';
import AppButton from "@/components/AppButton.vue";

const props = defineProps<{
  gambler?: Gambler;
  submitButtonText?: string;
  formTitle?: string;
}>();

const emit = defineEmits({
  'add-gambler': (gambler: Gambler) => {
    if (!gambler.email || !gambler.name || !gambler.dateOfBirth || !gambler.phoneNumber) {
      console.error('Invalid gambler data');
      return false;
    }
    return true;
  },
  'update-gambler': (gambler: Gambler) => {
    if (!gambler.email || !gambler.name || !gambler.dateOfBirth || !gambler.phoneNumber) {
      console.error('Invalid gambler data');
      return false;
    }
    return true;
  }
});

const gambler = ref<Gambler>(props.gambler || {
  email: '',
  dateOfBirth: null,
  name: '',
  phoneNumber: ''
});

const formSubmitted = ref(false);
const errorMessage = ref('');

const emailIsValid = computed(() => {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(gambler.value.email);
});

const phoneNumberIsValid = computed(() => {
  const regex = /^\+?3?8?(0\d{9})$/;
  return regex.test(gambler.value.phoneNumber);
});

const usernameIsValid = computed(() => {
  return !!gambler.value.name;
});

const dateOfBirthIsValid = computed(() => {
  return gambler.value.dateOfBirth && new Date(gambler.value.dateOfBirth) < new Date();
});

const onSubmit = () => {
  formSubmitted.value = true;
  errorMessage.value = '';

  const isEverythingValid = emailIsValid.value
      && phoneNumberIsValid.value
      && usernameIsValid.value
      && dateOfBirthIsValid.value;

  if (!isEverythingValid) {
    errorMessage.value = 'Please correct the errors in the form.';
    return;
  }

  if (props.gambler) {
    emit('update-gambler', { ...gambler.value });
  } else {
    emit('add-gambler', { ...gambler.value });
  }

  formSubmitted.value = false;
  gambler.value = {
    email: '',
    dateOfBirth: null,
    name: '',
    phoneNumber: ''
  };
};
</script>

<style lang="scss" scoped>
.text-grey {
  color: grey;
}
</style>