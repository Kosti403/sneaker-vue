<script setup>
import CardSneaker from './CardSneaker.vue'
defineProps({
  items: Array
})
</script>

<template>
  <div class="grid grid-cols-3 gap-5">
    <CardSneaker
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :prise="item.price"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
