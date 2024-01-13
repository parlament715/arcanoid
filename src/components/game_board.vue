<template>
  <div class="row" v-for="(row, index) in list" :key="index">
    <div
      class="cell"
      :class="{ 'cell-empty': cell == 0, 'cell-wall': cell == 1, 'cell-ball': cell == 3 }"
      v-for="(cell, index) in row"
      :key="index"
    ></div>
  </div>
</template>
<script setup>
let dy = -1
let dx = -1
let nx = 5
let ny = 8

window.addEventListener('keydown', (event) => {
  if (event.code == 'ArrowRight') {
    palitra(list)
  }
  if (event.code == 'ArrowLeft') {
    not_palitra(list)
  }
})

import { reactive } from 'vue'
const list = reactive([
  [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
  [1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 1],
  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
  [1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1],
  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
  [1, 0, 0, 0, 0, 3, 0, 0, 0, 0, 1],
  [0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0]
])
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
  if (list[ny][nx + dx] == 2) {
    dx = -1 * dx
    console.log(dx)
  }
  if (list[ny + dy][nx] == 2) {
    dy = -1 * dy
    console.log(dy)
  }
  if (list[ny + dy][nx + dx] == 0) {
    list[ny][nx] = 0
    list[ny + dy][nx + dx] = 3
    nx += dx
    ny += dy
  }
}, 1000)
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
.cell-wall {
  background-color: rgb(206, 35, 5);
}
.cell-ball {
  background-color: rgb(41, 226, 41);
}
</style>
