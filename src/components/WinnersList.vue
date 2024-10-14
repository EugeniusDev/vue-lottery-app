<template>
  <div class="mb-3 mt-5 p-4 d-flex bg-white rounded">
    <div class="w-75 border border-secondary border-1 p-3 rounded">
      <div class="d-flex">
        <Winner
            v-for="(winner, index) in winners"
            :key="index"
            :winner="winner"
            @delete="$emit('delete-winner', $event)"
        />
        <span class="text-secondary p-1">Winners</span>
      </div>
    </div>
    <div class="w-25 d-flex justify-content-center align-items-center">
      <Button
          :disabled="winners.length === 3 || usersCount === 0"
          buttonClass="btn btn-primary"
          @click="$emit('new-winner')"
      >
        New Winner
      </Button>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Gambler } from '@/gambler';
import Winner from './Winner.vue';
import Button from './Button.vue';

defineProps<{
  winners: Gambler[];
  usersCount: number;
}>();
defineEmits(['delete-winner', 'new-winner']);
</script>