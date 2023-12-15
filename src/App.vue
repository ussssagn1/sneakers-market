<script setup>
// COMPOSITION API
import { onMounted, reactive, ref, watch } from 'vue'
import Drawer from './components/Drawer.vue'
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
    itemsData.value = data.map((obj) => ({
      //Когда мы получили данные, перед записью в itemsData, мы внутрь data, будем по умолчанию добавлять isFavorite, isAdded
      ...obj, // всё что находиться в object (id, title, price...)
      favoriteId: null, // ID для закладки, в закладках есть объект который имеет свой id (нужжно для удаления)
      isFavorite: false, // внутрь доваляем isFavourite
      isAdded: false // внутрь добавлем isAdded
    }))
  } catch (err) {
    console.log(`Ошибка при получении данных ${err}`)
  }
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://70d8ccb0e330bcf6.mokky.dev/favourites')

    itemsData.value = itemsData.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id) //из данных (favorites), найди такой объект, который равен в БД favorites ;(

      if (!favorite) {
        // если нет такого, вернём элемент
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(`Ошибка при получении данных ${err}`)
  }
}
const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      } // создаём объект с ID как у item
      item.isFavorite = true // вешаем на него true
      const { data } = await axios.post('https://70d8ccb0e330bcf6.mokky.dev/favourites', obj) // (ПОЛУЧЕНИЕ ДАННЫХ - GET, ОТПРАВКА - POST) - отправляем obj на сервер      
      item.favoriteId = data.id
      console.log(data)
    } else {
      item.isFavorite = false     
      await axios.delete(`https://70d8ccb0e330bcf6.mokky.dev/favourites/${item.favoriteId}`)     
      item.favoriteId = null
      
    }
  } catch (e) {
    console.log(`Error ${e}`)
  }
}

onMounted(async () => {
  // ПРИ ПЕРВОМ РЕНДЕРЕ ДЕЛАТЬ ЧТО-ТО === onMOUNTED
  await fetchItems()
  await fetchFavorites()
})
watch(filterd, fetchItems) // ПРИ ОТСЛЕЖИВАНИИ ЧЕГО ЛИБО
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

      <CardList :items="itemsData" @addToFavorite="addToFavorite" />
    </div>
  </div>
</template>

<style scoped>
</style>
