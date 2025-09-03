<template>
  <div
    class="cell"
    @click="handleClick"
  >
    {{ value }}
  </div>
</template>

<script setup>
// Props ricevute dal componente padre
const props = defineProps({
  value: {
    type: String,
    default: ''
  },
  index: {
    type: Number,
    required: true
  },
  disabled: {
    type: Boolean,
    default: false
  }
})

// Eventi che il componente può emettere
const emit = defineEmits(['cell-click'])

// Funzione per gestire il click sulla cella
const handleClick = () => {
  if (props.value === '' && !props.disabled) {
    emit('cell-click', props.index)
  }
}
</script>

<style scoped>
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

.cell:hover:empty {
  background-color: #e2e6ea;
}
</style>