<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  prise: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onclickAdded: Function,
  onclickFavorite: Function
})
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition 2s flex flex-col justify-between"
  >
    <img
      :src="!isFavorite ? '/public/like-1.svg' : '/public/like-2.svg'"
      alt="like2"
      @click="onclickFavorite"
      class="absolute top-8 left-8"
    />

    <img :src="imageUrl" alt="sneakers" />
    <p class="mt-3">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-500">Цена:</span>
        <b>{{ prise }} $</b>
      </div>

      <img
        :src="!isAdded ? '/public/plus.svg' : '/public/checked.svg'"
        alt="plus"
        @click="onclickAdded"
      />
    </div>
  </div>
</template>

<style scope>
</style>
