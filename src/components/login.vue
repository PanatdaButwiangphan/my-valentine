<script setup lang="ts">
import WhoruImg from "@/assets/img/whoru.jpg";
import canupressme from "@/assets/img/canupressme.png";
import pressme2 from "@/assets/img/pressme2.png";
import pressme3 from "@/assets/img/presssme3.png";
import pressme4 from "@/assets/img/pressme4.png";
import pressmeLast from "@/assets/img/pressmelast.png";
import { LoginData } from "@/data/login";
import router from "@/router";
const Mapping: Ref<any> = ref<any>(LoginData);
const count = ref(0);

async function press() {
  count.value++;
  if (count.value == 6) {
      router.push('/character')
  }
}
</script>
<template>
  <div class="image-container">
    <img class="circular-image" :src="WhoruImg" />
  </div>
  <div class="d-flex align-center justify-center pa-4">
    <v-col cols="12" md="6">
      <v-text-field
        v-model="Mapping.userName"
        hide-details="auto"
        color="error"
        :label="Mapping.field"
      />
    </v-col>
  </div>
  <v-col cols="auto" class="d-flex align-center justify-center pa-4">
    <v-btn
      :disabled="!Mapping.userName"
      prepend-icon="mdi-heart"
      color="pink"
      outlined
      size="x-large"
      @click="press"
      >{{ Mapping.pressMe }}</v-btn
    >
  </v-col>
  <div class="image-container_1 mt-4" v-if="count == 1">
    <img class="img_1" :src="canupressme" />
    <p class="ml-5" style="font-size: 20px">{{ Mapping.more }}</p>
  </div>
  <div class="image-container_1 mt-4" v-if="count == 2">
    <img class="img_1" :src="pressme2" />
    <p class="ml-5" style="font-size: 50px">{{ Mapping.more }}</p>
  </div>
  <div class="image-container_1 mt-4" v-if="count == 3">
    <img class="img_1" :src="pressme3" />
    <p class="ml-5" style="font-size: 100px">{{ Mapping.more }}</p>
  </div>
  \
  <div class="image-container_1 mt-4" v-if="count == 4">
    <img class="img_1" :src="pressme4" />
    <p class="ml-5" style="font-size: 150px">{{ Mapping.more }}</p>
  </div>
  <div class="image-container_1" v-if="count == 5">
    <img class="img_1" :src="pressmeLast" />
    <p class="ml-5" style="font-size: 200px">{{ Mapping.more }}</p>
  </div>
</template>

<style scoped>
.image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 30vh;
}
.image-container_1 {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 30vh;
}
.circular-image {
  border-radius: 50%;
  width: 200px;
  height: 200px;
  object-fit: cover;
}
.img_1 {
  /* border-radius: 50%; */
  width: 200px;
  height: 200px;
  object-fit: cover;
}
.count-text {
  color: #e91e63;
  font-weight: bold;
  margin-top: 20px;
}
</style>
