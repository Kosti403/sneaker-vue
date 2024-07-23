<script setup>
import { inject, onMounted, reactive, ref, watch } from 'vue'
import CardList from '../components/card/CardList.vue'
import axios from 'axios'
import debounce from 'lodash.debounce';

const items = ref([])

const { addToCart, cart, removedFromCart } = inject('cart')

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})
const onClickAddPlus = (item) => {
  if (!item.isAdd) {
    addToCart(item)
  } else {
    removedFromCart(item)
  }
  console.log(cart)
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 300)
const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post('https://b4e88eb3a4ce1ad6.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://b4e88eb3a4ce1ad6.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    throw new Error(err.response.data.message || 'Something went wrong')
  }
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://b4e88eb3a4ce1ad6.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)
      if (!favorite) {
        return item
      }
      return { ...item, isFavorite: true, favoriteId: favorite.id }
    })
  } catch (error) {
    throw new Error(error.response.data.message || 'Something went wrong')
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://b4e88eb3a4ce1ad6.mokky.dev/items', {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdd: false
    }))
  } catch (error) {
    throw new Error(error.response.data.message || 'Something went wrong')
  }
}

watch(filters, fetchItems)

onMounted(async () => {
  const localCard = localStorage.getItem('cart')
  cart.value = localCard ? JSON.parse(localCard) : []

  await fetchItems()
  await fetchFavorites()
  items.value = items.value.map((item) => ({
    ...item,
    isAdd: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})
</script>

<template>
  <div class="flex justify-between items-center mainSection">
    <h2 class="text-3xl font-bold px-10 py-5">All Sneakers</h2>
    <div class="relative">
      <img src="/public/search.svg" alt="" class="absolute left-0 top-4 align-center" />
      <input
        @input="onChangeSearchInput"
        type="text"
        placeholder="Поиск..."
        class="border border-slate-200 px-4 py-2 pl-11 pr-4 rounded-md outline-none focus:border-gray-400 align-text-top"
      />
    </div>
    <select @change="onChangeSelect" class="py-2 px-3 border border-slate-200 outline-none">
      <option value="title">По названию</option>
      <option value="price">По цене (дешевые)</option>
      <option value="-price">По цене (дорогие)</option>
    </select>
  </div>
  <div class="mt-10">
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
  </div>
</template>

<style scoped>
@media screen and (max-width: 1100px) {
  .mainSection {
    flex-direction: column;
    gap: 15px;
  }
}
</style>
