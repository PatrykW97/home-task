<script setup>
import { reactive, computed, ref } from "vue";
import DescriptionInput from "./DescriptionInput.vue";
import ConfirmationInput from "./ConfirmationInput.vue";
import VatSelect from "./VatSelect.vue";
import PriceBruttoInput from "./PriceBruttoInput.vue";
import PriceNettoInput from "./PriceNettoInput.vue";

const form = reactive({
  description: "",
  confirmation: "",
  vat: "",
  priceNetto: "",
});

const errors = reactive({
  description: "",
  confirmation: "",
  vat: "",
  priceNetto: "",
});

const formSubmitted = ref(false);
const errorOccurred = ref(false);
const isSubmitting = ref(false);

// calculate brutto price from netto and vat value
const calculatedPriceBrutto = computed(() => {
  const nettoValue = parseFloat(form.priceNetto);
  const vatValue = parseFloat(form.vat);
  if (isNaN(nettoValue) || isNaN(vatValue)) {
    return 0;
  }
  return nettoValue + nettoValue * (vatValue / 100);
});

// validate form when submiting
const validateForm = () => {
  errors.description = form.description ? "" : "Text is required";
  errors.confirmation = form.confirmation ? "" : "Text is required";
  errors.vat = form.vat ? "" : "Text is required";
  errors.priceNetto =
    form.priceNetto && /^[0-9.,]+$/.test(form.priceNetto)
      ? ""
      : "Please, input number";

  return (
    !errors.description &&
    !errors.confirmation &&
    !errors.vat &&
    !errors.priceNetto
  );
};

//clear error if value is changed
const clearError = (field) => {
  errors[field] = ''; 
};

// submit form with api fetch simulation
const handleSubmit = async () => {
  if (!validateForm()) return;

  isSubmitting.value = true;

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        description: form.description,
        confirmation: form.confirmation,
        vat: form.vat,
        priceNetto: form.priceNetto,
        priceBrutto: calculatedPriceBrutto.value
      }),
    });

    if (response.ok) {
      formSubmitted.value = true;
    } else {
      errorOccurred.value = true;
    }
  } catch (error) {
    errorOccurred.value = true;
  } finally {
    isSubmitting.value = false;
  }
};
</script>

<template>
  <form :class="{'hidden-form': formSubmitted == true}" @submit.prevent="handleSubmit">
    <DescriptionInput v-model="form.description" :error="errors.description"  @input="clearError('description')"/>
    <ConfirmationInput
      v-model="form.confirmation"
      :error="errors.confirmation"
      @input="clearError('confirmation')"
    />
    <VatSelect v-model="form.vat" :error="errors.vat" @input="clearError('vat')"/>
    <PriceNettoInput
      v-model="form.priceNetto"
      :vat="form.vat"
      :error="errors.priceNetto"
      @input="clearError('priceNetto')"
    />
    <PriceBruttoInput :priceBrutto="calculatedPriceBrutto" />
    <input class="subimt-btn" type="submit" value="Submit" :disabled="isSubmitting" />
  </form>
  <div v-if="formSubmitted" class="success-message">
    <p>Form submited successfully!</p>
  </div>

  <div v-if="errorOccurred">
    <p>Something went wrong. Please try again.</p>
  </div>
</template>

<style scoped>
form{
  width:500px;
  display: flex;
  flex-direction: column;
  gap:6px;
}
.hidden-form{
  display: none;
}
.success-message {
  background-color: green;
  color: white;
  padding: 15px;
  margin-top: 10px;
}

.error-message {
  background-color: red;
  color: white;
  padding: 15px;
  margin-top: 10px;
}
.subimt-btn{
  width:30%
}
</style>
