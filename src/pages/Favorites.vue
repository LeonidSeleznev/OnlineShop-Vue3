<script setup>
import ShopList from '@/components/shop-list.vue'
import axios from 'axios'
import { computed, inject, onMounted, provide, reactive, ref, watch } from 'vue'

const favorites = ref([])

const { cartManipulation } = inject('cart')

onMounted(async () => {
  try {
    const { data } = await axios.get('https://4aba6cffe4a51351.mokky.dev/favorites')

    favorites.value = data.map((obj) => (obj = obj.item))
    console.log(favorites)
  } catch (err) {
    console.log(err)
  }
})


</script>

<template>
  <div
    class="mainHeader my-10 flex flex-col justify-center items-center xl:flex-row xl:justify-between"
  >
    <b class="block text-xl lg:text-3xl mb-4 xl:mb-0">Желаемое</b>
  </div>
  <shop-list
    @cart-manipulation="cartManipulation"
    :items="favorites"
  />
</template>
