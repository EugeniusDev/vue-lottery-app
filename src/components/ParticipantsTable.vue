<template>
  <div class="mt-3 p-4 mb-5 border bg-white rounded">
    <table class="table rounded">
      <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">
          Name
          <button @click="sort('name')">
            <i :class="sortIcon('name')"></i>
          </button>
        </th>
        <th scope="col">Email</th>
        <th scope="col">
          Date of Birth
          <button @click="sort('dateOfBirth')">
            <i :class="sortIcon('dateOfBirth')"></i>
          </button>
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

const sortIcon = (field: string) => {
  if (sortField.value !== field) return 'fas fa-sort';
  return sortDirection.value === 'asc' ? 'fas fa-sort-up' : 'fas fa-sort-down';
};
</script>