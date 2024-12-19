<script setup>
const emit = defineEmits(['openDrawer', 'openLogin', 'openProfile', 'getUserMiscInfo'])

defineProps({
  isAuth: Boolean,
  cartSummary: String,
})
</script>

<template>
  <header class="flex justify-between border-b border-indigo-100 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="./assets/logo.png" alt="Logo" class="w-10" />
      <div>
        <h2 class="text-xl font-bold uppercase">Vladimir's Pharmacy</h2>
        <p class="text-slate-400">Лучшая аптека в Антарктиде</p>
      </div>
    </div>

    <ul class="flex items-center gap-10">
      <li
        @click="() => (!isAuth ? emit('openLogin') : emit('openDrawer'))"
        class="flex items-center cursor-pointer gap-3 text-slate-500 hover:text-black"
      >
        <img src="./assets/cart.svg" alt="Cart" />
        <b>{{ cartSummary }} руб.</b>
      </li>

      <li
        @click="
          () => (!isAuth ? emit('openLogin') : [emit('openProfile'), emit('getUserMiscInfo')])
        "
        class="flex items-center cursor-pointer gap-3 text-slate-500 hover:text-black"
      >
        <img src="./assets/profile.svg" alt="Cart" />
        <span>{{ isAuth ? 'Профиль' : 'Войти' }}</span>
      </li>
    </ul>
  </header>
</template>
