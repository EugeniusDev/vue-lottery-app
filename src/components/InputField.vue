<template>
  <div class="mb-3">
    <label :for="id" class="form-label fw-bold">{{ label }}</label>
    <input
        :id="id"
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', ($event.target as HTMLInputElement).value)"
        :class="{ 'is-invalid': invalid, 'is-valid': valid, 'form-control': true, 'bg-secondary-subtle': true }"
        :required="required"
        :placeholder="placeholder"
    >
    <div class="invalid-feedback">
      {{ errorMessage }}
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  id: string;
  label: string;
  type: string;
  modelValue: string | number | null;
  invalid: boolean;
  valid: boolean;
  required: boolean;
  placeholder: string;
  errorMessage: string;
}>();
defineEmits(['update:modelValue']);
</script>