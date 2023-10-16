<script setup>
import { ref, onMounted } from 'vue';

const speech = ref(null)
const texts = ref([])

onMounted(() => {
  if (!('SpeechRecognition' in window) && !('webkitSpeechRecognition' in window)) {
    alert("お使いのブラウザは音声認識に対応しておりません")
    return
  }
  // マイク準備
  const SpeechRecognition = window.SpeechRecognition || webkitSpeechRecognition;
  speech.value = new SpeechRecognition()
  speech.value.lang = 'ja-JP'
  speech.value.onresult = (e) => {
    // 音声認識のテキストを取得
    if (e.results[0].isFinal) {
      texts.value.push(e.results[0][0].transcript)
    }
  }
  speech.value.onend = () => {
    // 音声認識が切断されたので再度開始
    speech.value.start()
  }
  speech.value.start()
})

</script>

<template>
    <h1>webkitSpeechRecognitionでマイクから音声認識</h1>
    マイクに向かってなにか喋ってください。
    <div v-for="(text) in texts" v-bind:key="text">
      {{ text }}
    </div>
</template>

