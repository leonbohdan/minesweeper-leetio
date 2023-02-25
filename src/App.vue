<script setup>
import { ref, onMounted } from 'vue';

const gridStructure = ref([]);

const cellModel = ref({
  value: 0,
  isHole: false,
  id: null,
});

const rows = ref(5);
const cols = ref(5);
const holes = ref(3);
const holesCoordination = ref([]);

const fillGridStructure = () => {
  for (let r = 0; r < rows.value; r++) {
    gridStructure.value[r] = [];

    for (let c = 0; c < cols.value; c++) {
      gridStructure.value[r].push({
        ...cellModel.value,
        id: `${r}-${c}`,
      });
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
        ...randomCell,
        isHole: true,
        value: 'H',
      };

      holesCoordination.value.push(randomCell.id);

      assignedHoles++;
    }
  }

  handleAdjacentCells();
};

const handleAdjacentCells = () => {
  for (let rowCoordinate = 0; rowCoordinate < rows.value; rowCoordinate++) {
    for (let colCoordinate = 0; colCoordinate < cols.value; colCoordinate++) {
      if (!gridStructure.value[rowCoordinate][colCoordinate].isHole) {
        const countOfAdjacentHols = getAdjacentHoles(rowCoordinate, colCoordinate);

        gridStructure.value[rowCoordinate][colCoordinate].value = countOfAdjacentHols;
      }
    }
  }
};

const getAdjacentHoles = (rowCoordinate, colCoordinate) => {
  if (rowCoordinate < 0 || rowCoordinate >= rows.value || colCoordinate < 0 || colCoordinate >= cols.value) return;

  let foundedHoles = 0;

  foundedHoles += checkCell(rowCoordinate - 1, colCoordinate - 1);
  foundedHoles += checkCell(rowCoordinate - 1, colCoordinate);
  foundedHoles += checkCell(rowCoordinate - 1, colCoordinate + 1);

  foundedHoles += checkCell(rowCoordinate, colCoordinate - 1);
  foundedHoles += checkCell(rowCoordinate, colCoordinate + 1);

  foundedHoles += checkCell(rowCoordinate + 1, colCoordinate - 1);
  foundedHoles += checkCell(rowCoordinate + 1, colCoordinate);
  foundedHoles += checkCell(rowCoordinate + 1, colCoordinate + 1);

  return foundedHoles;
};

const checkCell = (rowCoordinate, colCoordinate) => {
  if (rowCoordinate < 0 || rowCoordinate >= rows.value || colCoordinate < 0 || colCoordinate >= cols.value) {
    return 0;
  }

  return holesCoordination.value.includes(`${rowCoordinate}-${colCoordinate}`) ? 1 : 0;
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
    <v-row
      v-for="(row, i) in gridStructure"
      :key="i"
      no-gutters
      dense
    >
      <v-col v-for="cell in row" :key="cell.id">
        <v-sheet
          class="d-flex align-center justify-center pa-2 ma-0 font-weight-bold"
          border
          min-width="90"
          min-height="50"
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
