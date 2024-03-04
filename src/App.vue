<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'
import indexHeader from './components/index-header.vue'
import footerComp from './components/footer-comp.vue'
import mainHeader from './components/main-header.vue'
import cartComp from './components/cart-comp.vue'
import shopList from './components/shop-list.vue'

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
        parentId: item.id
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

const items = ref([])

const cart = ref([])

const isCartOpen = ref(false)

const cartPrice = computed(() => cart.value.reduce((acc, item) => (acc += item.itemPrice), 0))

const orderSent = ref(false)

const lastOrderId = ref(null)

const sendAnOrder = async (orderedItems) => {
  try {
    const orderInfo = {
      boughtItems: [],
      itemsQuantity: orderedItems.length,
      orderPrice: cartPrice.value
    }

    orderedItems.map((item) => {
      ;(item.inCart = false), orderInfo.boughtItems.push(item.itemName)
    })

    const lastOrder = await axios.post('https://4aba6cffe4a51351.mokky.dev/orders', orderInfo)

    lastOrderId.value = lastOrder.data.id

    orderSent.value = !orderSent.value

    items.value = items.value.map((item) => ({
      ...item,
      inCart: false
    }))

    cart.value = []
  } catch (err) {
    console.log(err)
  }
}

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

  if ((orderSent.value = false)) {
    orderSent.value = true
  }
}

const deleteFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  items.value.map((deletedItem) => {
    if (deletedItem.id == item.id) {
      deletedItem.inCart = false
    }
  })
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  closeCart,
  openCart,
  cartManipulation,
  sendAnOrder,
  orderSent,
  lastOrderId
})

provide('cartPrice', { cartPrice })
</script>

<template>
  <transition name="fade">
    <cart-comp v-if="isCartOpen" />
  </transition>

  <div class="container w-full sm:w-3/4 m-auto rounded-3xl bg-white shadow-xl">
    <index-header :cartPrice="cartPrice" @open-cart="openCart" />
    <main class="mainWrapper w-3/4 m-auto">
      <main-header @search="searchForItem" @change-sort-parameter="changeSortParameter" />
      <shop-list
        @cart-manipulation="cartManipulation"
        @add-to-favorites="addToFavorites"
        :items="items"
      />
    </main>
    <footer-comp />
  </div>
</template>

<style scoped></style>
