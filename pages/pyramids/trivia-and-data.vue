<template>
  <div class="py-6 xl:py-28">
    <div class="flex flex-col gap-2">
      <div class="w-full flex gap-4">
        <div
          class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-9 px-4 xl:px-0 pb-4 xl:pb-8"
        >
          <div class="flex flex-col">
            <span class="mb-4">Pyramid with biggest rectangle</span>
            <TheItem
              v-for="item in elementWithBiggestRectangleArea"
              :key="item.id"
              :item="item"
            />
          </div>
          <div class="flex flex-col">
            <span class="mb-4"> Pyramid with average volume</span>
            <TheItem
              v-for="item in elementWithAverageVolume"
              :key="item.id"
              :item="item"
            />
          </div>
        </div>
      </div>
      <div
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-9 px-4 xl:px-0 pb-4 xl:pb-32"
      >
        <div class="shadow-lg flex flex-col gap-4 p-4 rounded-md">
          <span>Average cost of visiting</span>
          <span>{{ mean.toFixed(2) }} $</span>
        </div>
        <div class="shadow-lg flex flex-col gap-4 p-4 rounded-md">
          <span>Average volume of the pyramids</span>
          <span>{{ averageVolume.toFixed(0) }} m</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  mean: Number,
  averageVolume: Number,
  elementWithBiggestRectangleArea: Array,
  elementWithAverageVolume: Array,
});
useHead({
  title: "Egypt Express",
});
</script>
