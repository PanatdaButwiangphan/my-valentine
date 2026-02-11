<script setup lang="ts">
import WhoruImg from "@/assets/img/whoru.jpg";
import notOut from "@/assets/img/notout.png";
import { characterData } from "@/data/character";
const Mapping = ref(characterData);
const showLabelNO = ref(false);
const isActive = ref(false);
const  props = defineProps({label: String})
async function notLogOut() {
  isActive.value = true;
  showLabelNO.value = true;
}
</script>
<template>
  <v-row>
    <v-col cols="12" md="4" class="d-flex justify-start align-center">
      <img class="circular-image mt-2" :src="WhoruImg" />
    </v-col>
   <v-col
  cols="12"
  md="4"
  class="d-flex justify-center align-center"
>
  <v-chip
    class="ma-2"
    color="pink"
    label
  >
    <v-icon icon="mdi-flower" start></v-icon>
    {{ props.label }}
  </v-chip>
</v-col>
    <v-col cols="12" md="4" class="d-flex justify-end align-center">
      <v-btn icon="mdi-power" color="error" @click="notLogOut"></v-btn>
    </v-col>
  </v-row>
  <v-divider></v-divider>
  <v-dialog max-width="500" v-model="isActive" >
    <template v-slot:default>
      <v-card title="ออกไป!">
        <v-card-text>
          <v-row>
            <v-col cols="12" md="4">
              <img :src="notOut" />
            </v-col>
            <v-col cols="12" md="6" class="d-flex flex-column justify-center align-center">
              <h4 class="text-center mt-2" >ถ้ากดออกเค้าจะกลับบ้าน</h4>
            </v-col>
          </v-row>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="error" text="ออกอยู่ดี" @click="isActive = false"></v-btn>

          <v-btn color="primary" text="ไม่ออกค้าบ" @click="isActive = false"></v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>

<style scoped>
.circular-image {
  border-radius: 50%;
  width: 50px;
  height: 50px;
  object-fit: cover;
}
</style>
