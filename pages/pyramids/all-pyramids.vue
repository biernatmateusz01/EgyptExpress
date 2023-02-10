<template>
  <div class="">
    <div class="w-full h-8  flex justify-between"></div>
    <div
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-9 px-4 xl:px-0 pb-4 xl:pb-32"
    >
      <TheItem v-for="item in pyramidhs" :key="item.id" :item="item" />
    </div>
  </div>
</template>

<script setup>
defineProps({
  pyramidhs: Array,
});

useHead({
  title: "Egypt Express",
});
</script>
