<script setup>
defineProps({
  totalPrice: Number
})
const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-100 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <div class="">
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-500">Магазин кращих кросiвок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black transition-all"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} $</b>
      </li>
      <router-link to="/favorites">
        <li
        class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black transition-all"
      >
        <img src="/heart.svg" alt="Cart" />
        <span>Закладки</span>
      </li>
      </router-link>
      <li
        class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black transition-all"
      >
        <img src="/profile.svg" alt="Cart" />
        <span>Профiль</span>
      </li>
    </ul>
  </header>
</template>

<script setup>
</script>