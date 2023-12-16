<script setup>
import CardList from '../components/Card/CardList.vue';
import axios from 'axios';
import { inject, reactive, watch, ref, onMounted } from 'vue';
import debounce from 'lodash.debounce';

const { addToCart, removeFromCart, cart } = inject('cart');

const itemsData = ref([]);

const filterd = reactive({
  sortBy: 'title',
  searchQuery: ''
});

const addToCartPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
  console.log(cart.value)
};

const onChangeSelect = (e) => {
  filterd.sortBy = e.target.value
}

const onChangeSearchInput = debounce((e) => {
  filterd.searchQuery = e.target.value
}, 400)

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }
      item.isFavorite = true

      const { data } = await axios.post('https://70d8ccb0e330bcf6.mokky.dev/favourites', obj)
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
      ...obj,
      favoriteId: null,
      isFavorite: false,
      isAdded: false
    }))
  } catch (err) {
    console.log(`Ошибка при получении данных ${err}`)
  }
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://70d8ccb0e330bcf6.mokky.dev/favourites')

    itemsData.value = itemsData.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
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
watch(cart, () => {
  itemsData.value =  itemsData.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
watch(filterd, fetchItems)

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []
  await fetchItems()
  await fetchFavorites()
  itemsData.value = itemsData.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Всi кросiвки</h2>
    <div class="flex justify-between gap-10">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>

      <div class="relative">
        <img class="absolute left-4 top-3 cursor-pointer" src="/search.svg" alt="" />
        <input @input="onChangeSearchInput"
          class="border border-gray-200 rounded-md py-2 pl-12 pr-4 outline-none focus:border-gray-400" type="text"
          placeholder="Поиск..." />
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList :items="itemsData" @add-to-favorite="addToFavorite" @add-to-cart="addToCartPlus" />
  </div>
</template>