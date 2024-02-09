<script setup>
const emits = defineEmits(['openCart'])

defineProps({
  cartPrice: Number
})
</script>

<template>
  <header class="header border-b-2 2xl: justify-around">
    <div
      class="headerContainer flex flex-col py-8 sm:mt-6 lg:flex lg:flex-row lg:justify-center 2xl:justify-around"
    >
      <div class="logo flex justify-center items-center lg:justify-between">
        <a href="#" class="logo w-10 h-10">
          <img src="/public/logo.png" alt="logoImage" loading="lazy"
        /></a>
        <div class="logoText ml-2">
          <h2 class="text-xl"><b>МАГАЗИН КРОССОВОК</b></h2>
          <p class="text-gray-400">Ваш любимый магазин</p>
        </div>
      </div>
      <nav
        class="headerNav flex justify-center items-center mt-5 lg:mt-0 xl:justify-between xl:ml-28 2xl:ml-72"
      >
        <ul class="flex items-center gap-3 sm:flex-row sm:gap-8 lg:ml-6 lg:gap-4 xl:gap-8 xl:ml-0">
          <li
            @click="emits('openCart')"
            class="flex items-center gap-2.5 cursor-pointer text-gray-400 hover:text-black"
          >
            <img src="/public/cart.svg" alt="cart" class="transition hover:rotate-45" /><b
              >{{ cartPrice }} руб.</b
            >
          </li>
          <li class="flex items-center gap-2.5 cursor-pointer text-gray-400 hover:text-black">
            <img src="/public/heart.svg" alt="cart" />Закладки
          </li>
          <li class="flex items-center gap-2.5 cursor-pointer text-gray-400 hover:text-black">
            <img src="/public/profile.svg" alt="cart" />Профиль
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>
