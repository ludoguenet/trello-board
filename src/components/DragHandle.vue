<script setup lang="ts">
defineProps<{
  icon: string,
}>();
</script>

<template>
  <span class="drag-handle cursor-grab">{{ icon }}</span>
</template>