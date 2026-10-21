<script setup>
import { ref } from 'vue'

const messageUrl = 'https://project-happy-thoughts-api-tfq9.onrender.com'
const message = ref('')
const emit = defineEmits(['submitted'])


const handleSubmit = async () => {
  const trimmedMessage = message.value.trim()
  try {
      const response = await fetch(messageUrl, {
      method: 'POST',
      body: JSON.stringify({ message: trimmedMessage }),
      headers: {
        'Content-Type': 'application/json'
      }
    })

    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`)
    }

    message.value = ''
    emit('submitted')
  } catch (error) {
    console.log('error:', error)
  }
}
</script>

<template>
   <div>



<main class="page">
  <h1 class="message-form__title"><span>Happy Thoughts</span></h1>

  <form class="message-form" @submit.prevent="handleSubmit">
    <h2 class="message-form_prompt">What's making you happy right now?</h2>

          <textarea
          class="thought"
        rows="2"
        placeholder="Something small is fine…"
        v-model="message"
        maxlength="140"
        aria-labelledby="prompt"
        required
      ></textarea>
      <button
        class="submit-button"
        type="submit"
        :disabled="message.trim().length < 5 || message.trim().length > 140"
      >
        Send Happy Thought
      </button>
    <div class="counter" id="counter" aria-live="polite">{{ message.length }}/140</div>
  </form>

  <ul class="messages" id="messages" aria-label="Happy thoughts"></ul>
</main>




  </div>
</template>

<style src="./MessageInput.css" scoped></style>