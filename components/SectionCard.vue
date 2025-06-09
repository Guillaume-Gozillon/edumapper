<template>
  <div
    class="rounded-2xl border border-gray-200 p-6 my-4"
    :class="
      open
        ? 'bg-white shadow-lg'
        : 'bg-gray-50 hover:bg-gray-100 cursor-pointer'
    "
    @click="open || toggle()"
  >
    <div class="flex items-center justify-between">
      <h2 class="text-lg font-semibold">
        {{ title }}
      </h2>

      <svg
        v-if="!open"
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5 text-gray-500"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5M18.5 2.5a2.121 2.121 0 013 3L13 14l-4 1 1-4 8.5-8.5z"
        />
      </svg>

      <button
        v-if="open"
        @click.stop="toggle()"
        class="text-gray-500 hover:text-gray-700"
        aria-label="Fermer"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>
    </div>

    <transition name="fade">
      <div v-if="open" class="mt-6">
        <slot />
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const props = defineProps<{
  title: string;
}>();

const open = ref(false);
function toggle() {
  open.value = !open.value;
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.25s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}
</style>
