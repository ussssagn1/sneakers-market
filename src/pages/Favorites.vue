<script setup>
import axios from 'axios'
import { onMounted, ref } from 'vue'
import CardList from '../components/Card/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://70d8ccb0e330bcf6.mokky.dev/favourites?_relations=items'
    )
    favorites.value = data.map(obj => obj.item)
  } catch (e) {
    console.log(e)
  }
})
</script>


<template>
    <h1 class="text-3xl font-bold mb-8">Мои закладки</h1>
    <CardList :items="favorites" is-favorites/>
</template>