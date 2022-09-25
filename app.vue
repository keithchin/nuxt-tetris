<script setup>
  import { ref } from 'vue'
  useHead({
    link: {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css?family=Press+Start+2P'
    }
  })


  const COLS = 10;
  const ROWS = 20;
  const BLOCK_SIZE = 30;
  const COLORS = [
  'none',
  'cyan',
  'blue',
  'orange',
  'yellow',
  'green',
  'purple',
  'red'
];

const SHAPES = [
  [],
  [[0, 0, 0, 0], [1, 1, 1, 1], [0, 0, 0, 0], [0, 0, 0, 0]],
  [[2, 0, 0], [2, 2, 2], [0, 0, 0]],
  [[0, 0, 3], // 0,0 -> 2,0 ; 0,1 -> 1,0 ; 0,2 -> 0,0
   [3, 3, 3], // 1,0 -> 2,1 ; 1,1 -> 1,1 ; 1,2 -> 0,1 
   [0, 0, 0]],// 2,0 -> 2,2 ; 2,1 -> 1,2 ; 2,2 -> 0,2
  [[4, 4], [4, 4]],
  [[0, 5, 5], [5, 5, 0], [0, 0, 0]],
  [[0, 6, 0], [6, 6, 6], [0, 0, 0]],
  [[7, 7, 0], [0, 7, 7], [0, 0, 0]]
];

const KEY = {
  ESC: 27,
  SPACE: 32,
  LEFT: 37,
  UP: 38,
  RIGHT: 39,
  DOWN: 40,
  P: 80,
  Q: 81
};

const POINTS = {
  SINGLE: 100,
  DOUBLE: 300,
  TRIPLE: 500,
  TETRIS: 800,
  SOFT_DROP: 1,
  HARD_DROP: 2,
};

const LEVEL = {
  0: 800,
  1: 720,
  2: 630,
  3: 550,
  4: 470,
  5: 380,
  6: 300,
  7: 220,
  8: 130,
  9: 100,
  10: 80,
  11: 80,
  12: 80,
  13: 70,
  14: 70,
  15: 70,
  16: 50,
  17: 50,
  18: 50,
  19: 30,
  20: 30,
  // 29+ is 20ms
};

const ROTATION = {
  LEFT: 'left',
  RIGHT: 'right'
};

  const canvas = ref(null);
  const ctx = ref(null);
  
  const board = ref(null);

  onMounted(() => {
    ctx.value = canvas.value.getContext('2d');

    // Calculate size of canvas from constants.
    ctx.value.canvas.width = COLS * BLOCK_SIZE;
    ctx.value.canvas.height = ROWS * BLOCK_SIZE;

    // Scale blocks
    ctx.value.scale(BLOCK_SIZE, BLOCK_SIZE);

      const grid = ref();

    class Board {
      reset = () => {
        this.grid = this.getEmptyBoard();
      }

      getEmptyBoard = () => {
        return Array.from(
          {length: ROWS}, () => Array(COLS).fill(0)
        );
      }
    }

    board.value = new Board();
  })
  class Piece {
      constructor(ctx) {
        this.ctx = ctx;
        this.spawn();
      }

      spawn() {
        this.typeId = this.randomizeTetrominoType(COLORS.length - 1);
        this.shape = SHAPES[this.typeId];
        this.color = COLORS[this.typeId];
        this.x = 0;
        this.y = 0;
        this.hardDropped = false;
      }

      draw() {
        this.ctx.fillStyle = this.color;
        this.shape.forEach((row, y) => {
          row.forEach((value, x) => {
            if (value > 0) {
              this.ctx.fillRect(this.x + x, this.y + y, 1, 1);
            }
          });
        });
      }

      move(p) {
        if (!this.hardDropped) {
          this.x = p.x;
          this.y = p.y;
        }
        this.shape = p.shape;
      }

      hardDrop() {
        this.hardDropped = true;
      }

      setStartingPosition() {
        this.x = this.typeId === 4 ? 4 : 3;
      }

      randomizeTetrominoType(noOfTypes) {
        return Math.floor(Math.random() * noOfTypes + 1);
      }
    }
  const play = () => {
    board.value.reset();  
    let piece = new Piece(ctx.value);
    piece.draw();
    
    board.piece = piece;
  }

</script>
<template>
  <div>
      <div class="grid grid-cols-board">
        <canvas ref="canvas" id="canvas" class="border-solid border-2"></canvas>
        <div id="test" class="flex flex-col justify-between">
            <RightColumn></RightColumn>
            <button @click="play()" class="fs-16 px-15 py-30 cursor-pointer">Play</button>
        </div>
      </div>
  </div>
</template>
