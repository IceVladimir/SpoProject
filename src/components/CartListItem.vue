<script setup>
import CartItem from './CartItem.vue'

defineProps({
  cartItems: Array,
})
</script>

<template>
  <div class="flex flex-col gap-4 flex-1 justify-between" v-auto-animate>
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
    />
  </div>
</template>
