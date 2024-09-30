<script setup>
import { ref, watch, defineProps } from "vue";

const props = defineProps(["modelValue", "error"]);
const emit = defineEmits(["update:modelValue"]);

const localError = ref("");

const description = ref(props.modelValue);

//check if user is trying to type while the description reached its limit
const checkDescriptionLength = (event) => {
    //check if length is 255 and block the keydown copy paste but allow other keydowns to not trigger the error
   if (
     description.value.length >= 255 &&
     !["Backspace", "Delete", "ArrowLeft", "ArrowRight", "ArrowUp", "ArrowDown","Alt","Control"].includes(event.key)
   ) {
     localError.value = "You can’t enter more than 255 characters";
     // set timeout to hide notification
     setTimeout(() => {
       localError.value = "";
     }, 3000);

     event.preventDefault();
   }
 };
watch(description, (newValue) => emit("update:modelValue", newValue));
</script>

<template>
  <div class="input-container">
    <label>Description</label>
    <textarea
      v-model="description"
      maxlength="255"
      @keydown="checkDescriptionLength"
    ></textarea>
    <span v-if="error" class="error">{{ error }}</span>
    <span v-if="localError" class="error">{{ localError }}</span>
    <p :class="{'text-length': description.length == 255}">{{ description.length }} / 255</p>
  </div>
</template>

<style scoped>
label {
  font-weight: 700;
  font-size: 16px;
}
.input-container {
  display: flex;
  flex-direction: column;
}
.error {
  color: red;
  font-size: 12px;
}
textarea{
  resize:vertical;
  padding:6px;
  font-size:16px; 
}
.text-length{
  color:red;
}
</style>
