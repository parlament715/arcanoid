<template>
  <div class="row" v-for="(row, index) in list" :key="index">
    <div
      class="cell"
      :class="{
        'cell-empty': cell == 0,
        'cell-wall-vertically': cell == 1,
        'cell-target': cell == 2,
        'cell-wall-angel': cell == 5,
        'cell-wall-horizontally': cell == 4,
        'cell-ball': cell == 3
      }"
      v-for="(cell, index) in row"
      :key="index"
    ></div>
  </div>
</template>
<script setup>
import { reactive } from 'vue'
import { gen_gameboard, palitra, not_palitra, game_over, vector_x } from '../func.js'

let dy = -1 // направление по y
let dx = vector_x() // направление по x
let nx = 22 //now x
let ny = 16 //now y

window.addEventListener('keydown', (event) => {
  // движение на клик стрелок
  if (event.code == 'ArrowRight') {
    palitra(list)
  }
  if (event.code == 'ArrowLeft') {
    not_palitra(list)
  }
})

const list = reactive(gen_gameboard())

setInterval(() => {
  game_over(list[list.length - 1])
  // движение шарика
  if (list[ny + dy][nx + dx] == 1) {
    dx = -1 * dx
  }
  if (list[ny + dy][nx + dx] == 4) {
    dy = -1 * dy
  }
  if (list[ny + dy][nx + dx] == 5) {
    dx = -1 * dx
    dy = -1 * dy
  }
  if (list[ny + dy][nx + dx] == 0) {
    list[ny][nx] = 0
    list[ny + dy][nx + dx] = 3
    nx += dx
    ny += dy
  }
  if (list[ny + dy][nx + dx] == 2) {
    list[ny + dy][nx + dx] = 0
    dy = -1 * dy
  }
}, 150)
</script>
<style scoped>
.cell {
  margin: 2px;
  display: inline-block;
  width: 30px;
  height: 30px;
  border: 1px solid black;
}
.cell-empty {
  background-color: rgb(255, 255, 255);
}
.cell-wall-vertically {
  background-color: rgb(206, 35, 5);
}
.cell-wall-angel {
  background-color: rgb(206, 35, 5);
}
.cell-wall-horizontally {
  background-color: rgb(206, 35, 5);
}
.cell-ball {
  background-color: rgb(41, 226, 41);
}
.cell-target {
  background-color: rgb(225, 255, 56);
}
</style>
