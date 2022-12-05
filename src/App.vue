<script setup>
import { ref } from 'vue'

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
const symbols = ['X', 'O']

function insertSymbol(n) {
  board.value[n] = 'X'
}
</script>

<template>
  <div class="w-full h-full flex justify-center items-center">
    <div class="grid grid-cols-3">
      <button
        v-for="n in [...Array(9).keys()]"
        :key="n"
        class="relative flex justify-center items-center h-40 w-40 border text-8xl bg-white"
        :class="[board[n] ? 'cursor-inherit' : 'cursor-pointer']"
        :disabled="board[n]"
        @click="insertSymbol(n)"
      >
        {{ board[n] }}
        <div class="absolute h-40 w-40 flex justify-center items-center text-gray-900/30">
          {{ 'Hi' }}
        </div>
      </button>
    </div>
  </div>
</template>
