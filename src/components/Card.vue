<script setup>
import { inject } from 'vue'

const { addItemToCart } = inject('cartActions')

defineProps({
  imageUrl: String,
  title: String,
  price: Number,
  discount: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function,
})
</script>

<template>
  <div>
    <div
      class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
    >
      <img :src="imageUrl" alt="Drug" class="w-52" />
      <p class="mt-2">{{ title }}</p>

      <div class="flex justify-between mt-5">
        <div class="flex flex-col">
          <span class="text-slate-400">Цена:</span>
          <b>{{ price }} руб.</b>
        </div>
        <img
          class="rounded-2xl w-16 h-16 cursor-pointer transition hover:shadow-xl cursor-pointer transition"
          @click="() => addItemToCart(title, price, imageUrl, discount)"
          :src="!isAdded ? '/plus.svg' : 'checked.svg'"
          alt="Plus"
        />
      </div>
    </div>
  </div>
</template>
