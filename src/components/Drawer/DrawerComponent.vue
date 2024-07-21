<script setup>
import { inject } from 'vue';
import CartItemList from './cart/CartItemList.vue'
const {closeDrawer} = inject('cart')
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>

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

    <CartItemList />

    <div class="flex flex-col gap-4 mt-5 my-5">
      <div class="flex gap-2">
        <span>Итого:</span>
        <span class="flex-1 border-b border-dashed mb-1"></span>
        <b>12 222 $</b>
      </div>

      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <span class="flex-1 border-b border-dashed mb-1"></span>
        <b>20 $</b>
      </div>

      <button
        disabled="false"
        class="bg-lime-500 w-full rounded-xl py-3 disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-800 transition cursor-pointer"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
