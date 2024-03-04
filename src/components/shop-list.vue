<script setup>
import itemComponent from './item-component.vue'
defineProps({
  items: Array
})

const emits = defineEmits(['cartManipulation', 'addToFavorites'])
</script>

<template>
  <div
    v-auto-animate
    class="itemsWrapper grid grid-cols-1 gap-3 sm:gap-2 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 xl:gap-5"
  >
    <item-component
      v-for="item in items"
      :key="item.id"
      :item-name="item.itemName"
      :item-price="item.itemPrice"
      :image-url="item.imageUrl"
      :addToCartOnClick="() => emits('cartManipulation', item)"
      :addToFavoritesOnClick="() => emits('addToFavorites', item)"
      :in-cart="item.inCart"
      :in-favorites="item.inFavorites"
    />
  </div>
</template>
