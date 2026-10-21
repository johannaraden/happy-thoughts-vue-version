<script setup>
import MessageCard from './MessageCard.vue'

defineProps({
  messages: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['liked'])
</script>

<template>
  <section class="messages">
    <MessageCard
      v-for="message in messages"
      :key="message._id"
      :id="message._id"
      :message="message.message"
      :hearts="message.hearts"
      :time="message.createdAt"
      @liked="emit('liked', $event)"
    />
  </section>
</template>

<style src="./MessageList.css" scoped></style>
