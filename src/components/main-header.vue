<template>
  <div
    class="mainHeader my-10 flex flex-col justify-center items-center sm:flex-row sm:justify-between"
  >
    <b class="block text-xl lg:text-3xl mb-4 sm:mb-0">Все кроссовки</b>
    <div class="searchWrapper relative">
      <img src="/public/search.svg" alt="" class="absolute top-2 left-3 lg:top-3.5 lg:left-3.5" />
      <input
        name="search"
        id="#"
        placeholder="Поиск..."
        class="h-8 w-40 border-2 rounded-xl pl-8 pr-1 outline-none transition border-gray-300 focus:border-lime-500 sm:mt-0 lg:h-11 lg:w-60 lg:pl-10"
      />
    </div>
  </div>
</template>
