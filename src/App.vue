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
};

onMounted(() => {
  fillGridStructure();
  assignHoles();
});
</script>

<template>
  <header>MineSweeper</header>

  <main>
    {{ gridStructure }}
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
