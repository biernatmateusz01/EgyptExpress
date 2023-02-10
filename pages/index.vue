<template>
  <main>
    <span></span>
  </main>
</template>

<script setup>
definePageMeta({
  middleware: (to) => {
    if (to.name === "index") return navigateTo({ name: "pyramids" });
  },
});
</script>
