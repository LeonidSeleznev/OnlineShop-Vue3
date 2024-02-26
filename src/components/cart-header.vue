<script setup>
import { inject } from 'vue'

const { closeCart } = inject('cart')
</script>

<template>
  <div class="cartHeader flex items-center gap-3 mb-7">
    <svg
      @click="closeCart"
      class="opacity-50 rotate-180 transition hover:opacity-100 cursor-pointer"
      xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="14"
      viewBox="0 0 16 14"
      fill="none"
    >
      <path
        d="M1 7H14.7143"
        stroke="black"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
      <path
        d="M8.71436 1L14.7144 7L8.71436 13"
        stroke="black"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      />
    </svg>
    <h1 class="font-bold text-xl">Корзина</h1>
  </div>
</template>
