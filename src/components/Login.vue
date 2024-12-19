<script setup>
import LoginBody from './LoginBody.vue'
import LoginHead from './LoginHead.vue'

const emit = defineEmits(['tryToLogin'])

defineProps({
  authFailed: Boolean,
})
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-50"></div>
  <div
    class="bg-white w-96 h-2/5 fixed inset-0 items-center justify-center w-1/6 m-auto rounded-xl shadow-xl mt-40 z-20 p-8"
  >
    <LoginHead />
    <LoginBody />
    <b class="flex mt-5 text-red-400 text-center">{{
      authFailed ? 'Войти не удалось. Неверный логин или пароль!' : ''
    }}</b>
    <button
      @click="() => emit('tryToLogin')"
      class="mt-14 bg-cyan-500 w-full rounded-2xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-cyan-600 transition active:bg-cyan-700"
    >
      Войти в аккаунт
    </button>
  </div>
</template>
