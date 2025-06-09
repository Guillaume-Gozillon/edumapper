<template>
  <div class="min-h-screen">
    <NuxtPage />
  </div>
</template>

<script setup lang="ts">
useHead({
  link: [
    {
      rel: "stylesheet",
      href: "https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap",
    },
  ],
});
</script>

<style global>
html {
  font-family: "DM Sans", sans-serif;
}

body {
  background-color: #f7f3f0;
}
</style>
