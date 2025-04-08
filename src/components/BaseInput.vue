<template>
  <div class="input-wrapper">
    <input
      type="text"
      class="input-field"
      v-model="localValue"
      :placeholder="placeholder"
    />
    <img v-if="icon" :src="icon" alt="icon" class="input-icon" />
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  modelValue: {
    type: String,
    default: "",
  },
  icon: {
    type: String,
    default: "",
  },
  placeholder: {
    type: String,
    default: "",
  },
});

const emit = defineEmits(["update:modelValue"]);
const localValue = ref(props.modelValue);

watch(localValue, (val) => {
  emit("update:modelValue", val);
});

watch(
  () => props.modelValue,
  (val) => {
    localValue.value = val;
  }
);
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Golos+Text&display=swap");

.input-wrapper {
  position: relative;
  width: 100%;
  max-width: 200px;
}

.input-field {
  width: 100%;
  padding: 25px;
  border: 1px solid #b9b9b9;
  border-radius: 20px;
  box-sizing: border-box;

  font-family: "Golos Text", sans-serif;
  font-weight: 400;
  font-size: 20px;
  line-height: 130%;
  letter-spacing: 0;
}

.input-icon {
  position: absolute;
  top: 50%;
  right: 25px;
  transform: translateY(-50%);
  width: 24px;
  height: 24px;
}
</style>
