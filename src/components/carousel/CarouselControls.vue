<template>
  <!-- in vue 2.x you need wrap these buttons in a div -->
  <button
    class="absolute left-2 mt-52 bg-slate-400 text-white opacity-60 hover:opacity-100 rounded-md justify-center items-center px-4 py-3 disabled:bg-slate-300 cursor-pointer hover:bg-slate-600 transition active:bg-slate-700"
    @click="$emit('prev')"
  >
    ПРЕДЫДУЩЕЕ
  </button>
  <button
    class="absolute right-2 mt-52 bg-slate-400 text-white opacity-60 hover:opacity-100 rounded-md align-middle px-4 py-3 disabled:bg-slate-300 cursor-pointer hover:bg-slate-600 transition active:bg-slate-700"
    @click="$emit('next')"
  >
    СЛЕДУЮЩЕЕ
  </button>
</template>

<script>
export default {
  emits: ['prev', 'next'], // vue v3
}
</script>
<style scoped>
.carousel-control {
  background-color: rgba(0, 0, 0, 0.5);
  border: none;
  display: inline-block;
  position: absolute;
  height: 50px;
  width: 120px;
  top: calc(50% - 25px);
  color: #f3f3f3;
  cursor: pointer;
}
.left {
  left: 0;
}
.right {
  right: 0;
}
</style>
