<script setup>
import { ref } from 'vue'
import MessageCard from './MessageCard.vue'
import './MessageList.css'

const count = ref(0)
</script>

<template>
  <div class="messages">
    <MessageCard />
  <MessageCard />
  </div>
</template>

