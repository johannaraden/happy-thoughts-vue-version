<script setup>
import { ref } from 'vue'


const messageUrl = 'https://technigo-thoughts.herokuapp.com/'

// Reactive state
const message = ref('')

// Form submission
const handleSubmit = async () => {
  try {
    await fetch(messageUrl, {
      method: 'POST',
      body: JSON.stringify({ message: message.value }),
      headers: {
        'Content-Type': 'application/json'
      }
    })

  } catch (error) {
    console.log('error:', error)
  }
}
</script>

<template>
   <div>
    <form class="message-form" @submit.prevent="handleSubmit">
      <h3>What's making you happy right now?</h3>

      <textarea
        rows="2"
        v-model="message"
      ></textarea>

      <button
        class="submit-button"
        type="submit"
        :disabled="message.length < 6 || message.length > 140"
      >
        Send Happy Thought
      </button>

      <p>{{ message.length }}/140</p>
    </form>
  </div>
</template>

<style src="./MessageInput.css" scoped></style>