<template>
  <div class="mt-3 p-4 mb-5 border bg-white rounded">
    <table class="table rounded">
      <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">
          Name
          <div>
          <button v-if="sortDirection === 'asc'" @click="sort('name')">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-sort-alpha-down" viewBox="0 0 16 16">
              <path fill-rule="evenodd" d="M10.082 5.629 9.664 7H8.598l1.789-5.332h1.234L13.402 7h-1.12l-.419-1.371zm1.57-.785L11 2.687h-.047l-.652 2.157z"/>
              <path d="M12.96 14H9.028v-.691l2.579-3.72v-.054H9.098v-.867h3.785v.691l-2.567 3.72v.054h2.645zM4.5 2.5a.5.5 0 0 0-1 0v9.793l-1.146-1.147a.5.5 0 0 0-.708.708l2 1.999.007.007a.497.497 0 0 0 .7-.006l2-2a.5.5 0 0 0-.707-.708L4.5 12.293z"/>
            </svg>
          </button>
          <button v-if="sortDirection === 'desc'" @click="sort('name')">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-sort-alpha-up" viewBox="0 0 16 16">
              <path fill-rule="evenodd" d="M10.082 5.629 9.664 7H8.598l1.789-5.332h1.234L13.402 7h-1.12l-.419-1.371zm1.57-.785L11 2.687h-.047l-.652 2.157z"/>
              <path d="M12.96 14H9.028v-.691l2.579-3.72v-.054H9.098v-.867h3.785v.691l-2.567 3.72v.054h2.645zM4.5 2.5a.5.5 0 0 0-1 0v9.793l-1.146-1.147a.5.5 0 0 0-.708.708l2 1.999.007.007a.497.497 0 0 0 .7-.006l2-2a.5.5 0 0 0-.707-.708L4.5 12.293z"/>
            </svg>
          </button>
          </div>
        </th>
        <th scope="col">Email</th>
        <th scope="col">
          Date of Birth
          <div>
          <button v-if="sortDirection === 'asc'" @click="sort('dateOfBirth')">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-sort-down" viewBox="0 0 16 16">
              <path d="M3.5 12.5a.5.5 0 0 1-1 0V3.707L1.354 4.854a.5.5 0 1 1-.708-.708l2-1.999.007-.007a.5.5 0 0 1 .7.006l2 2a.5.5 0 1 1-.707.708L3.5 3.707zm3.5-9a.5.5 0 0 1 .5-.5h7a.5.5 0 0 1 0 1h-7a.5.5 0 0 1-.5-.5M7.5 6a.5.5 0 0 0 0 1h5a.5.5 0 0 0 0-1zm0 3a.5.5 0 0 0 0 1h3a.5.5 0 0 0 0-1zm0 3a.5.5 0 0 0 0 1h1a.5.5 0 0 0 0-1z"/>
            </svg>
          </button>
          <button v-if="sortDirection === 'desc'" @click="sort('dateOfBirth')">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-sort-up" viewBox="0 0 16 16">
              <path d="M3.5 12.5a.5.5 0 0 1-1 0V3.707L1.354 4.854a.5.5 0 1 1-.708-.708l2-1.999.007-.007a.5.5 0 0 1 .7.006l2 2a.5.5 0 1 1-.707.708L3.5 3.707zm3.5-9a.5.5 0 0 1 .5-.5h7a.5.5 0 0 1 0 1h-7a.5.5 0 0 1-.5-.5M7.5 6a.5.5 0 0 0 0 1h5a.5.5 0 0 0 0-1zm0 3a.5.5 0 0 0 0 1h3a.5.5 0 0 0 0-1zm0 3a.5.5 0 0 0 0 1h1a.5.5 0 0 0 0-1z"/>
            </svg>
          </button>
          </div>
        </th>
        <th scope="col">Phone Number</th>
        <th scope="col">Actions</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(gambler, index) of sortedGamblers" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ gambler.name }}</td>
        <td>{{ gambler.email }}</td>
        <td>{{ gambler.dateOfBirth }}</td>
        <td>{{ gambler.phoneNumber }}</td>
        <td>
          <button class="btn btn-sm btn-primary me-2" @click="$emit('edit-gambler', gambler)">Edit</button>
          <button class="btn btn-sm btn-danger" @click="$emit('delete-gambler', gambler)">Delete</button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import type { Gambler } from '@/gambler';

const props = defineProps<{
  gamblers: Gambler[]
}>();

const emit = defineEmits(['edit-gambler', 'delete-gambler']);

const sortField = ref('');
const sortDirection = ref('asc');

const sortedGamblers = computed(() => {
  return [...props.gamblers].sort((a, b) => {
    if (sortField.value === 'dateOfBirth') {
      return sortDirection.value === 'asc'
          ? new Date(a.dateOfBirth).getTime() - new Date(b.dateOfBirth).getTime()
          : new Date(b.dateOfBirth).getTime() - new Date(a.dateOfBirth).getTime();
    }
    if (a[sortField.value] < b[sortField.value]) return sortDirection.value === 'asc' ? -1 : 1;
    if (a[sortField.value] > b[sortField.value]) return sortDirection.value === 'asc' ? 1 : -1;
    return 0;
  });
});

const sort = (field: string) => {
  if (sortField.value === field) {
    sortDirection.value = sortDirection.value === 'asc' ? 'desc' : 'asc';
  } else {
    sortField.value = field;
    sortDirection.value = 'asc';
  }
};
</script>