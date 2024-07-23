<script setup>
import { computed, provide, ref, watch } from 'vue'

import HeaderComponent from './components/header/HeaderComponent.vue'
import DrawerComponent from './components/Drawer/DrawerComponent.vue'

const cart = ref([])

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))
const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdd = true
}

const removedFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdd = false
}

provide(`cart`, {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removedFromCart
})

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)
</script>

<template>
  <DrawerComponent v-if="drawerOpen" :totalPrice="totalPrice" :vatPrice="vatPrice" />
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <HeaderComponent :totalPrice="totalPrice" @open-drawer="openDrawer" />

    <section class="p-10">
      <router-view></router-view>
    </section>
  </div>
</template>
