<template>
  <div class="container">
    <div class="item item-1">item-1</div>
    <div class="item item-2">
      <p class="sub-item">grid sub item</p>
    </div>
    <div class="item item-3">Item-3</div>
    <div class="item item-4">item -4</div>
    <div class="item item-5">Item-5</div>
    <div class="item item-6">Item-6</div>
  </div>
</template>
<script></script>
<style>
body {
  width: 100vw;
  height: 100vh;
  background-color: darkcyan;
  color: #000;
  font-size: 20px;
}
.container {
  display: grid;
  grid-template-columns: repeat(3, 200px);
  grid-template-rows: repeat(3, 200px);
  justify-content: start;
}
.item {
  display: grid;
  border: 2px solid #fff;
  padding: 20px;
  margin: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 10px 5px 5px #000;
}
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
}
.item-2 {
  grid-column: 1 / 3;
}
.item-3 {
  grid-column-start: 1;
  grid-column-end: 3;
}
.item-4 {
  grid-row: 1 /4;
}
</style>
