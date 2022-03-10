<template>
  <div
    v-if="props.show"
    class="fixed w-full h-full z-10 backdrop-blur-md flex items-center justify-center"
    @click.self="emit('closeMultiLetterSelector')"
  >
    <div
      class="grid grid-cols-4 grid-flow-row gap-5 p-4 shadow-lg rounded-3xl bg-white dark:bg-gray-800"
    >
      <button
        v-for="(item, index) in alphabetArray"
        :data-letter="item"
        class="w-12 aspect-square rounded-full mx-auto border-2 border-gray-500 cursor-pointer sm:hover:bg-gray-500 sm:hover:text-gray-100"
        :class="{
          'bg-gray-500 border-gray-500 text-gray-100':
            (props.currentBtn === 6
              ? props.information.yellowLetters
              : props.information.grayLetters).has(item)
        }"
        @click.self="emit('toggleLetterInLetterSet', item)"
      >
        {{ item }}
      </button>
    </div>
  </div>
</template>

<script setup>
  import { defineProps } from 'vue'

  const props = defineProps({
    show: {
      type: Boolean,
      required: true
    },
    information: {
      type: Object,
      required: true
    },
    currentBtn: {
      type: Number,
      required: true
    }
  })

  const emit = defineEmits([
    'closeMultiLetterSelector',
    'toggleLetterInLetterSet'
  ])

  const alphabetArray = [
    'A',
    'B',
    'C',
    'D',
    'E',
    'F',
    'G',
    'H',
    'I',
    'J',
    'K',
    'L',
    'M',
    'N',
    'O',
    'P',
    'Q',
    'R',
    'S',
    'T',
    'U',
    'V',
    'W',
    'X',
    'Y',
    'Z'
  ]
</script>

<style lang="scss" scoped></style>
