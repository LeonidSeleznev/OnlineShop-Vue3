<script setup>
const emits = defineEmits(['openCart'])

defineProps({
  cartPrice: Number
})
</script>

<template>
  <header class="header border-b-2 flex flex-center justify-around">
    <div
      class="headerContainer flex flex-col py-8 lg:justify-between md:w-3/4 xl:flex xl:flex-row 2xl:justify-around"
    >
      <router-link to="/">
        <div class="logo flex justify-center items-center">
          <a href="#" class="logo w-10 h-10">
            <img src="/public/logo.png" alt="logoImage" loading="lazy"
          /></a>
          <div class="logoText ml-2">
            <h2 class="text-xl"><b>МАГАЗИН КРОССОВОК</b></h2>
            <p class="text-gray-400">Ваш любимый магазин</p>
          </div>
        </div>
      </router-link>

      <div class="divider hidden lg:block lg:flex-1"></div>
      <nav class="headerNav flex justify-center items-center mt-5 xl:mt-0 xl:justify-between">
        <ul class="flex items-center gap-3 sm:flex-row sm:gap-8 xl:gap-8 xl:ml-0">
          <li
            @click="emits('openCart')"
            class="flex items-center gap-2.5 cursor-pointer text-gray-400 hover:text-black"
          >
            <img src="/public/cart.svg" alt="cart" class="transition hover:rotate-45" /><b
              >{{ cartPrice }} руб.</b
            >
          </li>
          <router-link to="/favorites">
            <li class="flex items-center gap-2.5 cursor-pointer text-gray-400 hover:text-black">
              <img src="/public/heart.svg" alt="cart" />Желаемое
            </li>
          </router-link>
          <li class="flex items-center gap-2.5 cursor-pointer text-gray-400 hover:text-black">
            <img src="/public/profile.svg" alt="cart" />Профиль
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>
