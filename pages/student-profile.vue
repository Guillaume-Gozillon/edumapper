<template>
  <div>
    <header class="text-black px-6 py-3 flex items-center">
      <h1 class="text-2xl font-serif">Edumapper</h1>
    </header>
    <div class="w-full h-1 bg-gray-200">
      <div
        class="bg-black h-full transition-all duration-300"
        :style="{ width: progressPercentage + '%' }"
      ></div>
    </div>

    <div class="max-w-3xl mx-auto px-4 py-8 space-y-6">
      <div class="w-full">
        <SchoolHeader
          name="Lycée Gaston Berger"
          city="Lille"
          type="Lycée Public"
        />
      </div>

      <SectionCard title="En quelle classe es-tu ?" @opened="markStep(1)">
        <div class="flex space-x-2 mb-4">
          <button
            v-for="c in classes"
            :key="c"
            @click="selectedClass = c"
            :class="pillClass(selectedClass === c)"
          >
            {{ c }}
          </button>
        </div>

        <p class="font-medium mb-2">Type de bac</p>
        <div class="flex space-x-2 mb-4">
          <button
            v-for="t in bacTypes"
            :key="t"
            @click="selectedBacType = t"
            :class="pillClass(selectedBacType === t)"
          >
            {{ t }}
          </button>
        </div>

        <button
          class="mt-4 w-full px-4 py-2 border rounded-full font-medium disabled:opacity-50 disabled:cursor-not-allowed bg-white hover:bg-gray-50"
          :disabled="!selectedClass || !selectedBacType"
          @click="confirm"
        >
          Confirmer
        </button>
      </SectionCard>

      <SectionCard title="Spécialités" @opened="markStep(2)">
        <UInput
          v-model="specialites"
          placeholder="À compléter"
          class="w-full"
        />
      </SectionCard>

      <SectionCard title="Notes" @opened="markStep(3)">
        <div v-for="(note, i) in notes" :key="i" class="flex space-x-2 mb-2">
          <UInput v-model="note.subject" placeholder="Matière" class="flex-1" />
          <UInput
            v-model="note.mark"
            placeholder="Moyenne"
            type="number"
            :min="0"
            :max="20"
            class="w-24"
          />
        </div>
        <button
          class="text-blue-600 hover:underline text-sm"
          @click="notes.push({ subject: '', mark: '' })"
        >
          + Ajouter une matière
        </button>
      </SectionCard>

      <SectionCard title="Résultats au bac" @opened="markStep(4)">
        <div class="space-y-3">
          <UInput v-model="bacResult.mention" label="Mention (si connue)" />
          <UInput
            v-model="bacResult.average"
            label="Moyenne générale"
            type="number"
            :min="0"
            :max="20"
          />
        </div>
      </SectionCard>

      <div class="w-full">
        <router-link
          to="/admission-chances"
          class="mt-4 w-full px-4 py-2 border rounded-full font-medium bg-white hover:bg-black hover:text-white inline-block text-center"
        >
          Voir les résultats
        </router-link>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import SchoolHeader from "~/components/SchoolHeader.vue";
import SectionCard from "~/components/SectionCard.vue";
import UInput from "~/components/UInput.vue";

const totalSteps = 5;
const currentStep = ref(0);

function markStep(step: number) {
  if (step > currentStep.value) currentStep.value = step;
}

const progressPercentage = computed(
  () => (currentStep.value / totalSteps) * 100,
);

const classes = ["Seconde", "Première", "Terminale"];
const bacTypes = ["Général", "Technologique", "Professionnel"];

const selectedClass = ref<string>("");
const selectedBacType = ref<string>("");
const specialites = ref<string>("");
const notes = ref<{ subject: string; mark: string | number }[]>([
  { subject: "", mark: "" },
]);
const bacResult = ref<{ mention: string; average: string | number }>({
  mention: "",
  average: "",
});

function confirm() {
  console.log(
    "Classe",
    selectedClass.value,
    "Bac",
    selectedBacType.value,
    "Spécialités",
    specialites.value,
    "Notes",
    notes.value,
    "Résultat bac",
    bacResult.value,
  );
}

function pillClass(selected: boolean) {
  return [
    "px-4 py-1 rounded-full cursor-pointer select-none",
    selected
      ? "bg-black text-white"
      : "bg-gray-100 text-gray-800 hover:bg-gray-200",
  ];
}
</script>
