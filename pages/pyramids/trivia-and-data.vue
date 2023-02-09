<template>
  <div class="py-6 xl:py-28">
    <div class="flex flex-col gap-2">
      <div class="flex justify-center p-2 border-t border-b border-emerald-500">
        <span>Mean:</span>
        <span>{{ mean }}</span>
      </div>
      <div class="flex justify-center p-2 border-t border-b border-emerald-500">
        <span>averageVolume:</span>
        <span>{{ averageVolume }}</span>
      </div>
    </div>
    <!-- <div class="w-full h-8 mb-4 xl:mb-12 flex justify-between"></div> -->
    <!-- <div
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-9 px-4 xl:px-0 pb-4 xl:pb-32"
    >

    </div> -->
  </div>
</template>

<script setup>
defineProps({
  mean: Number,
  averageVolume: Number,
});
useHead({
  title: "Egypt Express",
});
</script>
