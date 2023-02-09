<template>
  <div>
    <nav
      class="flex w-full gap-4 overflow-x-auto lg:space-x-0 px-4"
      aria-label="Tabs"
    >
      <NuxtLink
        v-for="tab in tabs"
        :key="tab.name"
        :to="{ name: tab.href, params: tab.params }"
      >
        <div>
          <span>{{ tab.name }}</span>
        </div>
      </NuxtLink>
    </nav>
  </div>
</template>

<script setup>
defineProps({
  tabs: Array,
});
</script>

<style scoped>
.router-link-exact-active {
  border-bottom: 1px solid gray;
}
</style>
