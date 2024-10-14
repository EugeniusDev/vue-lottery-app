<template>
  <span class="rounded text-white bg-primary p-1 ms-1 me-1">
    {{ winner.name }}
    <b role="button" @click="$emit('delete', winner)" class="fw-bold">X</b>
  </span>
</template>

<script setup lang="ts">
import type { Gambler } from '@/gambler';

defineProps<{
  winner: Gambler
}>();
defineEmits(['delete']);
</script>