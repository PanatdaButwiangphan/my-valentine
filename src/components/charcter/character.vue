<script setup lang="ts">
import Navbar from "../component/navbar.vue";
import { characterData } from "@/data/character";
import douluvme from "@/assets/img/douluvme.jpg";
import why from "@/assets/img/hh.jpg";
import kill from "@/assets/img/kill.jpg";
import giveu from "@/assets/img/giveu.png";
import {onMounted} from 'vue'
const Mapping: Ref<any> = ref<any>(characterData);
const showFirework = ref(false)
const count = ref(1);
const imgloop = ref(douluvme);
async function noBtn() {
  count.value++;
  // if(count.value == 1){
  // imgloop.value = douluvme
  // } else
  if (count.value == 2) {
    imgloop.value = why;
  } else {
    imgloop.value = kill;
  }
}

async function yesBtn() {
  imgloop.value = giveu;
   showFirework.value = true

  // ให้พลุหายไปเอง
  setTimeout(() => {
    showFirework.value = false
  }, 2500)
}

function randomStyle() {
  const x = (Math.random() - 0.5) * 900   // กระจายไกลขึ้น
  const y = (Math.random() - 0.5) * 900
  const delay = Math.random() * 0.4
  const size = Math.random() * 10 + 10   // 🔥 ใหญ่ขึ้น (10–20px)

  return {
    '--x': `${x}px`,
    '--y': `${y}px`,
    width: `${size}px`,
    height: `${size}px`,
    animationDelay: `${delay}s`
  }
}


</script>
<template>
  <Navbar :label="Mapping.today" />
  <div class="d-flex justify-center align-center mt-5">
    <img :src="imgloop" style="width: 200px" />
  </div>
  <h3 class="text-center mt-2">
    {{ imgloop == giveu ? Mapping.flower : Mapping.dou }}
  </h3>
  <h2
    :class="['text-center', count === 2 ? 'no1' : '', count >= 3 ? 'no2' : '']"
    v-if="imgloop != giveu"
  >
    {{ Mapping.luv }}
  </h2>
  <div class="d-flex justify-center align-center mt-5">
    <v-btn
      class="mr-5"
      v-if="imgloop != giveu"
      style="background-color: #ff52a0; color: white"
      @click="yesBtn"
      >{{ Mapping.yes }}</v-btn
    >
    <v-btn
      v-if="count < 4 && imgloop != giveu"
      style="background-color: #fff7cd"
      @click="noBtn"
      >{{ Mapping.no }}</v-btn
    >
  </div>

  <div v-if="showFirework" class="fireworks">
  <span
    v-for="n in 40"
    :key="n"
    class="spark"
    :style="randomStyle()"
  ></span>
</div>
</template>

<style scoped>
.no1 {
  font-size: 50px;
  text-align: center;
}

.no2 {
  font-size: 100px;
  text-align: center;
}

img {
  transition: all 0.3s ease;
}
.fireworks {
  pointer-events: none;
  position: fixed;
  inset: 0;
  overflow: hidden;
  z-index: 9999;
}

.spark {
  position: absolute;
  top: 50%;
  left: 50%;
  border-radius: 50%;
  background: radial-gradient(circle, #fff, #ff4da6, #ffd700);
  animation: explode 1.2s ease-out forwards;
}

.spark:nth-child(odd) { background: #ff4da6; }
.spark:nth-child(even) { background: #ffd700; }

@keyframes explode {
  from {
    transform: translate(0, 0) scale(1);
    opacity: 1;
  }
  to {
    transform: translate(var(--x), var(--y)) scale(0.3);
    opacity: 0;
  }
}

</style>
