<template>
  <div class="board">
    <GameCell
      v-for="(cell, index) in board"
      :key="index"
      :value="cell"
      :index="index"
      :disabled="gameEnded"
      @cell-click="handleCellClick"
    />
  </div>
</template>

<script setup>
import GameCell from './GameCell.vue'

// Props ricevute dal componente padre
defineProps({
  board: {
    type: Array,
    required: true,
    validator: (board) => board.length === 9
  },
  gameEnded: {
    type: Boolean,
    default: false
  }
})

// Eventi che il componente può emettere
const emit = defineEmits(['move'])

// Funzione per gestire il click su una cella
const handleCellClick = (index) => {
  emit('move', index)
}
</script>

<style scoped>
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
</style>