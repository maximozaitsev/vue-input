<script setup>
import { ref } from "vue";
import Input from "./components/BaseInput.vue";
import arrowIcon from "@/assets/chevron.svg";
import deleteIcon from "@/assets/trash.svg";

const input1 = ref("");
const input2 = ref("");
</script>

<template>
  <div class="container">
    <Input v-model="input1" placeholder="" />
    <Input v-model="input2" :icon="arrowIcon" placeholder="" />
    <Input v-model="input3" :icon="deleteIcon" placeholder="" />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  gap: 20px;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
