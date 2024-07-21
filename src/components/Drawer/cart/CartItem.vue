<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
})
</script>

<template>
  <div class="relative flex w-full border border-slate-100 rounded-xl p-2 gap-4 justify-between">
    <img class="h-16 w-16" :src="imageUrl" alt="sneakers" />

    <div class="flex flex-col w-full gap-2">
      <p class="leading-4">{{ title }}</p>
      <div class="flex justify-between">
        <b>{{ price }} $</b>
        <img
          src="/public/close.svg"
          alt="close"
          class="opacity-40 hover:opacity-100 hover:-translate-y-1 transition"
        />
      </div>
    </div>
  </div>
</template>
