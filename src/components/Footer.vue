<template>
  <div class="bg-warning text-center p-2">
    <p class="m-0">&copy; 2024 | {{ name }} | Bolivia</p>
  </div>
</template>

<script setup>
defineProps({
  name: {
    type: String,
    required: true,
  },
});
</script>

<style scoped>
</style>

