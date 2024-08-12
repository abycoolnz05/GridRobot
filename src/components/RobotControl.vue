<template>
  <div class="robot-simulator">
    <div class="grid">
      <div
        v-for="(cell, index) in grid"
        :key="index"
        :class="['cell', { 'robot-cell': index === robotPosition }]"
      >
        <div v-if="index === robotPosition" :class="['robot', directionClass]">🤖</div>
      </div>
    </div>
    <div class="controls">
      <button @click="rotateLeft">Rotate Left</button>
      <button @click="rotateRight">Rotate Right</button>
      <button @click="moveForward">Move Forward >></button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      size: 5, // Grid size (5x5)
      robotPosition: 12, // Initial position (index of the grid array)
      direction: 'N', // Initial direction (N: North, E: East, S: South, W: West)
      directions: ['N', 'E', 'S', 'W'],
    };
  },
  computed: {
    grid() {
      return Array(this.size * this.size).fill(null);
    },
    directionClass() {
      return {
        'direction-north': this.direction === 'N',
        'direction-east': this.direction === 'E',
        'direction-south': this.direction === 'S',
        'direction-west': this.direction === 'W',
      };
    },
  },
  methods: {
    rotateLeft() {
      const index = this.directions.indexOf(this.direction);
      this.direction = this.directions[(index + 3) % 4];
    },
    rotateRight() {
      const index = this.directions.indexOf(this.direction);
      this.direction = this.directions[(index + 1) % 4];
    },
    moveForward() {
      let newPosition = this.robotPosition;
      switch (this.direction) {
        case 'N':
          newPosition -= this.size;
          break;
        case 'E':
          newPosition += 1;
          break;
        case 'S':
          newPosition += this.size;
          break;
        case 'W':
          newPosition -= 1;
          break;
      }
      if (newPosition >= 0 && newPosition < this.size * this.size) {
        this.robotPosition = newPosition;
      }
    },
  },
};
</script>

<style scoped>
.robot-simulator {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: Arial, sans-serif;
}
.grid {
  display: grid;
  grid-template-columns: repeat(5, 50px);
  grid-template-rows: repeat(5, 50px);
  gap: 1px;
  margin-bottom: 20px;
}
.cell {
  width: 50px;
  height: 50px;
  background-color: #eee;
  border: 1px solid #000;
  position: relative;
}
.cell.robot-cell {
  background-color: #ddd;
}
.robot {
  width: 20px;
  height: 20px;
  background-color: #ddd;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.direction-north {
  transform: translate(-50%, -50%) rotate(0deg);
}
.direction-east {
  transform: translate(-50%, -50%) rotate(90deg);
}
.direction-south {
  transform: translate(-50%, -50%) rotate(180deg);
}
.direction-west {
  transform: translate(-50%, -50%) rotate(270deg);
}
.controls {
  display: flex;
  gap: 10px;
}
</style>