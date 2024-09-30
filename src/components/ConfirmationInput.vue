<script setup>
import { ref, watch, defineProps } from 'vue';

const props = defineProps(['modelValue', 'error']);
const emit = defineEmits(['update:modelValue']);

const confirmation = ref(props.modelValue);

watch(confirmation, (newValue) => emit('update:modelValue', newValue));
</script>

<template>
    <div class="input-container">
        <label>Send confirmation</label>
        <div class="radio-container">
            <input type="radio" id="yes" value="YES" v-model="confirmation">
            <span for="yes">Yes</span>

            <input type="radio" id="no" value="NO" v-model="confirmation">
            <span for="no">No</span>
        </div>
        <span v-if="error" class="error">{{error}}</span>
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
.radio-container{
    display:flex;
    gap:6px;
}
</style>