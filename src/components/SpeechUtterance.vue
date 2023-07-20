<script setup>
import { watch } from 'vue';

const props = defineProps(['word'])

const sayWord = () => {
  const utterance = new SpeechSynthesisUtterance(props.word);
  speechSynthesis.speak(utterance);
};

watch(() => props.word, sayWord);

</script>

<template>
<button @click="sayWord">Say Word</button>
</template>