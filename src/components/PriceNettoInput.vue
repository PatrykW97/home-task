<script setup>
import { ref, watch, defineProps } from 'vue';

const props = defineProps(['modelValue', 'vat', 'error']);
const emit = defineEmits(['update:modelValue']);

const priceNetto = ref(props.modelValue);

watch(priceNetto, (newValue) => emit('update:modelValue', newValue));

// allow only numbers and commas
const validateNumberInput = () => {
  if (!/^[0-9.,]*$/.test(priceNetto.value)) {
    priceNetto.value = priceNetto.value.replace(/[^0-9.,]/g, '');
  }
};
</script>

<template>
  <div class="input-container">
    <label>Price netto EUR</label>
    <input
      type="text"
      :disabled="!vat"
      v-model="priceNetto"
      @input="validateNumberInput"
    />
    <span v-if="error" class="error">{{ error }}</span>
  </div>
</template>
<style scoped>
label{
    font-weight: 700;
    font-size:16px
}
.input-container{
  display: flex;
  flex-direction: column;
}
.error {
  color: red;
  font-size: 12px;
}
input{
    padding:4px;
}
</style>