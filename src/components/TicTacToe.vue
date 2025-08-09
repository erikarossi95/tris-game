<template>
  <div class="game-container">
    <h1>Tic Tac Toe</h1>
    
    <div v-if="showNameInput" class="player-input">
      <input v-model="player1Name" placeholder="Nome Giocatore 1 (X)">
      <input v-model="player2Name" placeholder="Nome Giocatore 2 (O)">
      <button @click="startGame">Inizia</button>
    </div>

    <div v-else class="game-board">
      <h2 v-if="winner && winner !== 'Draw'">Ha vinto {{ winner === 'X' ? player1Name : player2Name }}! 🎉</h2>
      <h2 v-else-if="winner === 'Draw'">Pareggio! 🤝</h2>
      <h2 v-else>È il turno di: {{ currentPlayer === 'X' ? player1Name : player2Name }} ({{ currentPlayer }})</h2>

      <div class="board">
        <div
          v-for="(cell, index) in board"
          :key="index"
          class="cell"
          @click="handleCellClick(index)"
        >
          {{ cell }}
        </div>
      </div>
      
      <button v-if="winner" @click="resetGame">Ricomincia</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TicTacToe',
  data() {
    return {
      board: ['', '', '', '', '', '', '', '', ''],
      currentPlayer: 'X',
      winner: null,
      winningCombinations: [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
      ],
      player1Name: 'Giocatore 1',
      player2Name: 'Giocatore 2',
      showNameInput: true,
    };
  },
  methods: {
    handleCellClick(index) {
      if (this.board[index] !== '' || this.winner || this.showNameInput) {
        return;
      }

      this.board.splice(index, 1, this.currentPlayer);

      this.checkWinner();

      if (!this.winner) {
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
    },
    checkWinner() {
      for (const combination of this.winningCombinations) {
        const [a, b, c] = combination;
        if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c]) {
          this.winner = this.board[a];
          return;
        }
      }

      if (!this.board.includes('')) {
        this.winner = 'Draw';
      }
    },
    resetGame() {
      this.board = ['', '', '', '', '', '', '', '', ''];
      this.currentPlayer = 'X';
      this.winner = null;
      this.player1Name = 'Giocatore 1';
      this.player2Name = 'Giocatore 2';
      this.showNameInput = true;
    },
    startGame() {
      if (this.player1Name.trim() === '') {
        this.player1Name = 'Giocatore 1';
      }
      if (this.player2Name.trim() === '') {
        this.player2Name = 'Giocatore 2';
      }
      this.showNameInput = false;
    }
  }
};
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

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 8px;
  margin-bottom: 30px;
  background: #fff;
  padding: 8px;
  border-radius: 10px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.cell {
  width: 100px;
  height: 100px;
  background-color: #f8f9fa;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 4em;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s, color 0.2s;
  border-radius: 5px;
  color: #2c3e50;
}

.cell:hover {
  background-color: #e2e6ea;
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