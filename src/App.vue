<script setup>
import { ref, onMounted } from 'vue';

const gridStructure = ref([]);

const cellModel = ref({
  value: 0,
  isHole: false,
});

const rows = ref(3);
const cols = ref(3);
const holes = ref(2);

const fillGridStructure = () => {
  for (let r = 0; r < rows.value; r++) {
    gridStructure.value[r] = [];

    for (let c = 0; c < cols.value; c++) {
      gridStructure.value[r].push(cellModel.value);
    }
  }
};

const assignHoles = () => {
  let assignedHoles = 0;

  while (assignedHoles < holes.value) {
    const rowRandomIndex = Math.floor(Math.random() * rows.value);
    const colRandomIndex = Math.floor(Math.random() * cols.value);

    const randomCell = gridStructure.value[rowRandomIndex][colRandomIndex];

    if (!randomCell.isHole) {
      gridStructure.value[rowRandomIndex][colRandomIndex] = {
        isHole: true,
        value: 'H',
      };

      assignedHoles++;
    }
  }

  handleAdjacentCells();
};

const handleAdjacentCells = () => {
  for (let currentRow = 0; currentRow < rows.value; currentRow++) {
    for (let currentCol = 0; currentCol < cols.value; currentCol++) {
      if (!gridStructure.value[currentRow][currentCol].isHole) {
        let holeCount = 0;
        const adjacentCells = getAdjacentCells(currentRow, currentCol);

        adjacentCells.forEach((cell, i) => {
          if (adjacentCells[i].isHole) {
            holeCount++;
          }
        });

        gridStructure.value[currentRow][currentCol].value = holeCount;
      }
    }
  }
};

const getAdjacentCells = (currentRow, currentCol) => {
  const results = [];

  for (
    let rowPos = currentRow > 0 ? -1 : 0;
    rowPos <= (currentRow < rows.value - 1 ? 1 : 0);
    rowPos++
  ) {
    for (
      let colPos = currentCol > 0 ? -1 : 0;
      colPos <= (currentCol < cols.value - 1 ? 1 : 0);
      colPos++
    ) {
      results.push(gridStructure.value[currentRow + rowPos][currentCol + colPos]);
    }
  }

  return results;
};

const handleClick = (cell) => {
  console.log('handleClick', cell);
};

onMounted(() => {
  fillGridStructure();
  assignHoles();
});
</script>

<template>
  <header>MineSweeper</header>

  <main>
    <code>
      {{ gridStructure }}
    </code>

    <v-row
      v-for="(row, i) in gridStructure"
      :key="i"
      no-gutters
    >
      <v-col v-for="(cell, index) in row" :key="index">
        <v-sheet
          class="d-flex align-center justify-center pa-2 ma-2"
          border
          @click="handleClick(cell)"
        >
          {{ cell.value }}
        </v-sheet>
      </v-col>
    </v-row>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
