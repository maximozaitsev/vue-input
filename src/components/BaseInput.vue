<template>
  <div class="wrapper" :class="{ error: hasError }">
    <div class="input-and-icon" :class="{ disabled: isDisabled }">
      <input
        type="text"
        class="input-field"
        v-model="localValue"
        :disabled="isDisabled"
        :placeholder="placeholder"
        :style="{ width: inputWidth }"
        @focus="onFocus"
        @blur="onBlur"
      />
      <img
        v-if="icon"
        :src="icon"
        alt="icon"
        class="input-icon"
        @click="onIconClick"
      />
    </div>
    <p v-if="hasError" class="error-text">обязательное поле*</p>
    <span ref="sizer" class="sizer"></span>
  </div>
</template>

<script setup>
import { ref, watch, nextTick, onMounted } from "vue";

const props = defineProps({
  modelValue: { type: String, default: "" },
  icon: { type: String, default: "" },
  placeholder: { type: String, default: "" },
  iconBehavior: { type: String, default: "" },
});
const emit = defineEmits(["update:modelValue"]);

const localValue = ref(props.modelValue);
const isDisabled = ref(false);
const isFocused = ref(false);
const hasError = ref(false);
const wasEdited = ref(false);
const inputWidth = ref("0px");
const sizer = ref(null);
const MIN_WIDTH = 5;
const MAX_WIDTH = 42;

function updateWidth() {
  nextTick(() => {
    if (hasError.value) {
      inputWidth.value = "149px";
      return;
    }
    const textToMeasure = localValue.value.length > 0 ? localValue.value : "";
    if (sizer.value) {
      sizer.value.textContent = textToMeasure;
      const measured = sizer.value.offsetWidth;
      const newWidth =
        textToMeasure === ""
          ? isFocused.value
            ? MIN_WIDTH
            : 0
          : Math.min(measured, MAX_WIDTH);
      inputWidth.value = newWidth + "px";
    }
  });
}

watch(localValue, (newVal) => {
  emit("update:modelValue", newVal);
  if (!wasEdited.value && newVal.length > 0) wasEdited.value = true;
  hasError.value = wasEdited.value && newVal.length === 0;
  updateWidth();
});
watch(
  () => props.modelValue,
  (newVal) => {
    localValue.value = newVal;
    updateWidth();
  }
);

function onFocus() {
  isFocused.value = true;
  updateWidth();
}
function onBlur() {
  isFocused.value = false;
  updateWidth();
}

function onIconClick() {
  if (props.iconBehavior === "disable") {
    isDisabled.value = !isDisabled.value;
  } else if (props.iconBehavior === "clear") {
    localValue.value = "";
  }
}

onMounted(updateWidth);
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Golos+Text:wght@400..900&display=swap");

.wrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.wrapper.error .input-and-icon {
  border-color: #f44336;
}
.input-and-icon {
  display: inline-flex;
  align-items: center;
  border: 1px solid #b9b9b9;
  border-radius: 20px;
  padding: 24px;
  box-sizing: border-box;
}
.input-and-icon.disabled {
  background-color: #f4f4f4;
}
.input-field {
  min-width: 16px;
  border: none;
  outline: none;
  background-color: transparent;
  font-family: "Golos Text", sans-serif;
  font-weight: 400;
  font-size: 20px;
  line-height: 130%;
  letter-spacing: 0;
  padding: 0;
  transition: width 0.2s ease;
}
.sizer {
  position: absolute;
  visibility: hidden;
  white-space: pre;
  font-family: "Golos Text", sans-serif;
  font-size: 20px;
  font-weight: 400;
  line-height: 130%;
  letter-spacing: 0;
}
.input-icon {
  width: 24px;
  height: 24px;
  cursor: pointer;
}
.error-text {
  margin: 4px 0 0 0;
  color: #f44336;
  font-size: 18px;
  font-family: "Golos Text", sans-serif;
  font-weight: 400;
  line-height: 130%;
  letter-spacing: 0;
  width: 100%;
  text-align: center;
}
</style>
