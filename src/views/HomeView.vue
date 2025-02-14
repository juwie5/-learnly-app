<template>
  <main class="flex flex-col items-center justify-center p-16">
    <h1 class="text-3xl font-bold text-[#5122A2]">Learnly App</h1>
    <div class="mt-20 flex gap-4 ">
      <router-link 
      to="/course-review" 
      class="bg-[#5122A2] py-2 px-4 rounded-lg text-white">
        Course Review
      </router-link>
      <router-link 
      to="/take-test" 
      class="bg-[#5122A2] py-2 px-4 rounded-lg text-white">
        Take Test
      </router-link>
    </div>
  </main>
</template>

<script setup>

</script>
