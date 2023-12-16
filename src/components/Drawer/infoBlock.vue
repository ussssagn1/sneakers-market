<script setup>
defineProps({
  title: String,
  desc: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto">
    <img :src="imageUrl" alt="CartInfoImg" class="h-32 w-32" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400 mt-2">{{ desc }}</p>
  </div>
</template>