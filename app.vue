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

  const canvas = ref(null);
  

  onMounted(() => {
    const ctx = canvas.value.getContext('2d');

    // Calculate size of canvas from constants.
    ctx.canvas.width = COLS * BLOCK_SIZE;
    ctx.canvas.height = ROWS * BLOCK_SIZE;

    // Scale blocks
    ctx.scale(BLOCK_SIZE, BLOCK_SIZE);
  })

  const grid = ref();
  // const reset = () => {
  //   grid.value = getEmptyBoard();
  // }

  // const getEmptyBoard = () => {
  //   return Array.from(
  //     {length: ROWS}, () => Array(COLS).fill(0)
  //   );
  // }

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

  const board = new Board();
  const play = () => {
    board.reset();  
    console.table(board.grid);  
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
