<template>
  <div class="w-75 m-auto">
    <WinnersList
        :winners="luckyGamblers"
        :users-count="gamblers.length"
        @delete-winner="deleteWinner"
        @new-winner="addNewWinner"
    />
    <RegistrationForm
        @add-gambler="addGambler"
        submitButtonText="Save"
        form-title="Registration form"
    />
    <SearchBar @filter-by-name="filterGamblers" />
    <ParticipantsTable
        :gamblers="filteredGamblers"
        @edit-gambler="editGambler"
        @delete-gambler="deleteGambler"
    />
    <ErrorMessage :message="errorMessage" />
    <EditParticipantModal
        v-if="editingGambler"
        :gambler="editingGambler"
        @update="updateGambler"
        @close="closeEditModal"
    />
    <DeleteParticipantModal
        v-if="deletingGambler"
        :gambler="deletingGambler"
        @delete="confirmDeleteGambler"
        @close="closeDeleteModal"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue';
import type { Gambler } from '@/gambler';
import WinnersList from '@/components/WinnersList.vue';
import RegistrationForm from '@/components/RegistrationForm.vue';
import SearchBar from '@/components/SearchBar.vue';
import ParticipantsTable from '@/components/ParticipantsTable.vue';
import ErrorMessage from '@/components/ErrorMessage.vue';
import EditParticipantModal from '@/components/EditParticipantModal.vue';
import DeleteParticipantModal from '@/components/DeleteParticipantModal.vue';

const gamblers = ref<Gambler[]>([]);
const luckyGamblers = ref<Gambler[]>([]);
const searchTerm = ref('');
const errorMessage = ref('');
const editingGambler = ref<Gambler | null>(null);
const deletingGambler = ref<Gambler | null>(null);

onMounted(() => {
  const storedGamblers = localStorage.getItem('gamblers');
  if (storedGamblers) {
    gamblers.value = JSON.parse(storedGamblers);
  }
});

watch(gamblers, (newGamblers) => {
  localStorage.setItem('gamblers', JSON.stringify(newGamblers));
}, { deep: true });

const filteredGamblers = computed(() => {
  return gamblers.value.filter(gambler =>
      gambler.name.toLowerCase().includes(searchTerm.value.toLowerCase())
  );
});

const addGambler = (gambler: Gambler) => {
  if (gamblers.value.some(g => g.email === gambler.email)) {
    errorMessage.value = 'A participant with this email already exists.';
    return;
  }
  gamblers.value.push(gambler);
  errorMessage.value = '';
};

const deleteWinner = (gambler: Gambler) => {
  luckyGamblers.value = luckyGamblers.value.filter(x => x.email !== gambler.email);
};

const addNewWinner = () => {
  if (luckyGamblers.value.length >= 3 || gamblers.value.length === 0) {
    return;
  }
  const randomIndex = Math.floor(Math.random() * gamblers.value.length);
  const lucker = gamblers.value[randomIndex];
  if (luckyGamblers.value.some(w => w.email === lucker.email)) {
    return;
  }
  luckyGamblers.value.push(lucker);
};

const filterGamblers = (term: string) => {
  searchTerm.value = term;
};

const editGambler = (gambler: Gambler) => {
  editingGambler.value = { ...gambler };
};

const updateGambler = (updatedGambler: Gambler) => {
  const index = gamblers.value.findIndex(g => g.email === updatedGambler.email);
  if (index !== -1) {
    gamblers.value[index] = updatedGambler;
  }
  closeEditModal();
};

const closeEditModal = () => {
  editingGambler.value = null;
};

const deleteGambler = (gambler: Gambler) => {
  deletingGambler.value = gambler;
};

const confirmDeleteGambler = (gambler: Gambler) => {
  gamblers.value = gamblers.value.filter(g => g.email !== gambler.email);
  luckyGamblers.value = luckyGamblers.value.filter(g => g.email !== gambler.email);
  closeDeleteModal();
};

const closeDeleteModal = () => {
  deletingGambler.value = null;
};
</script>

<style lang="scss" scoped>

</style>