<template>
  <div class="row" v-for="(row, index) in list" :key="index">
    <div
      class="cell"
      :class="{
        'cell-empty': cell == 0,
        'cell-wall-vertically': cell == 1,
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
let dy = -1 // направление по y
let dx = -1 // направление по x
let nx = 5 //now x
let ny = 8 //now y

window.addEventListener('keydown', (event) => {
  if (event.code == 'ArrowRight') {
    palitra(list)
  }
  if (event.code == 'ArrowLeft') {
    not_palitra(list)
  }
})

import { reactive } from 'vue'
import { gen_gameboard } from './other.vue'
const list = reactive([gen_gameboard()])
function palitra(list) {
  let last_list = list[list.length - 1]
  const lastElement = last_list.pop() // Удаляем последний элемент и сохраняем его

  last_list.unshift(lastElement) // Помещаем удаленный элемент в начало массива
}
function not_palitra(list) {
  let last_list = list[list.length - 1]
  const firstElement = last_list.shift() // Удаляем первый элемент и сохраняем его

  last_list.push(firstElement)
}
setInterval(() => {
  if (list[ny + dy][nx + dx] == 1) {
    // проверяем будет ли стенка на следующем шаге, а также проверяем есть ли стенка справа или слева, для того чтобы понять это стенка вертикальна или нет
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
}, 250)
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
</style>
