<script setup>
import { inject } from 'vue'
import CardItem from './CardItem.vue'

const { cart, removeToCart } = inject('cart')
</script>
<template>
  <div class="flex flex-col gap-2" v-auto-animate>
    <CardItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeToCart(item)"
    />
  </div>
</template>
