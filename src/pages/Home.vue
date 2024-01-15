<script setup>
import { ref, reactive, inject, watch, onMounted } from 'vue'
import Axios from 'axios'
import debonce from 'lodash.debounce'
import SnekersItemList from '../components/SnekersItemList.vue'

const { cart, addToCart, removeToCart } = inject('cart')

const items = ref([])

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeToCart(item)
  }
}

const filter = reactive({
  sortBy: 'title',
  serchQuery: ''
})

const onChangeSelect = (event) => {
  filter.sortBy = event.target.value
}

const onChangeSearchInput = debonce((event) => {
  filter.serchQuery = event.target.value
}, 350)

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }

      item.isFavorite = true
      const { data } = await Axios.post(`https://185ccf303bf06ebd.mokky.dev/favorites`, obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await Axios.delete(`https://185ccf303bf06ebd.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchItem = async () => {
  try {
    const params = {
      sortBy: filter.sortBy
    }

    if (filter.serchQuery) {
      params.title = `*${filter.serchQuery}*`
    }

    const { data } = await Axios.get(`https://185ccf303bf06ebd.mokky.dev/items`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorite = async () => {
  try {
    const { data: favorites } = await Axios.get(`https://185ccf303bf06ebd.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItem()
  await fetchFavorite()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filter, fetchItem)
</script>
<template>
  <div class="flex items-center justify-between mb-10">
    <h2 class="text-3xl font-bold">Все кросовки</h2>
    <div class="flex items-center gap-4">
      <select
        @change="onChangeSelect"
        name=""
        id=""
        class="border rounded-md py-2 px-3 outline-non"
      >
        <option value="name">По названию</option>
        <option value="price">По цене(дешевые)</option>
        <option value="-price">По цене(дорогие)</option>
      </select>
      <div class="relative">
        <img src="/search.svg" alt="search" class="absolute left-3 top-3" />
        <input
          @input="onChangeSearchInput"
          type="text"
          placeholder="Поиск..."
          id="search"
          class="border rounded-md pl-10 pr-4 py-2 outline-none focus:border-gray-400"
        />
      </div>
    </div>
  </div>
  <SnekersItemList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
</template>
