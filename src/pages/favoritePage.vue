<script setup>
import axios from 'axios'
import CardList from '../components/card/CardList.vue'

import { onMounted, ref } from 'vue'
const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://b4e88eb3a4ce1ad6.mokky.dev/favorites?_relations=items'
    )
    favorites.value = data.map((obj) => obj.item)
  } catch (error) {
    throw new Error(error.response.data.message || 'Something went wrong')
  }
})
</script>

<template>
    <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
    <CardList :items="favorites" is-favorites />

</template>
