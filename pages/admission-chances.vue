
<template>
  <div class="p-8">
    <h1 class="text-xl font-bold mb-4">Admission Chances</h1>
    <button class="btn" @click="goToProfile">Back to Profile</button>
  </div>
</template>

<script setup lang="ts">
const router = useRouter()
const goToProfile = () => {
  router.push('/student-profile')
}
</script>

<style scoped>
.btn {
  @apply px-4 py-2 bg-black text-white rounded hover:bg-gray-800;
}
</style>