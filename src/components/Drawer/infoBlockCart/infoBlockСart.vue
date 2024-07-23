<script setup>
defineProps({
  title: String,
  descriptions: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center text-center w-96 mx-auto">
    <img :src="imageUrl" :alt="title" height="90" width="90" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-500">{{ descriptions }}</p>
  </div>
  
</template>
