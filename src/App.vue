<script setup>
// COMPOSITION API
import { computed, provide, ref, watch } from 'vue'
import Drawer from './components/Drawer/Drawer.vue'
import Header from './components/Header.vue'


// В Componition API ref(i) -> i.value
// В Options API data() { i: 1 } -> this.i


// Корзина (START)
const cart = ref([]) // список товаров которые находяться в корзине


const drawerOpen = ref(false)
const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}
const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}
watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

// Корзниа (END)



provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})
</script>

<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
  />

  <div class="w-4/5 bg-gray-50 mx-auto rounded-xl shadow-2xl mt-14 pb-5">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <div class="p-10">
      <router-view>

      </router-view>
    </div>
  </div>
</template>

<style scoped>
</style>
