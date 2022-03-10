<template>
  <button
    v-if="props.color === 'green'"
    class="btn btn-square border-2 bg-green-500 border-green-500 hover:bg-green-500 hover:border-green-500 sm:hover:bg-green-700 sm:hover:border-green-700 text-white font-bold text-3xl"
    @click="emit('showOneLetterSelector')"
  >
    {{ props.letter }}
  </button>
  <button
    v-if="props.color === 'yellow'"
    class="btn btn-square border-2 bg-yellow-500 border-yellow-500 hover:bg-yellow-500 hover:border-yellow-500 sm:hover:bg-yellow-700 sm:hover:border-yellow-700 text-white font-bold text-3xl"
    @click="emit('showMultiLetterSelector')"
  >
    {{ props.letter }}
  </button>
  <button
    v-if="props.color === 'gray'"
    class="btn btn-square border-2 bg-gray-500 border-gray-500 hover:bg-gray-500 hover:border-gray-500 sm:hover:bg-gray-700 sm:hover:border-gray-700 text-white font-bold text-3xl"
    @click="emit('showMultiLetterSelector')"
  >
    {{ props.letter }}
  </button>
</template>

<script setup>
  import { defineProps } from 'vue'

  const props = defineProps({
    color: String,
    letter: String
  })

  const emit = defineEmits(['showOneLetterSelector', 'showMultiLetterSelector'])
</script>
