<script setup>
import DrawerHead from './DrawerHead.vue'
import CartListItem from './../Cart/CartListItem.vue'
import infoBlock from './infoBlock.vue'
import axios from 'axios'
import { ref, computed, inject } from 'vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})
const { cart } = inject('cart')
const isCreating = ref(false)
const orderId = ref(null)
const createOrder = async () => {
  // создание заказа
  try {
    isCreating.value = true
    const { data } = await axios.post('https://70d8ccb0e330bcf6.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })
    cart.value = []
    orderId.value = data.id;
    
  } catch (e) {
    console.log(`Error: ${e}`)
  } finally {
    isCreating.value = false
  }
}
const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-50"></div>
  <div class="bg-white w-3/12 h-full fixed right-0 top-0 z-20 p-8">

    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <infoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        desc="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <infoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :desc="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>
    <div v-else class="">
      <CartListItem />
      <div class="flex flex-col gap-3 mb-8 mt-7">
        <div class="flex items-center gap-2">
          <span class="text-l">Итого: </span>
          <div class="flex-1 border-b border-dashed h-5"></div>
          <b class="text-xl">{{ totalPrice }}$</b>
        </div>
        <div class="flex items-center gap-2">
          <span class="text-l">Налог 5%: </span>
          <div class="flex-1 border-b border-dashed h-5"></div>
          <b class="text-xl">{{ vatPrice }}$</b>
        </div>
        <button
          :disabled="cartButtonDisabled"
          @click="createOrder"
          class="mt-4 bg-lime-500 w-full disabled:bg-slate-300 cursor-pointer transition rounded-xl py-4 text-white font-bold hover:bg-lime-600"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>