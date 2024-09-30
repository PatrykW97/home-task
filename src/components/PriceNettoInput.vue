<script setup>
import { ref, watch, defineProps } from 'vue';

const props = defineProps(['modelValue', 'vat', 'error']);
const emit = defineEmits(['update:modelValue']);

const priceNetto = ref(props.modelValue);

watch(priceNetto, (newValue) => emit('update:modelValue', newValue));

// allow only numbers and commas
const validateNumberInput = () => {
  // changle commas to a dot
  priceNetto.value = priceNetto.value.replace(',', '.');

  // check if given number meets the validation regex
  if (!/^\d+(\.\d{0,2})?$/.test(priceNetto.value)) {
    priceNetto.value = priceNetto.value.replace(/[^0-9.]/g, '');
    
    // check if first input is a dot
    if (priceNetto.value.startsWith('.')) {
      priceNetto.value = '';
    }

    // allow for only one dot, if more are typed in delete them
    const parts = priceNetto.value.split('.');
    if (parts.length > 2) {
      priceNetto.value = `${parts[0]}.${parts[1]}`;
    }
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