<script setup>
import Card from './Card.vue'

const onClickAdd = () => {
    
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      imageURL="/sneakers/sneakers-1.jpg"
      sneakers-name="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="239"
      :isAdded="false"
      :isFavorite="false"
      :onClickAdd="onClickAdd"
    />
    <Card
      imageURL="/sneakers/sneakers-2.jpg"
      sneakers-name="Мужские Кроссовки Nike Air Max 270"
      :price="239"
      :isAdded="false"
      :isFavorite="false"
    />
    <Card
      imageURL="/sneakers/sneakers-3.jpg"
      sneakers-name="Мужские Кроссовки Nike Blazer Mid Suede"
      :price="124"
      :isAdded="false"
      :isFavorite="false"
    />
    <Card
      imageURL="/sneakers/sneakers-4.jpg"
      sneakers-name="Кроссовки Puma X Aka Boku Future Rider"
      :price="124"
      :isAdded="false"
      :isFavorite="false"
    />
    <Card
      imageURL="/sneakers/sneakers-5.jpg"
      sneakers-name="Мужские Кроссовки Under Armour Curry 8"
      :price="259"
      :isAdded="false"
      :isFavorite="false"
    />
    <Card
      imageURL="/sneakers/sneakers-6.jpg"
      sneakers-name="Мужские Кроссовки Nike Kyrie 7"
      :price="214"
      :isAdded="false"
      :isFavorite="false"
    />
  </div>
</template>