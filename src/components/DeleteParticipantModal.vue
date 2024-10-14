<template>
  <Modal v-if="showModal" @close="closeModal">
    <template #header>Confirm Deletion</template>
    <template #body>
      <p>Are you sure you want to delete participant "{{ gambler.name }}", "{{ gambler.email }}"?</p>
      <div class="d-flex justify-content-end">
        <Button buttonClass="btn btn-danger me-2" @click="confirmDelete">Yes</Button>
        <Button buttonClass="btn btn-secondary" @click="closeModal">No</Button>
      </div>
    </template>
  </Modal>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import type { Gambler } from '@/gambler';
import Modal from './Modal.vue';
import Button from './Button.vue';

const props = defineProps<{
  gambler: Gambler
}>();

const emit = defineEmits(['delete', 'close']);

const showModal = ref(true);

const confirmDelete = () => {
  emit('delete', props.gambler);
  closeModal();
};

const closeModal = () => {
  showModal.value = false;
  emit('close');
};
</script>