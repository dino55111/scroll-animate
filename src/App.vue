<script setup>
import { onMounted, ref, computed } from 'vue'
const list = [
  { word: "DON'T", color: '#9D7553' },
  { word: 'WAIT', color: '#A98B73' },
  { word: 'FOR', color: '#DABEA7' },
  { word: 'OPPORTUNITY,', color: '#876D5A' },
  { word: 'CREATE', color: '#CDA581' },
  { word: 'IT.', color: '#9A6852' }
]

const showList = computed(() => {
  return [list.slice(-1)[0], ...list, list[0]]
})

const scrollLock = ref(false)
const now = ref(1)

const bgStyleObj = computed(() => {
  return {
    transform: `translateY(-${now.value * 100}dvh)`,
    transitionDuration: scrollLock.value ? '1.2s' : '0s'
  }
})

const wordStyleObj = computed(() => {
  return {
    transform: `translateY(-${now.value * 212}px)`,
    transitionDuration: scrollLock.value ? '0.9s' : '0s',
    transitionDelay: scrollLock.value ? '0.3s' : '0s'
  }
})

const scrollHandler = (e) => {
  if (scrollLock.value) return
  scrollLock.value = true
  if (e.deltaY > 0) {
    now.value += 1
  } else {
    now.value -= 1
  }
}

const transitionEndHandler = () => {
  scrollLock.value = false
  if (now.value === 0) {
    now.value = list.length
  } else if (now.value === list.length + 1) {
    now.value = 1
  }
}

onMounted(() => {
  window.addEventListener('mousewheel', scrollHandler)
})
</script>

<template>
  <div class="container">
    <div
      class="bgBox"
      :style="bgStyleObj"
      @transitionend="transitionEndHandler"
    >
      <div
      v-for="item in showList"
      :key="item.word" class="bg" :style="{ backgroundColor: item.color }"></div>
    </div>

      <div class="wordBox">
        <div :style="wordStyleObj">
          <h1 v-for="item in showList" :key="item.word" class="word">{{ item.word }}</h1>
        </div>
      </div>

    <div class="scrollHint">SCROLL DOWN</div>
  </div>
</template>

<style>
.container {
  width: 100%;
  height: 100dvh;
  overflow: hidden;
}

.bgBox {
  width: 100%;
  height: 100dvh;
}

.bg {
  width: 100%;
  height: 100dvh;
}

.wordBox {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  height: 212px;
  overflow: hidden;
  text-align: center;
}

.word {
  padding: 10px 20px;
  font-size: 120px;
  font-weight: 900;
  color: #000;
}

.scrollHint {
  font-size: 20px;
  position: fixed;
  bottom: 120px;
  right: 10px;
  transform: rotate(90deg);
  padding-bottom: 5px;
  padding-right: 30px;
  border-bottom: 1px #ddd solid;
  animation: shake 1.5s infinite;
}

.scrollHint::after {
  content: '';
  display: block;
  position: absolute;
  right: 5px;
  bottom: -1px;
  height: 20px;
  width: 1px;
  background-color: #ddd;
  transform: rotate(-30deg);
}

@keyframes shake {
  0% {
    transform: rotate(90deg) translateX(0);
  }
  50% {
    transform: rotate(90deg) translateX(10px);
  }
  100% {
    transform: rotate(90deg) translateX(0);
  }
}
</style>
