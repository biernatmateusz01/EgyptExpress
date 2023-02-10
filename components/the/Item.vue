<template>
  <div
    class="h-80 relative flex flex-col rounded-md shadow-lg overflow-hidden group"
  >
    <div
      class="h-1/2 bg-yellow overflow-hidden group-hover:scale-110 transition duration-150 ease-in-out"
    >
      <img :src="item.imageUrl" alt="" class="object-cover" />
    </div>
    <div class="h-1/2 flex flex-col gap-8 p-8">
      <div class="w-full font-semibold text-lg truncate">{{ item.name }}</div>
      <div class="w-full flex gap-10">
        <div class="flex flex-col">
          <span class="text-gray-500 uppercase text-xs">height</span>
          <span class="font-semibold">{{ item.height }} m </span>
        </div>
        <div class="flex flex-col">
          <span class="text-gray-500 uppercase text-xs">tour price</span>
          <span class="font-semibold">{{ item.calculatePrice }} $</span>
        </div>
      </div>
    </div>
    <div
      v-if="item.price === 0"
      class="bg-white bg-opacity-80 h-full w-full absolute top-0 left-0"
    ></div>
  </div>
</template>

<script setup>
defineProps({
  item: Object,
});
</script>
