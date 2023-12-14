<script setup>
import DrawerHead from './DrawerHead.vue'
import CartListItem from './Cart/CartListItem.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-50"></div>
  <div class="bg-white w-3/12 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <CartListItem />

    <div class="flex flex-col gap-3 mb-8 mt-7">
      <div class="flex items-center gap-2">
        <span class="text-l">Итого: </span>
        <div class="flex-1 border-b border-dashed h-5"></div>
        <b class="text-xl">345$</b>
      </div>
      <div class="flex items-center gap-2">
        <span class="text-l">Налог 5%: </span>
        <div class="flex-1 border-b border-dashed h-5"></div>
        <b class="text-xl">345$</b>
      </div>
      <button disabled="" class="mt-4 bg-lime-500 w-full disabled:bg-slate-300 cursor-pointer transition rounded-xl py-4 text-white font-bold hover:bg-lime-600">Оформить заказ</button>
    </div>

   

  </div>
</template>