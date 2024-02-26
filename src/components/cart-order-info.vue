<script setup>
import { computed, inject } from 'vue'

const { cartPrice } = inject('cartPrice')
const { cart, sendAnOrder } = inject('cart')
const tax = computed(() => {
  return Math.floor(cartPrice.value * 0.05)
})
</script>

<template>
  <div class="cartInfo flex flex-col gap-3">
    <div class="checkoutPrice flex justify-between">
      <p>Итого:</p>
      <b>{{ cartPrice }} руб.</b>
    </div>
    <div class="taxes flex justify-between">
      <p>Налог 5%:</p>
      <b>{{ tax }} руб.</b>
    </div>
    <button
      @click="sendAnOrder(cart)"
      class="cursor-pointer w-full transition bg-lime-500 disabled:bg-slate-300 text-white h-12 rounded-xl hover:bg-lime-600 active:bg-lime-700"
    >
      Оформить заказ
    </button>
  </div>
</template>
