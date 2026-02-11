<script setup lang="ts">
import { ref } from "vue";

const isCameraActive = ref(false);
const videoElement = ref<HTMLVideoElement | null>(null);
const stream = ref<MediaStream | null>(null);
const photoData = ref<string | null>(null);
const errorMessage = ref<string | null>(null);

async function startCamera() {
  try {
    errorMessage.value = null;

    const mediaStream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: "user" },
      audio: false,
    });

    stream.value = mediaStream;

    if (videoElement.value) {
      videoElement.value.srcObject = mediaStream;

      // ⭐ ตัวนี้แหละที่ทำให้ภาพขึ้น
      await videoElement.value.play();
    }

    isCameraActive.value = true;
  } catch (error) {
    errorMessage.value = "ไม่สามารถเข้าถึงกล้องได้ กรุณาอนุญาตการใช้งาน";
    console.error("Error accessing camera:", error);
  }
}

function stopCamera() {
  if (stream.value) {
    stream.value.getTracks().forEach((track) => track.stop());
    stream.value = null;
  }
  isCameraActive.value = false;
  photoData.value = null;
}

function takePhoto() {
  if (videoElement.value) {
    const canvas = document.createElement("canvas");
    canvas.width = videoElement.value.videoWidth;
    canvas.height = videoElement.value.videoHeight;
    const ctx = canvas.getContext("2d");
    if (ctx) {
      ctx.drawImage(videoElement.value, 0, 0);
      photoData.value = canvas.toDataURL("image/png");
    }
  }
}

function downloadPhoto() {
  if (photoData.value) {
    const link = document.createElement("a");
    link.href = photoData.value;
    link.download = `photo-${Date.now()}.png`;
    link.click();
  }
}
</script>

<template>
  <v-container class="camera-container">
    <v-card class="mt-8">
      <v-card-title>เข้าถึงกล้อง</v-card-title>

      <v-card-text>
        <!-- Camera Preview -->
        <div v-if="isCameraActive" class="camera-preview mb-4">
          <video
            ref="videoElement"
            autoplay
            playsinline
            muted
            class="camera-video"
          ></video>
        </div>

        <!-- Photo Display -->
        <div v-if="photoData" class="photo-preview mb-4">
          <img :src="photoData" class="captured-photo" />
        </div>

        <!-- Error Message -->
        <v-alert v-if="errorMessage" type="error" class="mb-4">
          {{ errorMessage }}
        </v-alert>

        <!-- Camera Status -->
        <div v-if="isCameraActive" class="status-text mb-4">
          <v-chip color="green" text-color="white">
            <v-icon start>mdi-camera</v-icon>
            กล้องทำงานอยู่
          </v-chip>
        </div>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>

        <!-- Start Camera Button -->
        <v-btn
          v-if="!isCameraActive"
          color="primary"
          prepend-icon="mdi-camera"
          @click="startCamera"
        >
          เปิดกล้อง
        </v-btn>

        <!-- Take Photo Button -->
        <v-btn
          v-if="isCameraActive"
          color="success"
          prepend-icon="mdi-camera-plus"
          @click="takePhoto"
        >
          ถ่ายรูป
        </v-btn>

        <!-- Download Photo Button -->
        <v-btn
          v-if="photoData"
          color="info"
          prepend-icon="mdi-download"
          @click="downloadPhoto"
        >
          ดาวน์โหลด
        </v-btn>

        <!-- Stop Camera Button -->
        <v-btn
          v-if="isCameraActive"
          color="error"
          prepend-icon="mdi-camera-off"
          @click="stopCamera"
        >
          ปิดกล้อง
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<style scoped>
.camera-container {
  max-width: 600px;
  margin: 0 auto;
}

.camera-preview {
  width: 100%;
  border-radius: 12px;
  overflow: hidden;
  background-color: #000;
}

.camera-video {
  width: 100%;
  height: auto;
  display: block;
}

.photo-preview {
  width: 100%;
  border-radius: 12px;
  overflow: hidden;
}

.captured-photo {
  width: 100%;
  height: auto;
  display: block;
  border-radius: 8px;
}

.status-text {
  margin: 16px 0;
}
</style>
