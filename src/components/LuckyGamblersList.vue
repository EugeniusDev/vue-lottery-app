<script setup lang="ts">
import type {Gambler} from "@/gambler";

defineProps<{
  luckers: Gambler[],
  usersCount: number
}>();

const emits = defineEmits(['new-lucker', 'delete-lucker']);

const deleteLucker = (lucker: Gambler) => {
  emits('delete-lucker', {...lucker});
}
</script>

<template>
<div class="mb-3 mt-5 p-4 d-flex bg-white rounded">
  <div class="w-75 border border-secondary border-1 p-3 rounded">
    <div class="d-flex">
      <span class="rounded text-white bg-primary p-1 ms-1 me-1" v-for="(lucker, index) of luckers" :key="index">
        {{lucker.name}}
        <b role="button" @click="() => deleteLucker(lucker)" class="fw-bold">X</b>
      </span>
      <span class="text-secondary p-1">Winners</span>
    </div>
  </div>
  <div class="w-25 d-flex justify-content-center align-items-center">
    <button :disabled="luckers.length === 3 || usersCount === 0" class="btn btn-primary" @click="$emit('new-lucker')">New Winner</button>
  </div>
</div>
</template>

<style scoped></style>