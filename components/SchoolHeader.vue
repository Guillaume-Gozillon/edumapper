<template>
  <div class="w-full overflow-hidden rounded-2xl">
    <div
      class="bg-gradient-to-r from-orange-400 to-pink-300 p-5 flex items-center justify-between text-white"
    >
      <div>
        <h2 class="text-xl font-bold mb-1">{{ name }}</h2>
        <div class="flex items-center gap-2 text-sm">
          <span>📍 {{ city }}</span>
          <span>🏫 {{ type }}</span>
        </div>
      </div>

      <button
        class="bg-white text-black rounded-full px-4 py-1 text-sm hover:bg-gray-100 transition"
      >
        Modifier
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  name: string;
  city: string;
  type: string;
}>();
</script>
