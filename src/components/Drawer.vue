<script setup>
import DrawerHead from './DrawerHead.vue'
import CartListItem from './CartListItem.vue'
import InfoBlock from './InfoBlock.vue'

defineProps({
  cartItems: Array,
  cartSummary: Number,
  cartDiscount: Number,
})
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-50"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8" v-auto-animate>
    <DrawerHead />
    <div v-if="cartSummary === '0'" class="flex h-full items-center">
      <InfoBlock title="Упс! Ваша корзина сейчас пуста!"> </InfoBlock>
    </div>
    <div v-else>
      <CartListItem :cartItems="cartItems" />

      <div class="flex flex-col gap-4 mb-6 my-7 mt-7">
        <div class="flex gap-4">
          <span>Скидка:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ cartDiscount }} руб.</b>
        </div>

        <div class="flex gap-4">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ cartSummary }} руб.</b>
        </div>

        <button
          disabled=""
          class="mt-4 bg-cyan-500 w-full rounded-2xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-cyan-600 transition active:bg-cyan-700"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
