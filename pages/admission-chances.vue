<template>
  <div class="max-w-3xl mx-auto px-4 py-8 space-y-6">
    <h2 class="text-3xl font-semibold">
      Tes chances d’admission aux formations
    </h2>

    <p class="text-gray-700 leading-relaxed">
      Cette estimation provient de calculs rigoureux fondés sur les
      caractéristiques de ton profil et les statistiques passées de cette
      formation. Elle est fournie à titre indicatif et ne garantit en aucun cas
      la décision d’admission de l’établissement.
    </p>

    <button class="btn flex items-center" @click="goToProfile">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5 mr-2"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M11 5H6a2 2 0 00-2 2v12a2 2 0 002 2h12a2 2 0 002-2v-5m-2.414-9.414a2 2 0 112.828 2.828L11 16l-4 1 1-4 8.586-8.586z"
        />
      </svg>
      Modifier mon profil
    </button>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from "vue-router";

const router = useRouter();
const goToProfile = () => {
  router.push("/student-profile");
};
</script>

<style scoped>
@reference "tailwindcss";

.btn {
  @apply px-4 py-2 border-1   border-gray-400 text-black rounded-full
  hover:bg-black hover:text-white transition-colors duration-200;
}
</style>
