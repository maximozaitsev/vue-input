<script setup>
import { ref } from "vue";
import BaseInput from "./components/BaseInput.vue";
import arrowIcon from "@/assets/chevron.svg";
import deleteIcon from "@/assets/trash.svg";

const input1 = ref("");
const input2 = ref("");
const input3 = ref("");
</script>

<template>
  <div class="container">
    <BaseInput v-model="input1" placeholder="" />
    <BaseInput
      v-model="input2"
      :icon="arrowIcon"
      iconBehavior="disable"
      placeholder=""
    />
    <BaseInput
      v-model="input3"
      :icon="deleteIcon"
      iconBehavior="clear"
      placeholder=""
    />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  gap: 24px;
  justify-content: center;
  align-items: center;
  height: 96vh;
  font-family: sans-serif;
}
</style>
