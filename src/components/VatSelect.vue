<script setup>
import { ref, watch, defineProps } from "vue";

const props = defineProps(["modelValue", "error"]);
const emit = defineEmits(["update:modelValue"]);

const vat = ref(props.modelValue);

watch(vat, (newValue) => emit("update:modelValue", newValue));
</script>

<template>
  <div class="input-container">
    <label>VAT</label>
    <select v-model="vat">
      <option value="" disabled hidden>Choose VAT</option>
      <option value="19">19%</option>
      <option value="21">21%</option>
      <option value="23">23%</option>
      <option value="25">25%</option>
    </select>
    <span v-if="error" class="error">{{ error }}</span>
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
select{
    padding:4px;
}
</style>
