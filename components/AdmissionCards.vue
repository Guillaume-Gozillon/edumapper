<template>
  <div class="space-y-6">
    <div
      v-for="(item, idx) in items"
      :key="idx"
      class="bg-white rounded-2xl shadow overflow-hidden"
    >
      <header class="bg-gray-50 px-4 pt-3 pb-5">
        <div class="flex items-center justify-between mb-2">
          <div class="flex items-center text-sm text-gray-600">
            <span class="font">{{ item.school }}</span>
            <span class="mx-2 text-gray-400 select-none">|</span>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="w-4 h-4 text-gray-400 mr-1"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                fill-rule="evenodd"
                d="M10 2a6 6 0 00-6 6c0 4 6 10 6 10s6-6 6-10a6 6 0 00-6-6zm0 8a2 2 0 110-4 2 2 0 010 4z"
                clip-rule="evenodd"
              />
            </svg>
            <span>{{ item.city }}</span>
          </div>
          <button
            v-if="onRemove"
            class="text-gray-400 hover:text-gray-600 transition text-xl leading-none"
            @click="onRemove(idx)"
            aria-label="Supprimer la carte"
          >
            &times;
          </button>
        </div>

        <h3 class="text-xl font-semibold text-gray-800">
          {{ item.program }}
        </h3>
      </header>

      <div class="px-5 pt-3 pb-4">
        <!-- progress bar -->
        <section>
          <div class="flex items-center justify-between font-medium mb-4">
            <span>Mes chances</span>
            <span
              :class="[
                'px-2 py-0.5 rounded-full text-xs   tracking-wide flex items-center gap-1',
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

          <div class="flex space-x-2 mb-4">
            <div
              v-for="n in 5"
              :key="n"
              class="flex-1 h-3 rounded-full"
              :class="
                n <= filledSegments(item.score)
                  ? barBg(item.score)
                  : 'bg-gray-200'
              "
            />
          </div>
        </section>

        <button
          class="flex items-center gap-1 text-sm font-medium text-gray-800 mb-4 select-none cursor-pointer"
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
                    n <= item.confidence
                      ? starColor(item.score)
                      : 'text-gray-300'
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
const toggle = (idx: number) =>
  openSet.value.has(idx) ? openSet.value.delete(idx) : openSet.value.add(idx);
const isOpen = (idx: number) => openSet.value.has(idx);

const label = (score: number) => {
  if (score >= 80) return "Très élevées";
  if (score >= 60) return "Élevées";
  if (score >= 40) return "Moyennes";
  return "Faibles";
};

const labelBg = (score: number) => {
  if (score >= 80) return "bg-emerald-100 text-emerald-400";
  if (score >= 60) return "bg-blue-100 text-blue-400";
  if (score >= 40) return "bg-fuchsia-100 text-fuchsia-400";
  return "bg-amber-100 text-amber-400";
};

const barBg = (score: number) => {
  if (score >= 80) return "bg-emerald-400";
  if (score >= 60) return "bg-blue-400";
  if (score >= 40) return "bg-fuchsia-400";
  return "bg-amber-400";
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
