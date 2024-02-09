<script setup>
import { inject } from 'vue'
import cartHeader from './cart-header.vue'
import cartItems from './cart-items.vue'
import cartOrderInfo from './cart-order-info.vue'

const { cart, deleteFromCart } = inject('cart')
</script>

<template>
  <div class="cartWrapper fixed top-0 left-0 w-full h-full flex z-10">
    <div class="cartTransparent flex-1 bg-black opacity-60"></div>
    <div class="cart bg-white w-96 h-full p-8 flex flex-col">
      <cart-header />
      <div class="cartMain flex items-center flex-col gap-y-2">
        <cart-items
          v-for="item in cart"
          :key="item.id"
          :image-url="item.imageUrl"
          :item-name="item.itemName"
          :item-price="item.itemPrice"
          @delete-from-cart="deleteFromCart(item)"
        />
      </div>

      <div class="divider min-h-1 flex-1"></div>
      <cart-order-info />
    </div>
  </div>
</template>
