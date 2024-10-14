<template>
  <div class="mb-3">
    <input
        type="text"
        class="form-control"
        placeholder="Search by name"
        v-model="searchTerm"
        @input="emitFilterEvent"
    >
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const searchTerm = ref('');
const emit = defineEmits(['filter-by-name']);

const emitFilterEvent = () => {
  emit('filter-by-name', searchTerm.value);
};
</script>