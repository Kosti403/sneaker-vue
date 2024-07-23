<script setup>
import { computed, inject, ref } from 'vue'
import CartItemList from './cart/CartItemList.vue'
import infoBlockCart from './infoBlockCart/infoBlockСart.vue'
import axios from 'axios'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})
const isCreateOrder = ref(false)
const orderId = ref(null)
const createOrder = async () => {
  try {
    isCreateOrder.value = true
    const { data } = await axios.post('https://b4e88eb3a4ce1ad6.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })
    cart.value = []
    orderId.value = data.id
  } catch (error) {
    throw new Error(error.response.data.message || 'Something went wrong')
  } finally {
    isCreateOrder.value = false
  }
}

const { cart, closeDrawer } = inject('cart')

const cartIsEmpty = computed(() => cart.value.length === 0)

const cartDisabledOrder = computed(() => isCreateOrder.value || cartIsEmpty.value)
</script>

<template>
  <div @click="closeDrawer" class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>

  <div class="bg-white w-96 fixed right-0 top-0 z-20 p-8 h-full">
    <div class="flex items-center gap-4 mb-5">
      <img
        @click="closeDrawer"
        class="cursor-pointer opacity-50 rotate-180 hover:opacity-100 transition hover:-translate-x-1"
        src="/public/arrow-back.svg"
        alt="closeCart"
      />
      <h2 class="text-2xl font-bold">Корзина</h2>
    </div>
    <div v-if="!totalPrice || orderId" class="flex h-full items-center justify-center">
      <infoBlockCart
        v-if="orderId"
        title="Заказ Оформлен!!"
        :descriptions="`Ваш заказ номер №${orderId} скоро будет передан службе доставки`"
        imageUrl="/public/order-success-icon.png"
      />
      <infoBlockCart
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        descriptions="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
        imageUrl="/public/package-icon.png"
      />
    </div>
    <div v-else>
      <CartItemList />

      <div class="flex flex-col gap-4 mt-5 my-5">
        <div class="flex gap-2">
          <span>Итого:</span>
          <span class="flex-1 border-b border-dashed mb-1"></span>
          <b>{{ totalPrice }} $</b>
        </div>

        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <span class="flex-1 border-b border-dashed mb-1"></span>
          <b>{{ vatPrice }} $</b>
        </div>

        <button
          @click="createOrder"
          :disabled="cartDisabledOrder"
          class="bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-800 transition cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
