<script setup lang="ts">
defineProps({
  title: { type: String, default: 'Julius' },
})
</script>

<template>
  <h1 class="text-4xl uppercase border-b-2">{{ title }}</h1>
</template>
