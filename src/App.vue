<script setup>
import { ref, computed } from 'vue'

const message = ref(`Welcome! Press [Start Game] to play.`)
const gameStarted = ref(false)
const turns = ref(0)
const board = ref({
  0: null,
  1: null,
  2: null,
  3: null,
  4: null,
  5: null,
  6: null,
  7: null,
  8: null,
})
let symbols = ['X', 'O']
const winningCombinations = [[0, 1, 2], [3, 4, 5], [6, 7, 8], [0, 3, 6], [1, 4, 7], [2, 5, 8], [0, 4, 8], [2, 4, 6]]
const winningSymbol = ref(null)
const currentSymbol = computed(() => {
  return symbols[turns.value % 2]
})

function startGame() {
  if (winningSymbol.value) {
    winningSymbol.value === 'X' ? symbols = ['O', 'X'] : symbols = ['X', 'O']
  } else if (turns.value > 0) {
    symbols = symbols.reverse()
  }
  winningSymbol.value = null
  turns.value = 0
  board.value = {
    0: null,
    1: null,
    2: null,
    3: null,
    4: null,
    5: null,
    6: null,
    7: null,
    8: null,
  }
  gameStarted.value = true
  message.value = `${currentSymbol.value}'s turn...`
}
function insertSymbol(n) {
  board.value[n] = currentSymbol.value
  turns.value++
  findWinner()
}
function findWinner() {
  let temp = false
  winningCombinations.every(c => {
    if ((board.value[c[0]] && board.value[c[0]] === board.value[c[1]]) && (board.value[c[0]] && board.value[c[0]] === board.value[c[2]])) {
      temp = true
      stopGame(board.value[c[0]])
      message.value = `${winningSymbol.value} won!`
      return false
    }
    message.value = `${currentSymbol.value}'s turn...`
    return true
  })
  !temp ? isCatGame() : null
  return temp
}
function isCatGame() {
  if (turns.value === 9 && !winningSymbol.value) {
    gameStarted.value = false
    message.value = `Cat game! Try again.`
  }
}
function stopGame(w) {
  gameStarted.value = false
  winningSymbol.value = w
}
</script>

<template>
  <div class="w-full h-full flex flex-col justify-center items-center gap-6">
    <button
      type="button"
      class="
        items-center
        rounded-md border
        border-gray-300
        bg-white
        disabled:bg-gray-50
        px-6
        py-3
        text-base
        font-medium
        text-gray-700
        shadow-sm
        hover:bg-gray-50
        disabled:hover:bg-gray-50
        focus:outline-none
        focus:ring-2
        focus:ring-indigo-500
        focus:ring-offset-2
        disabled:cursor-not-allowed
      "
      @click="startGame()"
      :disabled="gameStarted"
    >Start Game</button>
    <div class="text-indigo-600 font-light">{{ message }}</div>
    <div class="flex justify-center items-center">
      <div class="grid grid-cols-3">
        <button
          v-for="n in [...Array(9).keys()]"
          :key="n"
          class="relative flex justify-center items-center h-40 w-40 border text-8xl bg-white"
          :class="[board[n] || !gameStarted ? 'cursor-inherit bg-gray-100' : 'cursor-pointer']"
          :disabled="(board[n] || !gameStarted)"
          @click="insertSymbol(n)"
        >
          {{ board[n] ? board[n] : '' }}
          <!-- <div class="absolute h-40 w-40 flex justify-center items-center text-gray-900/30">
            {{ 'Hi' }}
          </div> -->
        </button>
      </div>
    </div>
  </div>
</template>
