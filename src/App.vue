<script setup>
// COMPOSITION API
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue';
import Header from './components/Header.vue'
</script>

<template>
  <Drawer />
  <div class="w-4/5 bg-green-50 mx-auto rounded-xl shadow-2xl mt-14 pb-5">
    <Header />
    <div class="p-10">
      <h2 class="text-3xl font-bold mb-8">Всi кросiвки</h2>
      <CardList />
    </div>
  </div>
</template>

<style scoped>
</style>
