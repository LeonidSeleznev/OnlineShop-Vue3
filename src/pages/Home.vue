<script setup>
import { inject, onMounted, provide, reactive, watch } from 'vue'
import axios from 'axios'
import mainHeader from '../components/main-header.vue'
import shopList from '../components/shop-list.vue'

const items = inject('items')
const { cart, cartManipulation } = inject('cart')

onMounted(async () => {
  await fetchItems()
  const savedCartFromStorage = localStorage.getItem('cart')
  cart.value = savedCartFromStorage ? JSON.parse(savedCartFromStorage) : []
  items.value = items.value.map((item) => ({
    ...item,
    inCart: cart.value.some((cartItem) => cartItem.id == item.id)
  }))
})

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filterParameters.sortBy
    }

    if (filterParameters.searchedItem) {
      params.itemName = `*${filterParameters.searchedItem}*`
    }

    const { data } = await axios.get('https://4aba6cffe4a51351.mokky.dev/items', {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      inFavorites: false,
      favoriteId: null
    }))

    await fetchFavorites()
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
        parentId: item.id,
        item
      }
      item.inFavorites = true

      const { data } = await axios.post(
        'https://4aba6cffe4a51351.mokky.dev/favorites',
        favoriteItem
      )

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

const filterParameters = reactive({
  sortBy: 'itemName',
  searchedItem: ''
})

watch(filterParameters, fetchItems)

const changeSortParameter = async (sortParameter) => {
  filterParameters.sortBy = sortParameter
}

const searchForItem = (searchText) => {
  filterParameters.searchedItem = searchText
}

provide('addToFavorites', {
  addToFavorites
})
</script>

<template>
  <main-header @search="searchForItem" @change-sort-parameter="changeSortParameter" />
  <shop-list
    @cart-manipulation="cartManipulation"
    @add-to-favorites="addToFavorites"
    :items="items"
  />
</template>
