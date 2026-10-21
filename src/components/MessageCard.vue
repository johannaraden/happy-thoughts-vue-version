
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

const messageUrl = 'https://project-happy-thoughts-api-tfq9.onrender.com'


const emit = defineEmits(['liked'])

const likeClick = async () => {
  try {
    const response = await fetch(
      `${messageUrl}/${props.id}/like`,
      {
        method: 'POST'
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
      <button
  type="button"
  :class="heartClass"
  aria-label="Like this thought"
  @click="likeClick"
>
  <img
    class="heart"
    alt=""
    src="https://img.icons8.com/cotton/64/000000/like--v1.png"
  />
</button>

      <p class="like-p">x {{ hearts }}</p>
      <p class="time">{{ moment(time).fromNow() }}</p>
    </div>
  </section>
</template>

<style src="./MessageCard.css" scoped></style>