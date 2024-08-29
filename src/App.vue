<script setup>
import { ref, computed } from 'vue';

const questions = ref([
  { 
    question: 'What is the capital of France?',
    answers: ['Berlin', 'Madrid', 'Paris', 'Lisbon'],
    correctAnswerIndex: 2
  },
  { 
    question: 'What is 2 + 2?',
    answers: ['3', '4', '5', '6'],
    correctAnswerIndex: 1
  },
  { 
    question: 'Who wrote "To Kill a Mockingbird"?',
    answers: ['Harper Lee', 'J.K. Rowling', 'Ernest Hemingway', 'Mark Twain'],
    correctAnswerIndex: 0
  }
]);

const currentQuestionIndex = ref(0);
const selectedAnswerIndex = ref(null);
const answerSelected = ref(false);
const isCorrect = ref(false);

const currentQuestion = computed(function () {
  return questions.value[currentQuestionIndex.value];
});

const selectAnswer = function(index) {
  if(!answerSelected.value) {
    answerSelected.value = true;
    selectedAnswerIndex.value = index;
    if (currentQuestion.value.correctAnswerIndex === index) {
      isCorrect.value = true;
    } else {
      
    }
  }
}

const getButtonClass = function (index) {
  if (answerSelected.value) {
    if (index === selectedAnswerIndex.value) {
      return isCorrect.value ? 'bg-green-500 text-white' : 'bg-red-500 text-white';
    }
    if (index === currentQuestion.value.correctAnswerIndex) {
      return 'bg-green-500 text-white';
    }
    return 'bg-gray-200';
  }
}

const nextQuestion = function() {
  if (currentQuestionIndex.value < questions.value.length - 1) {
    currentQuestionIndex.value++;
    selectedAnswerIndex.value = null;
    answerSelected.value = false;
    isCorrect.value = false;
  }
}

const correctAnswer = computed(function() {
  return currentQuestion.value.answers[currentQuestion.value.correctAnswerIndex]
})

</script>

<template>
  <div class="container mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4 text-center">Trivia Quiz</h1>

    <div class="mb-4">
      <p class="text-lg">{{ currentQuestion.question }}</p>
    </div>

    <div>
      <button 
        v-for="(answer, index) in currentQuestion.answers"
        :key="index"
        @click="selectAnswer(index)"
        :class="getButtonClass(index)"
        class="block w-full mb-2 p-2 border rounded">{{ answer }}</button>
    </div>

    <div v-if="answerSelected">
      <p :class="{ 'text-green-500': isCorrect, 'text-red-500': !isCorrect }">
        {{ isCorrect ? 'Correct!' : 'Incorrect. The correct answer was: ' + correctAnswer }}
      </p>
      <button 
        @click="nextQuestion"
        class="mt-4 p-2 bg-blue-500 text-white rounded">Next Question</button>
    </div>
  </div>
</template>