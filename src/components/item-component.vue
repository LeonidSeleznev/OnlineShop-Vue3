<script setup>
defineProps({
  imageUrl: String,
  itemName: String,
  itemPrice: Number,
  inCart: Boolean,
  inFavorites: Boolean,
  addToCartOnClick: Function,
  addToFavoritesOnClick: Function
})
</script>

<template>
  <div
    class="shopItem text-sm border-2 rounded-3xl hover transition hover:-translate-y-2 hover:shadow-xl"
  >
    <div class="itemWrapper w-36 relative m-auto py-5">
      <img
        @click="addToFavoritesOnClick"
        :src="inFavorites ? '/like-2.svg' : '/like-1.svg'"
        alt="favoriteBtn"
        class="absolute top-6 hover:cursor-pointer"
      />
      <img :src="imageUrl" alt="itemImage" class="max-h-[122px] max-w-36 hover:cursor-pointer" />
      <p class="my-3.5">{{ itemName }}</p>
      <div class="priceAndAddWrapper flex justify-between items-center">
        <div class="priceWrapper">
          <p class="text-gray-400">Цена:</p>
          <b>{{ itemPrice }} руб.</b>
        </div>
        <img
          @click="addToCartOnClick"
          :src="inCart ? '/checked.svg' : '/plus.svg'"
          alt="addBtn"
          class="hover:cursor-pointer"
        />
      </div>
    </div>
  </div>
</template>
