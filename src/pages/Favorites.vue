<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import SnekersItemList from '@/components/SnekersItemList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://185ccf303bf06ebd.mokky.dev/favorites?_relations=items`
    )

    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-10">Мои закладки</h2>

  <div v-if="favorites.length == 0" class="flex flex-col items-center text-center w-72 mx-auto">
    <img height="70" width="70" src="/emoji-1.png" alt="Info Image" />
    <h2 class="mt-4 text-2xl font-medium">Закладок нет :(</h2>
    <p class="mt-2 text-gray-400">Вы ничего не добавляли в закладки</p>
  </div>

  <SnekersItemList v-else :items="favorites" is-favorites />
</template>
