<template>
  <div class="space-y-6">
    <div
      v-for="(item, idx) in items"
      :key="idx"
      class="bg-white rounded-xl shadow p-5"
    >
      <header
        class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2 mb-4"
      >
        <p class="text-sm text-gray-500">
          {{ item.school }} <span class="mx-1">•</span> {{ item.city }}
        </p>
        <button
          v-if="onRemove"
          class="text-gray-400 hover:text-gray-600 transition"
          @click="onRemove(idx)"
          aria-label="Supprimer la carte"
        >
          &times;
        </button>
      </header>

      <h3 class="text-xl font-semibold mb-6">
        {{ item.program }}
      </h3>

      <!-- progress bar -->
      <section>
        <div class="flex items-center justify-between text-sm font-medium mb-2">
          <span>Mes chances</span>
          <span
            :class="[
              'px-2 py-0.5 rounded-full text-xs uppercase tracking-wide flex items-center gap-1',
              labelBg(item.score),
            ]"
          >
            {{ label(item.score) }}
            <span v-if="label(item.score) === 'Très élevées'">⚡️</span>
            <span v-else-if="label(item.score) === 'Élevées'">👍</span>
            <span v-else-if="label(item.score) === 'Faibles'">😬</span>
            <span v-else>😕</span>
          </span>
        </div>

        <div class="flex space-x-1 mb-4">
          <div
            v-for="n in 5"
            :key="n"
            class="flex-1 h-1.5 rounded-full"
            :class="
              n <= filledSegments(item.score)
                ? barBg(item.score)
                : 'bg-gray-200'
            "
          />
        </div>
      </section>

      <button
        class="flex items-center gap-1 text-sm font-medium text-gray-800 mb-4 select-none"
        @click="toggle(idx)"
      >
        {{ isOpen(idx) ? "Voir moins" : "Voir plus" }}
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          class="w-4 h-4 transition-transform"
          :class="isOpen(idx) ? 'rotate-180' : ''"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M12 15a1 1 0 0 1-.707-.293l-5-5a1 1 0 0 1 1.414-1.414L12 12.586l4.293-4.293a1 1 0 0 1 1.414 1.414l-5 5A1 1 0 0 1 12 15Z"
            clip-rule="evenodd"
          />
        </svg>
      </button>

      <transition name="fade">
        <div v-if="isOpen(idx)" class="space-y-4">
          <p v-if="item.details" class="text-gray-600 leading-relaxed">
            {{ item.details }}
          </p>

          <div v-if="item.confidence" class="font-medium text-sm">
            Indice de confiance
            <span class="inline-flex items-center ml-2">
              <svg
                v-for="n in 5"
                :key="n"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
                class="w-4 h-4"
                :class="
                  n <= item.confidence ? starColor(item.score) : 'text-gray-300'
                "
              >
                <path
                  d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 0 0 .95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.804 2.04a1 1 0 0 0-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.538 1.118l-2.804-2.04a1 1 0 0 0-1.176 0l-2.804 2.04c-.783.57-1.838-.197-1.538-1.118l1.07-3.292a1 1 0 0 0-.364-1.118L2.4 8.72c-.783-.57-.38-1.81.588-1.81h3.463a1 1 0 0 0 .95-.69l1.048-3.293Z"
                />
              </svg>
            </span>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, ref } from "vue";

interface FormationItem {
  school: string;
  city: string;
  program: string;
  score: number;
  details?: string;
  confidence?: number;
}

const props = defineProps<{
  items: FormationItem[];
  onRemove?: (idx: number) => void;
}>();

const openSet = ref<Set<number>>(new Set());

const toggle = (idx: number) => {
  openSet.value.has(idx) ? openSet.value.delete(idx) : openSet.value.add(idx);
};
const isOpen = (idx: number) => openSet.value.has(idx);

const label = (score: number) => {
  if (score >= 80) return "Très élevées";
  if (score >= 60) return "Élevées";
  if (score >= 40) return "Moyennes";
  return "Faibles";
};

const labelBg = (score: number) => {
  if (score >= 80) return "bg-emerald-100 text-emerald-700";
  if (score >= 60) return "bg-blue-100 text-blue-700";
  if (score >= 40) return "bg-fuchsia-100 text-fuchsia-700";
  return "bg-amber-100 text-amber-700";
};

const barBg = (score: number) => {
  if (score >= 80) return "bg-emerald-500";
  if (score >= 60) return "bg-blue-500";
  if (score >= 40) return "bg-fuchsia-500";
  return "bg-amber-500";
};

const starColor = (score: number) => {
  if (score >= 80) return "text-emerald-500";
  if (score >= 60) return "text-blue-500";
  if (score >= 40) return "text-fuchsia-500";
  return "text-amber-500";
};

const filledSegments = (score: number) => Math.round(score / 20);
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.2s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}
</style>
