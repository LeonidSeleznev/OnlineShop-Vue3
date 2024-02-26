<script setup>
import { inject } from 'vue';

    const { cart, closeCart } = inject('cart')
</script>

<template>
  <div
    v-if="cart.length == 0"
    class="cartMessage flex flex-col items-center text-center m-auto max-w-60"
  >
    <img src="/public/package-icon.png" alt="box-icon" class="max-w-[120px] max-h-[120px] mb-5" />
    <b class="text-2xl mb-2.5">Корзина пуста</b>
    <p class="text-base/4 text-slate-400 mb-10">
      Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ.
    </p>
    <button
      @click="closeCart"
      class="cursor-pointer w-full transition bg-lime-500 disabled:bg-slate-300 text-white h-12 rounded-xl hover:bg-lime-600 active:bg-lime-700 font-semibold flex justify-center items-center"
    >
      <svg
        class="rotate-180 transition cursor-pointer mr-4"
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="14"
        viewBox="0 0 16 14"
        fill="none"
      >
        <path
          d="M1 7H14.7143"
          stroke="white"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M8.71436 1L14.7144 7L8.71436 13"
          stroke="white"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
      Вернуться назад
    </button>
  </div>
</template>
