<script setup>
import { ref, reactive, computed, onMounted, watch } from "vue";
const formInformation = reactive({
  message: "",
  confirmation: null,
  vat: "",
  priceNetto: null,
  priceBrutton: null,
});

const showLengthError = ref(false);
const bruttoValue = ref(0);
const priveNettoValidateError = ref(false);
function checkMessageLength() {
  if (formInformation.message.length > 255) {
    showLengthError.value = true;
  } else {
    showLengthError.value = false;
  }
}

const calculatedPriceBrutto = computed(() => {
  const netto = parseFloat(formInformation.priceNetto);
  const vat = parseFloat(formInformation.vat);

  if (isNaN(netto) || isNaN(vat)) {
    return 0;
  }

  return netto + netto * (vat / 100);
});

const validatePriceNetto = (event) => {
  const value = event.target.value;
  const regex = /^[0-9.,]*$/;
  priveNettoValidateError.value = false;
  if (!regex.test(value)) {
    event.target.value = value.replace(/[^0-9.,]/g, "");
    formInformation.priceNetto = event.target.value;
    priveNettoValidateError.value = true;
  }
};

watch(formInformation, checkMessageLength);
</script>

<template>
  <form @submit.prevent="handleSubmit" class="form">
    <div class="input-container">
      <label class="label">Description</label>
      <textarea
        :class="{ 'error-textarea': formInformation.message.length > 255 }"
        v-model="formInformation.message"
        @input="validateTextarea"
      ></textarea>
      <div class="error-container">
        <span :class="{ 'message-full': formInformation.message.length > 255 }"
          >{{ formInformation.message.length }} / 255</span
        >
        <span v-if="formInformation.message.length > 255" class="error">You can't enter more than 255 characters</span>
      </div>
    </div>
    <div class="input-container">
      <label>Send confirmation</label>
      <div>
        <input
          type="radio"
          id="yes"
          name="confirmation"
          value="YES"
          v-model="formInformation.confirmation"
        />
        <label for="yes">Yes</label>

        <input
          type="radio"
          id="no"
          name="confirmation"
          value="NO"
          v-model="formInformation.confirmation"
        />
        <label for="no">No</label>
      </div>
    </div>
    <div class="input-container">
      <label>VAT</label>
      <select v-model="formInformation.vat">
        <option value="" disabled selected hidden>Choose VAT</option>
        <option value="19">19%</option>
        <option value="21">21%</option>
        <option value="23">23%</option>
        <option value="25">25%</option>
      </select>
    </div>
    <div class="input-container">
      <label>Price netto EUR</label>
      <input
        type="text"
        :disabled="formInformation.vat == ''"
        @input="validatePriceNetto"
        v-model="formInformation.priceNetto"
      />
      <span v-if="priveNettoValidateError">Please, input number</span>
    </div>
    <div class="input-container">
      <label>Price brutton EUR</label>
      <input type="text" disabled v-model="calculatedPriceBrutto" />
    </div>
    <input type="submit" value="Submit" />
  </form>
</template>

<style>
.form {
  display: flex;
  flex-direction: column;
}
.label {
  font-size: 12px;
}
.input-container {
  display: flex;
  flex-direction: column;
}
textarea {
  resize: vertical;
  width: 400px;
  padding: 0.5rem;
}
.error-textarea {
  outline-color: red;
}
.message-full {
  color: red;
}
.error-container {
  display: flex;
  gap: 10px;
}
</style>
