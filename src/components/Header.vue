<script setup>
const emit = defineEmits(['openDrawer'])

defineProps({
  totalPrice: Number
})
</script>
<template>
  <header class="flex items-center justify-between border-b border-slate-200 px-11 py-8">
    <router-link to="/">
      <div class="flex items-center gap-x-4">
        <img src="/logo.png" alt="Logo" class="w-10 h-10" />
        <div>
          <h2 class="text-xl leading-none font-bold uppercase">Vue Sneakers</h2>
          <p class="text-sm leading-none font-normal text-slate-500">Магазин лучших кросовок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-x-8">
      <il @click="() => emit('openDrawer')" class="flex items-center gap-3 cursor-pointer">
        <img src="/cart.svg" alt="cart" />
        <p class="text-gray-500 font-bold hover:text-black duration-300">{{ totalPrice }} руб.</p>
      </il>

      <router-link to="/favorites">
        <il class="flex items-center gap-3 cursor-pointer">
          <img src="/heart.svg" alt="heart" />
          <p class="text-gray-500 hover:text-black duration-300">Закладки</p>
        </il>
      </router-link>

      <il class="flex items-center gap-3 cursor-pointer">
        <img src="/profile.svg" alt="profile" />
        <p class="text-gray-500 hover:text-black duration-300">Профиль</p>
      </il>
    </ul>
  </header>
</template>
