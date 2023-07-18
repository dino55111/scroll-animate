<script setup>
const list = [
  {word: '1', color: '#9D7553'},
  {word: '2', color: '#A98B73'},
  {word: '3', color: '#DABEA7'},
  {word: '4', color: '#876D5A'},
  {word: '5', color: '#CDA581'},
  {word: '6', color: '#9A6852'}
]
</script>

<template>
  <div class="container">
    <div v-for="item in list" :key="item.word" class="box" :style="{backgroundColor: item.color}">
      <h1 class="word">{{ item.word }}</h1>
    </div>
  </div>
</template>

<style>
.container {
  width:100%;
  height: 100dvh;
  /* overflow: hidden; */
}

.box {
  width:100%;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100dvh;
}

.word {
  font-size: 40px;
  font-weight: bold;
  color: #fff;
  text-shadow: 0 0 10px #000;
  animation: animate 10s linear infinite;
}
</style>
