<script setup>
import { inject } from 'vue'

const { deleteItemFromCart } = inject('cartActions')

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
})
</script>

<template>
  <div class="flex items-center border border-slate-100 p-4 rounded-xl gap-4">
    <img :src="imageUrl" alt="Drug" class="w-16 h-16" />

    <div class="flex flex-col">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b>{{ price }} руб.</b>
        <img
          @click="() => deleteItemFromCart(id)"
          class="opacity-40 hover:opacity-100 cursor-pointer transition absolute right-12"
          src="/close.svg"
        />
      </div>
    </div>
  </div>
</template>
