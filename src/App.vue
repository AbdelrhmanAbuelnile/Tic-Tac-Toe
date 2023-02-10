<script setup>
import { ref , computed} from 'vue'
const player = ref('X')
const board = ref([
  ['','',''],
  ['','',''],
  ['','',''],
  
])

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2], // row win
    [3, 4, 5], // row win
    [6, 7, 8], // row win
    [0, 3, 6], // column win
    [1, 4, 7], // column win
    [2, 5, 8], // column win
    [0, 4, 8], // digonal win
    [2, 4, 6], // digonal win
  ];

  for (let i = 0; i< lines.length; i++){
    const [a, b, c] = lines[i]
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]){
      return squares[a]
    }
  }
  return null 
}

const winner = computed( () => calculateWinner(board.value.flat()) )

const MakeMove = (x, y) => {
  if(winner.value) return

  if(board.value[x][y] !== '') return 

  board.value[x][y] = player.value

  player.value = player.value === 'X'? 'O' : 'X' 

}

const RestGame = () => {
  board.value = [
    ['','',''],
    ['','',''],
    ['','',''],
  ]
  player.value = 'X'
}

</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class=" mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <h3 class=" text-xl mb-4 ">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div
        v-for="(row, x) in board"
        :key="x"
        class="flex "
      >
        <div
        v-for="(cell, y) in row"
        :key="y"
        @click="MakeMove(x, y)"
        :class="`border-2 border-black dark:border-white w-20 h-20 hover:bg-gray-300 dark:hover:bg-gray-700
        flex items-center justify-center material-icons-outlined text-4xl cursor-pointer
        ${cell === 'X' ? 'text-pink-500' : 'text-blue-500'}`"
        >
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
        </div>
      </div>
    </div>

    <h2 v-if="winner" class="text-6xl font-bold mb-8">
      player <span v-if="winner === 'X'" class=" text-pink-500">{{ winner }}</span>
      <span v-if="winner === 'O'" class=" text-blue-500">{{ winner }}</span> wins!
    </h2>
    <button @click="RestGame" class="px-4 py-2 bg-pink-500 hover:bg-pink-600 duration-300 rounded">
      Rest Game
    </button>
    
  </main>
</template>

<style>

</style>
