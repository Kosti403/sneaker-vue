<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})
</script>

<template>
  <div class="flex w-full border border-slate-100 rounded-xl p-2 gap-4 align-center">
    <img class="h-16 w-16" :src="imageUrl" alt="sneakers" />

    <div class="flex flex-col gap-2 w-full">
      <p class="leading-4">{{ title }}</p>
      <div class="flex justify-between">
        <b class="flex-1">{{ price }} $</b>
        <img
          @click="emit('onClickRemove')"
          src="/public/close.svg"
          alt="close"
          class="opacity-40 hover:opacity-100 hover:-translate-y-1 transition"
        />
      </div>
    </div>
  </div>
</template>
