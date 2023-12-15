<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
})
const emit = defineEmits([
  'addToFavorite'
])

const onClickAdd = () => {
  console.log('111')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageURL="item.imageUrl"
      :sneakers-name="item.title"
      :price="item.price"
      :isAdded="false"
      :isFavorite="item.isFavorite"
      :onClickAdd="onClickAdd"
      :onClickFavorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>