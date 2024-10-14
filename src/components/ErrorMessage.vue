<template>
  <div v-if="message" class="error-message alert alert-danger" role="alert">
    {{ message }}
  </div>
</template>

<script setup lang="ts">
defineProps<{
  message: string
}>();
</script>

<style lang="scss" scoped>
.error-message {
  margin-top: 1rem;
  margin-bottom: 1rem;
}
</style>