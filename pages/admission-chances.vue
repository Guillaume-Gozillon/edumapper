<script setup lang="ts">
import { useRouter } from "vue-router";
import AdmissionCards from "@/components/AdmissionCards.vue";

const router = useRouter();
const goToProfile = () => {
  router.push("/student-profile");
};

const formations = [
  {
    school: "SKEMA",
    city: "Lille",
    program: "BBA – Global Management",
    score: 92,
    details:
      "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
    confidence: 5,
  },
  {
    school: "EDHEC Business School",
    city: "Paris",
    program: "International BBA – Business Management",
    score: 71,
    details:
      "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
    confidence: 4,
  },
  {
    school: "IPAG Business School",
    city: "Grenoble",
    program: "International BBA – Business Management",
    score: 33,
    details:
      "Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.",
    confidence: 2,
  },
  {
    school: "ICN Business School",
    city: "Puteaux",
    program: "IBBA – Manager international",
    score: 55,
    details:
      "Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    confidence: 3,
  },
];
</script>

<template>
  <div class="max-w-3xl mx-auto px-4 py-8 space-y-6">
    <h2 class="text-3xl font">Tes chances d’admission aux formations</h2>

    <p class="text-gray-700 leading-relaxed">
      Cette estimation provient de calculs rigoureux fondés sur les
      caractéristiques de ton profil et les statistiques passées de cette
      formation. Elle est fournie à titre indicatif et ne garantit en aucun cas
      la décision d’admission de l’établissement.
    </p>

    <button class="btn flex items-center cursor-pointer" @click="goToProfile">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5 mr-2"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <path d="M11 5H6a2 2 0 00-2 2v12a2 2 0 002 2h12a2 2 0 002-2v-5" />
        <path
          d="M16.586 6.586a2 2 0 112.828 2.828L11 18l-4 1 1-4 8.586-8.586z"
        />
      </svg>
      Modifier mon profil
    </button>

    <AdmissionCards :items="formations" />
  </div>
</template>

<style scoped>
@reference "tailwindcss";

.btn {
  @apply px-4 py-2 border border-gray-400 text-black rounded-full
  hover:bg-black hover:text-white transition-colors duration-200;
}
</style>
