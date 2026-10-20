
<script setup>
import { computed } from 'vue'
import moment from 'moment'

const props = defineProps({
  id: [String, Number],
  message: String,
  hearts: {
    type: Number,
    default: 0
  },
  time: String
})

const emit = defineEmits(['liked'])

const likeClick = async () => {
  try {
    const response = await fetch(
      `https://technigo-thoughts.herokuapp.com/${props.id}/like`,
      {
        method: 'POST',
        body: '',
        headers: { 'Content-Type': 'application/json' }
      }
    )

    if (!response.ok) {
      throw new Error(`Request failed: ${response.status}`)
    }

    emit('liked', props.id)
  } catch (error) {
    console.error('Error liking thought:', error)
  }
}

const heartClass = computed(() =>
  props.hearts > 10
    ? 'lots'
    : props.hearts > 5
      ? 'few'
      : 'none'
)
</script>

<template>
  <section class="card">
    <h4>{{ message }}</h4>

    <div class="card-details">
      <span :class="heartClass" @click="likeClick">
        <img
          class="heart"
          alt="heart-icon"
          src="https://img.icons8.com/cotton/64/000000/like--v1.png"
        />
      </span>

      <p class="like-p">x {{ hearts }}</p>
      <p class="time">{{ moment(time).fromNow() }}</p>
    </div>
  </section>
</template>

<style src="./MessageCard.css" scoped></style>