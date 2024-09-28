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
  priceNetto:"",
})

const errors = reactive({
  description:"",
  confirmation:"",
  vat:"",
  priceNetto:"",
})

const formSubmitted = ref(false)
const errorOccured = ref(false)
const isSubmiting = ref(false)

// calculate brutto price from netto and vat value
const calculateBruttoPrice = computed(()=>{
  const nettoValue = parseFloat(form.priceNetto)
  const vatValue = parseFloat(form.vat)
  if(isNaN(nettoValue)||isNaN(vatValue)){
    return 0;
  }
  return nettoValue + nettoValue*(vat/100)
})

// validate form when submiting
const validateForm = () =>{
  errors.description = form.description ? '' : "Text is required"
  errors.confirmation = form.confirmation ? '' : "Text is required"
  errors.vat = form.vat ? '' : "Text is required"
  errors.priceNetto = form.priceNetto && /^[0-9.,]+$/.test(form.priceNetto) ? '' : 'Please, input number';
  
  return !errors.description && !errors.confirmation && !errors.vat && !errors.priceNetto
}

// submit form with api fetch simulation
const handleSubmit = async() =>{
  if(!validateForm()) return

  isSubmiting.value = true

  try{
    await new Promise(resolve => setTimeout(resolve, 100))
    formSubmitted.value = true
  }catch(error){
    errorOccured.value = true
  }finally{
    isSubmiting.value = false
  }
}

</script>

<template>
  <form @submit.prevent="handleSubmit">
    <DescriptionInput />
    <ConfirmationInput />
    <VatSelect />
    <PriceNettoInput />
    <PriceBruttoInput />
    <input type="submit" value="Submit" :disabled="isSubmiting">
  </form>

  <div v-if="formSubmitted" class="success-message">
    <p>Form submited successfully!</p>
  </div>

  <div v-if="errorOccured">
    <p>Something went wrong. Please try again.</p>
  </div>
</template>

<style scoped>
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
</style>

<!-- const props = defineProps({
  vat: {
    type: String,
    required: true
  },
  priceNetto: {
    type: String,
  },
  priceBrutto: { type: String },
});

const emit = defineEmits(['update:vat', 'update:priceNetto']);

const priceNettoValidateError = ref(false);

const validatePriceNetto = (event) => {
  const value = event.target.value;
  const regex = /^[0-9.,]*$/;
  priceNettoValidateError.value = false;
  if (!regex.test(value)) {
    event.target.value = value.replace(/[^0-9.,]/g, "");
    priceNetto = event.target.value;
    priceNettoValidateError.value = true;
  }
  emit('update:priceNetto', event.target.value);
};

const calculatedPriceBrutto = computed(() => {
  const netto = parseFloat(props.priceNetto);
  const vat = parseFloat(props.vat);

  if (isNaN(netto) || isNaN(vat)) {
    return 0;
  }

  return netto + netto * (vat / 100);
}); -->