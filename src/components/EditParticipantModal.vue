<template>
  <Modal v-if="showModal" @close="closeModal">
    <template #header>Edit Participant</template>
    <template #body>
      <RegistrationForm
          :gambler="editedGambler"
          @submit="updateGambler"
          submitButtonText="Update"
      />
    </template>
  </Modal>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import type { Gambler } from '@/gambler';
import Modal from './Modal.vue';
import RegistrationForm from './RegistrationForm.vue';

const props = defineProps<{
  gambler: Gambler
}>();

const emit = defineEmits(['update', 'close']);

const showModal = ref(true);
const editedGambler = reactive({ ...props.gambler });

const updateGambler = () => {
  emit('update', editedGambler);
  closeModal();
};

const closeModal = () => {
  showModal.value = false;
  emit('close');
};
</script>