<script setup>
import { defineEmits } from 'vue'
const emits = defineEmits(['changeSortParameter', 'search'])

</script>

<template>
  <div
    class="mainHeader my-10 flex flex-col justify-center items-center xl:flex-row xl:justify-between"
  >
    <b class="block text-xl lg:text-3xl mb-4 xl:mb-0">Все кроссовки</b>

    <div
      class="headerFilters md:w-full xl:w-auto xl:gap-x-4 md:flex md:flex-row md:justify-between xl:justify-evenly"
    >
      <div class="filterWrapper relative">
        <img
          class="filterIcon absolute fill-slate-400 top-2 left-3 lg:top-3.5"
          src="/public/filter.svg"
          alt="filter"
        />
        <select
          @change="(event) => emits('changeSortParameter', event.target.value)"
          class="filter cursor-pointer bg-white h-8 w-48 pl-8 mb-4 md:mb-0 border-2 rounded-xl pr-1 outline-none transition border-gray-300 sm:mt-0 lg:h-11 lg:w-60"
        >
          <option value="itemPrice">По цене(↓)</option>
          <option value="-itemPrice">По цене(↑)</option>
          <option value="itemName">По названию</option>
        </select>
      </div>
      <div class="searchWrapper relative">
        <img src="/public/search.svg" alt="" class="absolute top-2 left-3 lg:top-3.5 lg:left-3.5" />
        <input
          @input="(submitEvent) => emits('search', submitEvent.target.value)"
          type="text"
          name="search"
          id="#"
          placeholder="Поиск..."
          class="h-8 w-48 border-2 rounded-xl pl-8 pr-1 outline-none transition border-gray-300 focus:border-lime-500 sm:mt-0 lg:h-11 lg:w-60 lg:pl-10"
        />
      </div>
    </div>
  </div>
</template>
