<script setup>
import ProfileHead from './ProfileHead.vue'
import ProfileBody from './ProfileBody.vue'

const emit = defineEmits(['tryToLogout', 'tryToDeleteAccount'])

defineProps({
  miscInfo: Array,
})
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-50"></div>
  <div
    class="bg-white w-96 h-2/5 fixed inset-0 items-center justify-center w-2/5 m-auto rounded-xl shadow-xl mt-40 z-20 p-8"
  >
    <ProfileHead />
    <ProfileBody :name="miscInfo.name" :email="miscInfo.email" />
    <div class="flex">
      <button
        @click="() => emit('tryToLogout')"
        class="absolute bottom-5 left-5 bg-red-400 w-1/4 rounded-2xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-red-500 transition active:bg-red-900"
      >
        Выйти из аккаунта
      </button>

      <button
        @click="() => emit('tryToDeleteAccount')"
        class="absolute bottom-5 right-5 bg-red-400 w-1/4 rounded-2xl py-3 text-white disabled:bg-slate-300 cursor-pointer hover:bg-red-500 transition active:bg-red-900"
      >
        Удалить аккаунт
      </button>
    </div>
  </div>
</template>
