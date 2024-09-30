<script setup>
import { defineProps } from 'vue';

const props = defineProps(['priceBrutto']);

</script>

<template>
  <div class="input-container">
    <label>Price brutto EUR</label>
    <input type="text" :value="priceBrutto" disabled />
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