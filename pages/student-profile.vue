
<template>
  <div class="p-8">
    <h1 class="text-xl font-bold mb-4">Student Profile</h1>
    <button class="btn" @click="goToAdmission">Go to Admission Chances</button>
  </div>
</template>

<script setup lang="ts">
const router = useRouter()
const goToAdmission = () => {
  router.push('/admission-chances')
}
</script>

<style scoped>
.btn {
  @apply px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600;
}
</style>