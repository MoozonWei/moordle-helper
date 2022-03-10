<script setup>
  import { computed, reactive, ref } from 'vue'
  import { allWords } from './wordOfTheDay'
  import LetterSelectBtn from './components/LetterSelectBtn.vue'
  import OneLetterSelector from './components/OneLetterSelector.vue'
  import MultiLetterSelector from './components/MultiLetterSelector.vue'

  const showOneLetterSelector = ref(false)
  const showMultiLetterSelector = ref(false)
  const currentBtn = ref(0)
  const information = reactive({
    greenLetters: ['?', '?', '?', '?', '?'],
    yellowLetters: new Set(),
    grayLetters: new Set()
  })

  console.log(
    allWords
      .filter((word) => /\w{2}f\w{2}/.test(word))
      .filter((word) => /\w{2}f\w{2}/.test(word))
      .filter((word) => /[^juicebrvhntys]{5}/.test(word))
      .filter((word) => /\w*o\w*/.test(word))
      .filter((word) => /\w*l\w*/.test(word))
      .filter((word) => /\w*a\w*/.test(word))
  )

  const regArr = computed(() => {
    const result = []
    information.greenLetters.forEach((e, i) => {
      if (e === '?') return
      const str = `\\w{${i}}${e}\\w{${4 - i}}`
      result.push(new RegExp(str))
    })
    information.yellowLetters.forEach((e) => {
      const str = `\\w*${e}\\w*`
      result.push(new RegExp(str))
    })
    if (information.grayLetters.size) {
      result.push(
        new RegExp(`[^${Array.from(information.grayLetters).join('')}]`)
      )
    }
    console.log(result)
    return result
  })

  const resultArr = computed(() => {
    let result = allWords
    for (let i = 0; i < regArr.value.length; i++) {
      result = result.filter((word) => regArr.value[i].test(word.toUpperCase()))
    }
    return result
  })

  document.documentElement.onclick = (e) => {
    e.preventDefault()
  }

  // handle method
  const handleShowOneLetterSelector = (e, i) => {
    currentBtn.value = i
    showOneLetterSelector.value = true
    console.log(showOneLetterSelector.value)
  }
  const handleShowMultiLetterSelector = (e, i) => {
    currentBtn.value = i
    console.log(i)
    showMultiLetterSelector.value = true
    console.log(showOneLetterSelector.value)
  }
  const handleSelectOneGreenLetter = (selectedLetter) => {
    information.greenLetters[currentBtn.value - 1] = selectedLetter
    information.greenLetters = new Array(...information.greenLetters)
  }
  const handleToggleLetterInLetterSet = (selectedLetter) => {
    const colorLetters =
      currentBtn.value === 6 ? 'yellowLetters' : 'grayLetters'
    information[colorLetters].has(selectedLetter)
      ? information[colorLetters].delete(selectedLetter)
      : information[colorLetters].add(selectedLetter)
  }
</script>

<template>
  <div class="max-w-3xl w-full">
    <div
      class="flex justify-center items-center h-16 shadow-lg rounded-xl mt-2 mx-4 mb-6"
    >
      <div class="flex-1"></div>
      <span class="text-3xl font-bold">MOORDLE · HELPER</span>
      <div class="flex-1"></div>
    </div>
    <div>
      <div class="flex justify-between mx-8">
        <LetterSelectBtn
          v-for="i in 5"
          :key="i"
          color="green"
          :letter="information.greenLetters[i - 1]"
          @showOneLetterSelector="handleShowOneLetterSelector(e, i)"
        />
      </div>
    </div>
    <div class="flex justify-between mx-8 items-center">
      <LetterSelectBtn
        color="yellow"
        :letter="information.yellowLetters.size === 0 ? '?' : '...'"
        @showMultiLetterSelector="handleShowMultiLetterSelector(e, 6)"
      />
      <div>
        <div class="stat text-center">
          <div class="stat-title">Total Result</div>
          <div class="stat-value">{{resultArr.length}}</div>
        </div>
      </div>
      <LetterSelectBtn
        color="gray"
        :letter="information.grayLetters.size === 0 ? '?' : '...'"
        @showMultiLetterSelector="handleShowMultiLetterSelector(e, 7)"
      />
    </div>
  </div>
  <div class="divider font-bold">RESULT</div>
  <div class="overflow-auto h-full">
    <ul class="grid grid-cols-3 grid-flow-row gap-y-2 text-center my-4">
      <li v-for="(item, index) in (resultArr.length <= 99 ? resultArr : resultArr.slice(0, 99))" :key="index">
        <span>{{ item }}</span>
      </li>
    </ul>
  </div>

  <OneLetterSelector
    :show="showOneLetterSelector"
    :current-btn="currentBtn"
    :green-letters="information.greenLetters"
    @closeOneLetterSelector="showOneLetterSelector = false"
    @selectOneGreenLetter="handleSelectOneGreenLetter"
  />
  <MultiLetterSelector
    :show="showMultiLetterSelector"
    :current-btn="currentBtn"
    :information="information"
    @closeMultiLetterSelector="showMultiLetterSelector = false"
    @toggleLetterInLetterSet="handleToggleLetterInLetterSet"
  />
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
