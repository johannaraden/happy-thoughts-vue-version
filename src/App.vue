<script setup>
import { ref, onMounted } from 'vue'
import MessageList from './components/MessageList.vue'
import MessageInput from './components/MessageInput.vue'

const messages = ref([])
const messageUrl = 'https://project-happy-thoughts-api-tfq9.onrender.com'

const fetchMessages = async () => {
  try {
        console.log('Fetching thoughts...')

    const response = await fetch(messageUrl)

    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`)
    }

    messages.value = await response.json()
        console.log('Fetched thoughts:', messages.value)

  } catch (error) {
    console.error('Failed to fetch thoughts:', error)
  }
}

onMounted(fetchMessages)

const onLiked = (likedThoughtId) => {
  messages.value = messages.value.map((thought) => {
    if (thought._id === likedThoughtId) {
      return { ...thought, hearts: thought.hearts + 1 }
    }

    return thought
  })
}
</script>

<template>
  <div>
  <MessageInput @submitted="() => {
    console.log('Submitted event received')
    fetchMessages()
  }" />
  <MessageList :messages="messages"
    @liked="onLiked" />
  </div>
</template>
