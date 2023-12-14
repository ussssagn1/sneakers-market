<script setup>
// COMPOSITION API
import { onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

import CardList from './components/CardList.vue'
import Header from './components/Header.vue'

// В Componition API ref(i) -> i.value
// В Options API data() { i: 1 } -> this.i

const itemsData = ref([])

const filterd = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (e) => {
  filterd.sortBy = e.target.value
}
const onChangeSearchInput = (e) => {
  filterd.searchQuery = e.target.value
}
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filterd.sortBy
    }
    if (filterd.searchQuery) {
      params.title = `*${filterd.searchQuery}*`
    }
    const { data } = await axios.get('https://70d8ccb0e330bcf6.mokky.dev/items', {
      params
    })
    itemsData.value = data
  } catch (err) {
    console.log(`Ошибка при получении данных ${err}`)
  }
}

// ПРИ ПЕРВОМ РЕНДЕРЕ ДЕЛАТЬ ЧТО-ТО === onMOUNTED
onMounted(fetchItems)

// ПРИ ОТСЛЕЖИВАНИИ ЧЕГО ЛИБО
watch(filterd, fetchItems)
</script>

<template>
  <!-- <Drawer /> -->
  <div class="w-4/5 bg-gray-50 mx-auto rounded-xl shadow-2xl mt-14 pb-5">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center mb-10">
        <h2 class="text-3xl font-bold mb-8">Всi кросiвки</h2>

        <div class="flex justify-between gap-10">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3 cursor-pointer" src="/search.svg" alt="" />
            <input
              @input="onChangeSearchInput"
              class="border border-gray-200 rounded-md py-2 pl-12 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>

      <CardList :items="itemsData" />
    </div>
  </div>
</template>

<style scoped>
</style>
