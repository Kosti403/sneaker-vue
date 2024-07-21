<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'
import HeaderComponent from './components/header/HeaderComponent.vue'
import CardList from './components/card/CardList.vue'
import DrawerComponent from './components/Drawer/DrawerComponent.vue'

const items = ref([])

const cart = ref([])

const drawerOpen = ref(false)

const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdd = true
}
const removedFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdd = false
}

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

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://b4e88eb3a4ce1ad6.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }
      return { ...item, isFavorite: true, favoriteId: favorite.id }
    })
  } catch (error) {
    throw new Error(error.response.data.message || 'Something went wrong')
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
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

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)

provide(`cart`, {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removedFromCart
})
</script>

<template>
  <DrawerComponent v-if="drawerOpen" :totalPrice="totalPrice" :vatPrice="vatPrice" />
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <HeaderComponent :totalPrice="totalPrice" @open-drawer="openDrawer" />

    <section class="p-10">
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
      <div class="mt-10"></div>
      <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
    </section>
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
