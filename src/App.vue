<script setup>
// COMPOSITION API
import CardList from './components/CardList.vue'
import Header from './components/Header.vue'
</script>

<template>
  <!-- <Drawer /> -->
  <div class="w-4/5 bg-gray-50 mx-auto rounded-xl shadow-2xl mt-14 pb-5">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Всi кросiвки</h2>

        <div class="flex justify-between gap-10">
          <select class="py-2 px-3 border rounded-md outline-none">
            <option>По названию</option>
            <option>По цене (дешевые)</option>
            <option>По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3 cursor-pointer" src="/search.svg" alt="" />
            <input
              class="border border-gray-200 rounded-md py-2 pl-12 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>

      <CardList />
    </div>
  </div>
</template>

<style scoped>
</style>
