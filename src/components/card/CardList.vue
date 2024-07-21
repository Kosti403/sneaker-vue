<script setup>
import CardSneaker from './CardSneaker.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['addToFavorite'])
</script>

<template>
  <div class="grid grid-cols-3 gap-5 sneakerGrid">
    <CardSneaker
      v-for="item in items"
      :id="item.id"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :prise="item.price"
      :isFavorite="item.isFavorite"
      :onclickFavorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>


<style scoped>
@media screen and (max-width: 1100px) {
  .sneakerGrid{
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>