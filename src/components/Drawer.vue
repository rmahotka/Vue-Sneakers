<script setup>
import { ref, computed, inject } from 'vue'
import axios from 'axios'

import DrawerHead from './DrawerHead.vue'
import CardItemList from './CardItemList.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const { cart } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post(`https://185ccf303bf06ebd.mokky.dev/orders`, {
      item: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)

const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>
<template>
  <div class="fixed top-0 left-0 bg-black opacity-60 min-h-full w-full z-10"></div>
  <div class="fixed right-0 top-0 bg-white w-96 h-full z-20 p-8 overflow-auto">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex items-center h-4/5">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />

      <InfoBlock
        v-if="orderId"
        title="Мои закладки"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>
    <div v-else>
      <CardItemList />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <span class="font-bold">{{ totalPrice }} руб.</span>
        </div>
        <div class="flex gap-2 items-end">
          <span>Налог НДС 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <span class="font-bold">{{ vatPrice }} руб.</span>
        </div>
        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="bg-lime-500 mt-4 w-full rounded-xl py-3 text-white disabled:bg-gray-300 hover:bg-lime-600 active:bg-lime-700 transition"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
