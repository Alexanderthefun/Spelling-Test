<script setup>
import data from './data'
import { ref, computed } from 'vue'
import speechUtterance from './components/SpeechUtterance.vue'
import SpellingScore from './components/SpellingScore.vue'

const questions = ref([...data]);
const activeIndex = ref(0);
const userInput = ref('');

const testFinished = computed(() => {
  return activeIndex.value >= questions.value.length
});

const handleSubmit = () => {
  const activeQuestion = questions.value[activeIndex.value]
  activeQuestion.userInput = userInput.value
  activeIndex.value++
  userInput.value = ''
};

const refreshPage = () => {
  window.location.reload();
};
</script>

<template>
  <div id="app">
    <div v-if="!testFinished">
      <p>Word {{ activeIndex + 1 }} of {{ questions.length }}</p>
      <speechUtterance :word="questions[activeIndex].word" />
      <form @submit.prevent="handleSubmit">
        <input spellcheck="false" v-model="userInput" type="text" placeholder="Spell the word" />
        <button type="submit">Submit</button>
      </form>
    </div>
    <div v-else>
      <SpellingScore v-if="testFinished" :questions="questions" />
      <button @click="refreshPage">Try Again</button>
    </div>
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
