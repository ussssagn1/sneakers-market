<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})
const emit = defineEmits(['clickOnRemove'])
</script>
<template>
  <div class="flex justify-between items-center border border-slate-100 p-4 rounded-xl gap-5">
    <img class="w-24 h-24" :src="imageUrl" :alt="title" />
    <div class="flex flex-col flex-1">
        <p>{{title}}</p>
        <div class="flex justify-between">
            <b class="flex-1">{{ price }}$</b>
            <img @click="emit('clickOnRemove')" class="opacity-40 hover:opacity-100 cursor-pointer transition" src="/close.svg" alt="close">
        </div>
    </div>
  </div>
</template>