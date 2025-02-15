<template>
  <main class="p-8">
    <div class="w-full flex justify-between items-center gap-8">
      <router-link to="/">
        <img src="../assets/back.svg" />
      </router-link>
      <p>Course Preview</p>
      <img src="../assets/question.svg" alt="">
    </div>
    <div class="w-full flex justify-between items-center gap-8 my-2.5">
      <div>
        <h3>Lesson 1.3</h3>
        <div class="flex gap-1">
          <div class="bg-[#5122A2] w-5 h-2 rounded-xl"></div>
          <div class="bg-[#5122A2] w-5 h-2 rounded-xl"></div>
          <div class="bg-[#5122A2] w-5 h-2 rounded-xl"></div>
          <div class="bg-[#CDCDCD] w-5 h-2 rounded-xl"></div>
          <div class="bg-[#CDCDCD] w-5 h-2 rounded-xl"></div>
        </div>
      </div>
      <div class="bg-purple-100 rounded-full flex items-center gap-2 px-2 py-1">
        <img src="../assets/time.svg" alt="">
        <p>00:12</p>
      </div>
    </div>
    <div class="my-2">
      <h4 class="text-center font-bold text-xl">Match the Algebraic Terms!</h4>
      <!-- Meaning Slots -->
      <div class="grid grid-cols-3 justify-items-center gap-4 my-3">
        <div v-for="(item, index) in shuffledWords" :key="index"
          class="border-2 border-dotted rounded-lg p-2.5 w-full lg:w-80 lg:h-30 flex flex-col items-center justify-center text-center"
          :class="matchedPairs[item.word] ? 'bg-green-200 border-green-400' : 'bg-[#ECE7F5] border-purple-400'"
          @dragover.prevent @drop="handleDrop($event, item.word)">
          <p v-if="matchedPairs[item.word] === 'correct'">
            {{ item.word }} ✅
          </p>
          <p v-else-if="matchedPairs[item.word] === 'wrong'">
            {{ item.word }} ❌
          </p>
          <p v-else class="text-stone-500">
            {{ item.meaning }}
          </p>
        </div>
      </div>

      <!-- Draggable Words -->
      <div class="mt-20">
        <p class="text-center">Drag the correct algebraic term below to match its definition above</p>
        <div class="grid grid-cols-3 justify-items-center gap-4 my-3">
          <div v-for="(item, index) in shuffledWords" :key="index"
            class="rounded-lg p-2.5 w-full lg:w-80 lg:h-30 text-white flex items-center justify-center cursor-pointer"
            :class="matchedPairs[item.word] ? 'bg-gray-400' : 'bg-[#36373A]'" draggable="true"
            @dragstart="handleDragStart($event, item.word)">
            <p>{{ item.word }}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="flex items-center justify-center gap-3 mt-10">
      <button @click="resetGame" class="border rounded-lg p-2">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
          class="size-6">
          <path stroke-linecap="round" stroke-linejoin="round"
            d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0 3.181 3.183a8.25 8.25 0 0 0 13.803-3.7M4.031 9.865a8.25 8.25 0 0 1 13.803-3.7l3.181 3.182m0-4.991v4.99" />
        </svg>

      </button>
      <button class="bg-[#5122A2] py-2 px-4 rounded-lg text-white">
        <div class="flex items-center justify-center gap-2">
          <p>Continue</p>
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
            stroke="currentColor" class="size-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M17.25 8.25 21 12m0 0-3.75 3.75M21 12H3" />
          </svg>
        </div>
      </button>
    </div>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";

const words = ref([
  { word: "Variable", meaning: "A container for storing data values in programming." },
  { word: "Function", meaning: "A reusable block of code that performs a specific task." },
  { word: "Array", meaning: "An ordered collection of elements, usually of the same type." },
  { word: "Object", meaning: "A data structure that stores key-value pairs." },
  { word: "Loop", meaning: "A control structure that repeats a block of code while a condition is met." }
]);

const draggedWord = ref(null);
const matchedPairs = ref({}); // Stores correctly matched pairs

const handleDragStart = (event, word) => {
  draggedWord.value = word;
  event.dataTransfer.setData("text/plain", word);
};

// Shuffle function
const shuffleArray = (array) => {
  return [...array].sort(() => Math.random() - 0.5);
};

// Computed property to get a randomized order
const shuffledWords = computed(() => shuffleArray(words.value));

const handleDrop = (event, meaningWord) => {
  const droppedWord = event.dataTransfer.getData("text/plain");

  if (droppedWord === meaningWord) {
    matchedPairs.value[droppedWord] = "correct"; // Correct match ✅
  } else {
    matchedPairs.value[droppedWord] = "wrong"; // Wrong match ❌
    // setTimeout(() => {
    //   delete matchedPairs.value[droppedWord]; // Remove the red ❌ after 1.5 seconds
    // }, 1500);
  }
};

const resetGame = () => {
  matchedPairs.value = {}; // Clear matched words
  words.value = [...originalWords]; // Reset words
};

</script>

<style scoped></style>