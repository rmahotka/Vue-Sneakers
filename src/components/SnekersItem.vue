<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavorite: Function
})
</script>
<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer px-7 transition hover:-translate-y-2 hover:shadow-xl flex flex-col justify-between"
  >
    <img
      v-if="onClickFavorite"
      @click="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like"
      class="absolute top-8 left-8"
    />
    <img :src="imageUrl" alt="image" />
    <div>
      <p class="text-black text-sm mt-2">{{ title }}</p>
      <div class="flex justify-between items-center mt-5">
        <div class="flex flex-col">
          <span class="uppercase text-xs text-slate-400">Цена:</span>
          <span class="font-bold">{{ price }} руб.</span>
        </div>
        <img
          v-if="onClickAdd"
          @click="onClickAdd"
          :src="!isAdded ? '/plus.svg' : '/checked.svg'"
          alt=""
        />
      </div>
    </div>
  </div>
</template>
