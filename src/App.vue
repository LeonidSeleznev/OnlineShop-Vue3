<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'
import indexHeader from './components/index-header.vue'
import footerComp from './components/footer-comp.vue'
import cartComp from './components/cart-comp.vue'

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

provide('items', items)
</script>

<template>
  <transition name="fade">
    <cart-comp v-if="isCartOpen" />
  </transition>

  <div class="container w-full sm:w-3/4 m-auto rounded-3xl bg-white shadow-xl">
    <index-header :cartPrice="cartPrice" @open-cart="openCart" />
    <main class="mainWrapper flex flex-col w-3/4 m-auto">
      <router-view></router-view>
    </main>
    <footer-comp />
  </div>
</template>