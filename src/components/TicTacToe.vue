<template>
  <div class="game-container">
    <h1>Tic Tac Toe</h1>
    
    <div v-if="showNameInput" class="player-input">
      <input 
        v-model="player1Name" 
        placeholder="Nome Giocatore 1 (X)"
        @keyup.enter="startGame"
      >
      <input 
        v-model="player2Name" 
        placeholder="Nome Giocatore 2 (O)"
        @keyup.enter="startGame"
      >
      <button @click="startGame">Inizia</button>
    </div>

    <div v-else class="game-board">
      <h2 v-if="winner && winner !== 'Draw'">
        Ha vinto {{ winner === 'X' ? player1Name : player2Name }}! 🎉
      </h2>
      <h2 v-else-if="winner === 'Draw'">Pareggio! 🤝</h2>
      <h2 v-else>
        È il turno di: {{ currentPlayer === 'X' ? player1Name : player2Name }} ({{ currentPlayer }})
      </h2>

      <TabelloneComponent
        :board="board"
        :game-ended="!!winner"
        @move="handleMove"
      />
      
      <button v-if="winner" @click="resetGame">Ricomincia</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import TabelloneComponent from './GameBoard.vue'

// State reattivo usando Composition API
const board = ref(['', '', '', '', '', '', '', '', ''])
const currentPlayer = ref('X')
const winner = ref(null)
const player1Name = ref('Giocatore 1')
const player2Name = ref('Giocatore 2')
const showNameInput = ref(true)

// Combinazioni vincenti (può essere const perché non cambia mai)
const winningCombinations = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8], // righe
  [0, 3, 6], [1, 4, 7], [2, 5, 8], // colonne
  [0, 4, 8], [2, 4, 6]             // diagonali
]

// Funzioni del gioco
const handleMove = (index) => {
  // Controllo se la cella è già occupata o se il gioco è finito
  if (board.value[index] !== '' || winner.value) {
    return
  }

  // Aggiorna la cella con il giocatore corrente
  board.value[index] = currentPlayer.value

  // Controlla se c'è un vincitore
  checkWinner()

  // Se il gioco non è finito, cambia il turno
  if (!winner.value) {
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
  }
}

const checkWinner = () => {
  // Controlla tutte le combinazioni vincenti
  for (const combination of winningCombinations) {
    const [a, b, c] = combination
    if (
      board.value[a] && 
      board.value[a] === board.value[b] && 
      board.value[a] === board.value[c]
    ) {
      winner.value = board.value[a]
      return
    }
  }

  // Controlla se è pareggio (nessuna cella vuota)
  if (!board.value.includes('')) {
    winner.value = 'Draw'
  }
}

const startGame = () => {
  // Usa nomi di default se i campi sono vuoti
  if (player1Name.value.trim() === '') {
    player1Name.value = 'Giocatore 1'
  }
  if (player2Name.value.trim() === '') {
    player2Name.value = 'Giocatore 2'
  }
  
  showNameInput.value = false
}

const resetGame = () => {
  // Reset dello stato di gioco
  board.value = ['', '', '', '', '', '', '', '', '']
  currentPlayer.value = 'X'
  winner.value = null
  player1Name.value = 'Giocatore 1'
  player2Name.value = 'Giocatore 2'
  showNameInput.value = true
}
</script>

<style scoped>
.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: #f0f2f5;
  color: #333;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  padding: 20px;
  box-sizing: border-box;
}

h1 {
  font-size: 3em;
  color: #007bff;
  margin-bottom: 20px;
}

h2 {
  font-size: 1.5em;
  margin: 10px 0 30px;
  min-height: 30px;
}

.player-input {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 30px;
  background: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.player-input input {
  padding: 10px;
  font-size: 1em;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 250px;
}

.game-board {
  display: flex;
  flex-direction: column;
  align-items: center;
}

button {
  padding: 12px 24px;
  font-size: 1.1em;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  transition: background-color 0.2s, transform 0.2s;
  font-weight: bold;
}

button:hover {
  background-color: #2c3e50;
  transform: translateY(-2px);
}
</style>