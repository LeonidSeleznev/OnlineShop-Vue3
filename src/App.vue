<script setup>
import { computed, onMounted, provide, ref } from 'vue'
import axios from 'axios'
import indexHeader from './components/index-header.vue'
import footerComp from './components/footer-comp.vue'
import mainHeader from './components/main-header.vue'
import cartComp from './components/cart-comp.vue'
import shopList from './components/shop-list.vue'

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

const fetchItems = async () => {
  try {
    const { data } = await axios.get('https://4aba6cffe4a51351.mokky.dev/items')
    items.value = data.map((obj) => ({
      ...obj,
      inFavorites: false,
      favoriteId: null,
      inCart: false
    }))
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://4aba6cffe4a51351.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        inFavorites: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavorites = async (item) => {
  try {
    if (!item.inFavorites) {
      const favoriteItem = {
        parentId: item.id
      }
      item.inFavorites = true

      const { data } = await axios.post('https://4aba6cffe4a51351.mokky.dev/favorites', favoriteItem)

      item.favoriteId = data.id
    } else {
      deleteFromFavorites(item)
    }
  } catch (err) {
    console.log(err)
  }
}

const deleteFromFavorites = async (item) => {
  item.inFavorites = false

  await axios.delete(`https://4aba6cffe4a51351.mokky.dev/favorites/${item.favoriteId}`)
}

const items = ref([])

const cart = ref([])

const isCartOpen = ref(false)

const cartPrice = computed(() => cart.value.reduce((acc, item) => (acc += item.itemPrice), 0))

const openCart = () => {
  isCartOpen.value = true
}

const closeCart = () => {
  isCartOpen.value = false
}

const cartManipulation = (item) => {
  if (!item.inCart) {
    addToCart(item)
  } else {
    deleteFromCart(item)
  }
}

const addToCart = (item) => {
  cart.value.push(item)
  item.inCart = true
}

const deleteFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.inCart = false
}

provide('cart', {
  cart,
  closeCart,
  openCart,
  addToCart,
  deleteFromCart
})

provide('cartPrice', {cartPrice})
</script>

<template>
  <cart-comp v-if="isCartOpen" />
  <div class="container w-full sm:w-3/4 m-auto rounded-3xl bg-white shadow-xl">
    <index-header :cartPrice="cartPrice" @open-cart="openCart" />
    <main class="mainWrapper w-3/4 m-auto">
      <main-header />
      <shop-list
        @add-to-cart="cartManipulation"
        @add-to-favorites="addToFavorites"
        :items="items"
      />
    </main>
    <footer-comp />
  </div>
</template>

<style scoped></style>
