<script setup>

defineProps({
  id: Number,
  imageURL: String,
  sneakersName: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="relative border border-slate-100 rounded-3xl bg-white p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      @click="onClickFavorite"
      :src="isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like-1"
      class="absolute top-8 left-8"
    />
    <img :src="imageURL" alt="Sneaker" />
    <p class="mt-2 text-xl">{{ sneakersName }}</p>

    <div class="flex justify-between mt-5 items-center">
      <div class="flex flex-col">
        <span class="text-slate-400 uppercase text-l">Цена:</span>
        <b class="text-xl">{{ price }}$</b>
      </div>
      <img @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="plus" />
    </div>
  </div>
</template>