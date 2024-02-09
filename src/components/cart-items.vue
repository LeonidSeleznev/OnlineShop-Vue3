<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  itemName: String,
  itemPrice: Number
})
const emits = defineEmits(['deleteFromCart'])
</script>

<template>
  <div class="cartItem flex justify-between items-center h-32 w-full border-2 rounded-2xl p-5">
    <img :src="imageUrl" alt="itemImg" class="w-16 h-16" />

    <div class="cartBottom flex flex-col flex-1 ml-2">
      <h2 class="text-sm">{{ itemName }}</h2>
      <div class="flex items-center justify-between mt-2">
        <b>{{ itemPrice }} руб.</b>
        <img
          @click="emits('deleteFromCart')"
          src="/public/close.svg"
          alt="closeBtn"
          class="transition opacity-50 hover:opacity-100 cursor-pointer"
        />
      </div>
    </div>
  </div>
</template>
