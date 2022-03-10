<script setup>
  import { allWords } from './wordOfTheDay'
  allWords.sort()
</script>

<template>
  <div class="max-w-3xl w-full">
    <div class="flex justify-center items-center h-16 shadow-lg rounded-xl mt-2 mx-4 mb-6">
      <div class="flex-1"></div>
      <span class="text-3xl font-bold">MOORDLE · HELPER</span>
      <div class="flex-1"></div>
    </div>
    <div>
      <div class="flex justify-between mx-8">
        <template v-for="(item, index) in 5" :key="index">
          <label
            class="btn btn-square border-2 bg-green-500 border-green-500 hover:bg-green-500 hover:border-green-500 sm:hover:bg-green-700 sm:hover:border-green-700"
            :for="`modal-${index}`"
          ></label>
          <input :id="`modal-${index}`" type="checkbox" class="modal-toggle" />
          <label :for="`modal-${index}`" class="modal cursor-pointer">
            <label
              class="modal-box relative grid grid-cols-4 grid-flow-row gap-x-12 gap-y-2"
              for=""
            >
            </label>
          </label>
        </template>
      </div>
    </div>
    <div class="flex justify-between mx-8 items-center">
      <label
        class="btn btn-square border-2 bg-yellow-500 border-yellow-500 hover:bg-yellow-500 hover:border-yellow-500 sm:hover:bg-yellow-700 sm:hover:border-yellow-700"
        for="modal-1"
      ></label>
      <input id="modal-1" type="checkbox" class="modal-toggle" />
      <label for="modal-1" class="modal cursor-pointer">
        <label class="modal-box relative" for=""></label>
      </label>
      <div>
        <div class="stat text-center">
          <div class="stat-title">Total Result</div>
          <div class="stat-value">89,400</div>
        </div>
      </div>
      <label
        class="btn btn-square border-2 bg-gray-500 border-gray-500 hover:bg-gray-500 hover:border-gray-500 sm:hover:bg-gray-700 sm:hover:border-gray-700"
        for="modal-1"
      ></label>
      <input id="modal-1" type="checkbox" class="modal-toggle" />
      <label for="modal-1" class="modal cursor-pointer">
        <label class="modal-box relative" for=""></label>
      </label>
    </div>
  </div>
  <div class="divider font-bold">RESULT</div>
  <div class="overflow-scroll">
    <ul class="grid grid-cols-3 grid-flow-row text-center mb-4">
      <li v-for="(item, index) in allWords.slice(0, 200)" :key="index">
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
  :root,
  body,
  #app {
    @apply w-full h-full flex flex-col items-center;
  }

  #app {
    @apply max-w-5xl;

    > * {
      @apply w-full;
    }
  }
</style>
