<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
})
</script>

<template>
  <div class="grid grid-cols-5 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.name"
      :discount="item.discount"
      :imageUrl="item.imageUrl"
      :price="item.price"
    />
  </div>
</template>
