<script setup>
defineProps({
  name: String,
  email: String,
})
</script>

<template>
  <div class="flex flex-col gap-4 flex-1 justify-between">
    <h2 class="mt-4 text-2xl font-light font-sans">ИМЯ: {{ name }}</h2>
    <h2 class="mt-4 text-2xl font-light font-sans">EMAIL: {{ email }}</h2>
  </div>
</template>
