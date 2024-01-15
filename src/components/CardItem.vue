<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})
</script>
<template>
  <div class="border border-slate-200 rounded-xl flex items-center p-3 gap-3">
    <img :src="imageUrl" :alt="title" class="w-16 h-16" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex items-end justify-between">
        <span class="flex-1 font-bold">{{ price }} руб.</span>
        <img
          @click="emit('onClickRemove')"
          src="/close.svg"
          alt="close"
          class="opacity-40 cursor-pointer hover:opacity-100 transition"
        />
      </div>
    </div>
  </div>
</template>
