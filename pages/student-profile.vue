<template>
  <div class="max-w-2xl mx-auto px-4 py-8">
    <SectionCard title="En quelle classe es-tu ?">
      <div class="space-x-2">
        <UButton
          v-for="c in classes"
          :key="c"
          :label="c"
          :variant="selectedClass === c ? 'solid' : 'ghost'"
          @click.stop="selectedClass = c"
        />
      </div>
      <hr class="my-4" />
      <p class="font-medium mb-2">Type de bac</p>
      <div class="space-x-2">
        <UButton
          v-for="t in bacTypes"
          :key="t"
          :label="t"
          :variant="selectedBacType === t ? 'solid' : 'ghost'"
          @click.stop="selectedBacType = t"
        />
      </div>

      <UButton
        class="mt-6 w-full"
        variant="outline"
        :disabled="!selectedClass || !selectedBacType"
        @click.stop="confirm"
      >
        Confirmer
      </UButton>
    </SectionCard>

    <SectionCard title="Spécialités">
      <UTextarea
        placeholder="Ex. : Maths, Physique-Chimie, NSI…"
        v-model="specialites"
      />
    </SectionCard>

    <SectionCard title="Notes">
      <p class="text-sm text-gray-600 mb-3">
        Saisis tes moyennes par matière :
      </p>
      <div v-for="(note, i) in notes" :key="i" class="flex space-x-2 mb-2">
        <UInput placeholder="Matière" v-model="note.subject" class="flex-1" />
        <UInput
          placeholder="Moyenne"
          v-model="note.mark"
          type="number"
          min="0"
          max="20"
          class="w-24"
        />
      </div>
      <UButton
        variant="ghost"
        @click.stop="notes.push({ subject: '', mark: '' })"
      >
        + Ajouter une matière
      </UButton>
    </SectionCard>

    <SectionCard title="Résultats au bac">
      <div class="space-y-3">
        <UInput label="Mention (si connue)" v-model="bacResult.mention" />
        <UInput
          label="Moyenne générale"
          type="number"
          v-model="bacResult.average"
          min="0"
          max="20"
        />
      </div>
    </SectionCard>
  </div>
</template>

<script setup lang="ts">
import SectionCard from "~/components/SectionCard.vue";
import { ref } from "vue";

const classes = ["Seconde", "Première", "Terminale"];
const bacTypes = ["Général", "Technologique", "Professionnel"];
const selectedClass = ref("");
const selectedBacType = ref("");
function confirm() {
  console.log("Classe", selectedClass.value, "Bac", selectedBacType.value);
}

const specialites = ref("");

const notes = ref<{ subject: string; mark: string | number }[]>([
  { subject: "", mark: "" },
]);

const bacResult = ref<{ mention: string; average: string | number }>({
  mention: "",
  average: "",
});
</script>
