<template>
  <button
      :class="computedClasses"
      @click="handleClick"
      :disabled="disabled"
      :type="type"
  >
    <slot></slot>
  </button>
</template>

<script setup lang="ts">
import { computed } from 'vue';

const props = withDefaults(defineProps<{
  variant?: 'primary' | 'secondary' | 'danger' | 'success' | 'warning' | 'info';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  type?: 'button' | 'submit' | 'reset';
  outline?: boolean;
  block?: boolean;
}>(), {
  variant: 'primary',
  size: 'md',
  disabled: false,
  type: 'button',
  outline: false,
  block: false
});

const emit = defineEmits<{
  (e: 'click', event: MouseEvent): void;
}>();

const computedClasses = computed(() => {
  return [
    'btn',
    `btn-${props.size}`,
    props.outline ? `btn-outline-${props.variant}` : `btn-${props.variant}`,
    { 'btn-block': props.block },
    { 'disabled': props.disabled }
  ];
});

const handleClick = (event: MouseEvent) => {
  if (!props.disabled) {
    emit('click', event);
  }
};
</script>