<script setup>
  import { computed, onMounted, reactive, ref } from 'vue'
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
  const arrScope = ref(99)

  const scrollContent = ref(null)
  const list = ref(null)

  // console.log(
  //   allWords
  //     .filter((word) => /\w{2}f\w{2}/.test(word))
  //     .filter((word) => /\w{2}f\w{2}/.test(word))
  //     .filter((word) => /[^juicebrvhntys]{5}/.test(word))
  //     .filter((word) => /\w*o\w*/.test(word))
  //     .filter((word) => /\w*l\w*/.test(word))
  //     .filter((word) => /\w*a\w*/.test(word))
  // )

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
        new RegExp(`[^${Array.from(information.grayLetters).join('')}]{5}`)
      )
    }
    return result
  })

  const resultArr = computed(() => {
    let result = allWords
    for (let i = 0; i < regArr.value.length; i++) {
      result = result.filter((word) => regArr.value[i].test(word.toUpperCase()))
    }
    setTimeout(() => {
      list.value.style.paddingTop = ''
      scrollContent.value.scrollTop = 0
      arrScope.value = 0
    }, 1)
    return result
  })

  document.documentElement.onclick = (e) => {
    e.preventDefault()
  }

  // handle method
  const handleShowOneLetterSelector = (e, i) => {
    currentBtn.value = i
    showOneLetterSelector.value = true
  }
  const handleShowMultiLetterSelector = (e, i) => {
    currentBtn.value = i
    showMultiLetterSelector.value = true
  }
  const handleSelectOneGreenLetter = (selectedLetter) => {
    information.greenLetters[currentBtn.value - 1] = selectedLetter
    information.greenLetters = new Array(...information.greenLetters)
    showOneLetterSelector.value = false
  }
  const handleToggleLetterInLetterSet = (selectedLetter) => {
    const colorLetters =
      currentBtn.value === 6 ? 'yellowLetters' : 'grayLetters'
    information[colorLetters].has(selectedLetter)
      ? information[colorLetters].delete(selectedLetter)
      : information[colorLetters].add(selectedLetter)
  }
  const handleReset = () => {
    information.greenLetters = ['?', '?', '?', '?', '?']
    information.yellowLetters = new Set()
    information.grayLetters = new Set()
  }
  const handleScroll = () => {
    const getPaddingTop = (node) => {
      return node.style.paddingTop === '' ? 0 : parseInt(node.style.paddingTop)
    }
    const [scrollTop, scrollHeight, offsetHeight] = [
      scrollContent.value.scrollTop,
      scrollContent.value.scrollHeight,
      scrollContent.value.offsetHeight
    ]
    const remToPx = parseInt(
      window.getComputedStyle(document.documentElement)['fontSize']
    )
    const rowShowNum = Math.ceil(offsetHeight / (remToPx * 2))
    const paddingTop = getPaddingTop(list.value)
    // scrollTop + offsetHeight = scrollHeight
    if (scrollTop + offsetHeight >= scrollHeight && (arrScope.value + 99) <= resultArr.value.length) {
      arrScope.value += (32 - rowShowNum) * 3
      list.value.style.paddingTop = paddingTop + (32 - rowShowNum) * 2 + 'rem'
    }
    if (scrollTop > 0 && scrollTop <= paddingTop * remToPx) {
      arrScope.value -= (32 - rowShowNum) * 3
      list.value.style.paddingTop = paddingTop - (32 - rowShowNum) * 2 + 'rem'
    }
  }
</script>

<template>
  <div class="max-w-3xl w-full rounded-3xl shadow-lg">
    <div class="flex justify-center items-center m-4">
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
    <div class="flex justify-between mx-8 items-center mt-2">
      <LetterSelectBtn
        color="yellow"
        :letter="information.yellowLetters.size === 0 ? '?' : '...'"
        @showMultiLetterSelector="handleShowMultiLetterSelector(e, 6)"
      />
      <div>
        <div class="stat text-center p-0">
          <div class="stat-title text-sm">Total Result</div>
          <div class="stat-value text-2xl">{{ resultArr.length }}</div>
        </div>
      </div>
      <LetterSelectBtn
        color="gray"
        :letter="information.grayLetters.size === 0 ? '?' : '...'"
        @showMultiLetterSelector="handleShowMultiLetterSelector(e, 7)"
      />
    </div>
    <div class="mx-8 mt-2 mb-4 grid grid-cols-1 grid-rows-1 gap-4">
      <button class="shadow-lg rounded-lg p-2" @click="handleReset">
        RESET
      </button>
      <!-- <button
        v-if="false"
        class="shadow-lg rounded-lg p-2 text-center"
      >
        BACK TO TOP
      </button> -->
    </div>
  </div>
  <div ref="scrollContent" class="overflow-auto h-full" @scroll="handleScroll">
    <ul ref="list" class="grid grid-cols-3 grid-flow-row text-center my-4">
      <li
        v-for="(item, index) in resultArr.length <= 99
          ? resultArr
          : resultArr.slice(arrScope, arrScope + 99)"
        :key="index"
        class="h-8"
      >
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
