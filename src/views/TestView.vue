<template>
    <main class="p-8">
        <div>
            <router-link to="/">
                <img src="../assets/back.svg" />
            </router-link>
        </div>
        <div class="my-3 flex justify-between items-center bg-[#5122A2] p-2.5 rounded-lg text-white">
            <div>
                <h2>Goal: 50 points</h2>
            </div>
            <div>
                <h2>Current Points: {{ score }}</h2>
            </div>
        </div>
        <div v-for="(quiz, index) in questions" :key="quiz.id">
            <div class="my-4">
                <h1 class="text-gray-700">Question {{ index + 1 }} </h1>
                <p class="text-gray-700">{{ quiz.question }}</p>
            </div>
            <div class="my-2">
                <div v-for="(option, key) in quiz.options" :key="key"
                    class="border-[1.5px] border-[#BCBCBC] rounded-lg p-2 my-2 cursor-pointer"
                    :class="getOptionClass(quiz.id, key, quiz.correctAnswer)"
                    @click="checkAnswer(quiz.id, key, quiz.correctAnswer)">
                    <p>{{ key }}: {{ option }}</p>
                </div>
            </div>
            <div v-if="selectedAnswers[quiz.id]">
                <div  v-if="selectedAnswers[quiz.id] !== quiz.correctAnswer" class="flex items-start  gap-2 p-2 bg-red-100 rounded-lg">
                    <img src="../assets/wrong.svg">
                    <div>
                        <h4 class="text-red-500">Think again!</h4>
                        <p class="text-red-500">{{ quiz.question }}</p>
                    </div>    
                </div>
                <div  v-else class="flex items-start  gap-2 p-2 bg-green-100 rounded-lg" >
                    <img src="../assets/right.svg">
                    <div>
                        <h4 class="text-green-500">Right !</h4>
                        <p class="text-green-500">{{ quiz.question }}</p>
                    </div>  
                </div>
            </div>
        </div>
        
    </main>
</template>

<script setup>
import { reactive, ref } from 'vue';

const score = ref(0);
const questions = ref([{
    id: 1,
    question: "What is the main purpose of photosynthesis?",
    options: {
        A: "To produce carbon dioxide",
        B: "To convert sunlight into chemical energy",
        C: "To break down glucose",
        D: "To generate heat for the plant"
    },
    correctAnswer: "B"
},
{
    id: 2,
    question: "Which organelle is responsible for photosynthesis?",
    options: {
        A: "Mitochondria",
        B: "Chloroplast",
        C: "Nucleus",
        D: "Ribosome"
    },
    correctAnswer: "B"
},
{
    id: 3,
    question: "What pigment in plants absorbs sunlight for photosynthesis?",
    options: {
        A: "Carotene",
        B: "Xanthophyll",
        C: "Chlorophyll",
        D: "Anthocyanin"
    },
    correctAnswer: "C"
},
{
    id: 4,
    question: "What are the main products of photosynthesis?",
    options: {
        A: "Oxygen and glucose",
        B: "Carbon dioxide and water",
        C: "Glucose and nitrogen",
        D: "Oxygen and carbon dioxide"
    },
    correctAnswer: "A"
},
{
    id: 5,
    question: "Which gas is absorbed by plants during photosynthesis?",
    options: {
        A: "Oxygen",
        B: "Nitrogen",
        C: "Carbon dioxide",
        D: "Hydrogen"
    },
    correctAnswer: "C"
}
]);

const selectedAnswers = reactive({});
const userFeedback = reactive({});

const checkAnswer = (questionId, selectedOption, correctOption) => {
    if (selectedAnswers[questionId]) return;

    selectedAnswers[questionId] = selectedOption;

    if (selectedOption === correctOption) {
        score.value += 10;
      }
};

const getOptionClass = (questionId, optionKey, correctAnswer) => {
    const selectedOption = selectedAnswers[questionId];

    if (!selectedOption) return "border-gray-400 bg-white"; // Default style

    if (selectedOption === optionKey) {
        return optionKey === correctAnswer
            ? "border-green-500 bg-green-100" // Correct answer
            : "border-red-500 bg-red-100"; // Incorrect answer
    }

    return "border-gray-300 bg-white"; // Unselected options remain default
}

</script>

<style scoped></style>