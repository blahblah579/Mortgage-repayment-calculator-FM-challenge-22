<template>
  <v-container class="pa-0">
    <v-row v-if="isScreenLarge" class="d-flex justify-center">
      <v-col cols="11" lg="9" class="pa-0 body d-flex flex-column flex-sm-row">
        <v-col cols="12" sm="6" class="px-5 left">
          <v-col cols="12" class="pa-0 d-flex">
            <v-col cols="9">
              <div class="heading">Mortgage Calculator</div>
            </v-col>
            <v-col cols="3" class="pa-0 d-flex justify-end mr-3 align-center">
              <a href="#" class="clear-all" @click="clearAll">Clear All</a>
            </v-col>
          </v-col>

          <!-- Mortgage Amount -->
          <v-col cols="12" class="my-2">
            <div class="heading2">Mortgage Amount</div>
            <div :class="['input1 d-flex mt-2', { 'input-error': showError1 }]">
              <v-col
                cols="1"
                :class="[
                  'symbol px-5 py-2 d-flex align-center justify-center',
                  { 'symbol-error': showError1 },
                ]"
                >₹</v-col
              >
              <v-col cols="11" class="py-2">
                <input
                  type="number"
                  v-model="mortgageAmount"
                  class="input1-field"
                  placeholder=""
                />
              </v-col>
            </div>
            <div v-if="showError1" class="error-msg1 mt-2">
              This field is required
            </div>
          </v-col>

          <!-- Mortgage Term and Interest Rate -->
          <v-col cols="12" class="d-flex py-2">
            <v-col cols="6" class="py-0 pl-0">
              <div class="heading3">Mortgage Term</div>
              <div
                :class="['input2 d-flex mt-2', { 'input-error': showError2 }]"
              >
                <v-col cols="7" class="py-2">
                  <input
                    type="number"
                    v-model="mortgageTerm"
                    class="input2-field"
                    placeholder=""
                  />
                </v-col>
                <v-col
                  cols="5"
                  :class="[
                    'years py-2 d-flex justify-center',
                    { 'years-error': showError2 },
                  ]"
                  >years</v-col
                >
              </div>
              <div v-if="showError2" class="error-msg1 mt-2">
                This field is required
              </div>
            </v-col>
            <v-col cols="6" class="pa-0">
              <div class="heading4">Interest Rate</div>
              <div
                :class="['input3 d-flex mt-2', { 'input-error': showError3 }]"
              >
                <v-col cols="9 py-2">
                  <input
                    type="number"
                    v-model="interestRate"
                    class="input3-field"
                    placeholder=""
                    step="0.01"
                  />
                </v-col>
                <v-col
                  cols="3"
                  :class="[
                    'years2 d-flex justify-center py-2',
                    { 'years2-error': showError3 },
                  ]"
                  >%</v-col
                >
              </div>
              <div v-if="showError3" class="error-msg1 mt-2">
                This field is required
              </div>
            </v-col>
          </v-col>

          <!-- Mortgage Type -->
          <v-col cols="12">
            <div class="heading5">Mortgage Type</div>
            <form class="my-2">
              <div
                class="option1 py-2"
                :class="
                  mortgageType === 'repayment' ? 'selected' : 'notSelected'
                "
              >
                <label class="px-4">
                  <input
                    type="radio"
                    class="custom-radio"
                    name="mortgageType"
                    v-model="mortgageType"
                    value="repayment"
                  />
                  <span class="radio-text">Repayment</span>
                </label>
              </div>
              <div
                class="option2 py-2 mt-2"
                :class="
                  mortgageType === 'interestOnly' ? 'selected' : 'notSelected'
                "
              >
                <label class="px-4">
                  <input
                    type="radio"
                    class="custom-radio"
                    name="mortgageType"
                    v-model="mortgageType"
                    value="interestOnly"
                  />
                  <span class="radio-text">Interest Only</span>
                </label>
              </div>
            </form>
            <div v-if="showError4" class="error-msg1 mt-2">
              This field is required
            </div>
          </v-col>

          <!-- Calculate Button -->
          <v-col cols="12">
            <v-btn
              class="calculate-btn text-capitalize pa-10 py-6 d-flex justify-center align-center"
              variant="flat"
              color=""
              rounded
              elevation="0"
              @click="calculateRepayments"
            >
              <v-icon class="mr-2" icon="mdi-calculator" size="large"></v-icon>
              Calculate Repayments
            </v-btn>
          </v-col>
        </v-col>

        <!-- Results -->
        <v-col cols="12" sm="6" class="right d-flex flex-column align-center">
          <v-col
            v-if="isMonthlyRepaymentValid"
            cols="12"
            class="d-flex flex-column py-0"
          >
            <v-col class="my-0">
              <div class="heading11">Your results</div>
              <div class="para11 my-3">
                Your results are shown below based on the information you
                provided. To adjust the results, edit the form and click
                "Calculate Repayments" again.
              </div>
              <div class="results d-flex flex-column px-7 py-7 mt-8">
                <div class="heading12 my-0">Your monthly repayments</div>
                <div class="final-result1 d-flex">
                  {{ monthlyRepayment }}
                </div>
                <v-divider class="mt-6 mb-7"></v-divider>
                <div class="para12">Total you'll repay over the term</div>
                <div class="final-result2">{{ totalRepayment }}</div>
              </div>
            </v-col>
          </v-col>

          <v-col v-else cols="12" class="d-flex py-0">
            <v-col class="d-flex flex-column align-center justify-center">
              <div class="img1 d-flex justify-center">
                <img src="/src/assets/illustration-empty.svg" alt="" />
              </div>
              <div class="text1 text-center">Results shown here</div>
              <div class="text2 text-center">
                Complete the form and click "Calculate Repayments" to see what
                your monthly repayments would be.
              </div>
            </v-col>
          </v-col>
        </v-col>
      </v-col>
    </v-row>
    <v-row v-else class="d-flex flex-column">
      <v-col cols="12" class="pa-0 body2 d-flex flex-column">
        <!-- MAIN -->
        <v-col cols="12" class="px-10 left ">
          <v-col cols="12" class="pa-0 d-flex flex-column">
            <v-col cols="9">
              <div class="heading">Mortgage Calculator</div>
            </v-col>
            <v-col cols="3" class="pa-0 d-flex ml-3 mb-3 align-center">
              <a href="#" class="clear-all" @click="clearAll">Clear All</a>
            </v-col>
          </v-col>

          <!-- Mortgage Amount -->
          <v-col cols="12" class="my-2">
            <div class="heading2">Mortgage Amount</div>
            <div :class="['input1 d-flex mt-2', { 'input-error': showError1 }]">
              <v-col
                cols="1"
                :class="[
                  'symbol px-5 py-2 d-flex align-center justify-center',
                  { 'symbol-error': showError1 },
                ]"
                >₹</v-col
              >
              <v-col cols="11" class="py-2">
                <input
                  type="number"
                  v-model="mortgageAmount"
                  class="input1-field"
                  placeholder=""
                />
              </v-col>
            </div>
            <div v-if="showError1" class="error-msg1 mt-2">
              This field is required
            </div>
          </v-col>

          <!-- Mortgage Term and Interest Rate -->
          <v-col cols="12" class="d-flex py-2">
            <v-col cols="6" class="py-0 pl-0">
              <div class="heading3">Mortgage Term</div>
              <div
                :class="['input2 d-flex mt-2', { 'input-error': showError2 }]"
              >
                <v-col cols="7" class="py-2">
                  <input
                    type="number"
                    v-model="mortgageTerm"
                    class="input2-field"
                    placeholder=""
                  />
                </v-col>
                <v-col
                  cols="5"
                  :class="[
                    'years py-2 d-flex justify-center',
                    { 'years-error': showError2 },
                  ]"
                  >years</v-col
                >
              </div>
              <div v-if="showError2" class="error-msg1 mt-2">
                This field is required
              </div>
            </v-col>
            <v-col cols="6" class="pa-0">
              <div class="heading4">Interest Rate</div>
              <div
                :class="['input3 d-flex mt-2', { 'input-error': showError3 }]"
              >
                <v-col cols="9 py-2">
                  <input
                    type="number"
                    v-model="interestRate"
                    class="input3-field"
                    placeholder=""
                    step="0.01"
                  />
                </v-col>
                <v-col
                  cols="3"
                  :class="[
                    'years2 d-flex justify-center py-2',
                    { 'years2-error': showError3 },
                  ]"
                  >%</v-col
                >
              </div>
              <div v-if="showError3" class="error-msg1 mt-2">
                This field is required
              </div>
            </v-col>
          </v-col>

          <!-- Mortgage Type -->
          <v-col cols="12">
            <div class="heading5">Mortgage Type</div>
            <form class="my-2">
              <div
                class="option1 py-2"
                :class="
                  mortgageType === 'repayment' ? 'selected' : 'notSelected'
                "
              >
                <label class="px-4">
                  <input
                    type="radio"
                    class="custom-radio"
                    name="mortgageType"
                    v-model="mortgageType"
                    value="repayment"
                  />
                  <span class="radio-text">Repayment</span>
                </label>
              </div>
              <div
                class="option2 py-2 mt-2"
                :class="
                  mortgageType === 'interestOnly' ? 'selected' : 'notSelected'
                "
              >
                <label class="px-4">
                  <input
                    type="radio"
                    class="custom-radio"
                    name="mortgageType"
                    v-model="mortgageType"
                    value="interestOnly"
                  />
                  <span class="radio-text">Interest Only</span>
                </label>
              </div>
            </form>
            <div v-if="showError4" class="error-msg1 mt-2">
              This field is required
            </div>
          </v-col>

          <!-- Calculate Button -->
          <v-col cols="12">
            <v-btn
              class="calculate-btn text-capitalize pa-10 py-6 d-flex justify-center align-center"
              variant="flat"
              color=""
              block
              rounded
              elevation="0"
              @click="calculateRepayments"
            >
              <v-icon class="mr-2" icon="mdi-calculator" size="large"></v-icon>
              Calculate Repayments
            </v-btn>
          </v-col>
        </v-col>

        <!-- Results -->
        <v-col cols="12" sm="6" class="right20 d-flex flex-column align-center">
          <v-col
            v-if="isMonthlyRepaymentValid"
            cols="12"
            class="d-flex flex-column py-0"
          >
            <v-col class="my-0">
              <div class="heading11">Your results</div>
              <div class="para11 my-3">
                Your results are shown below based on the information you
                provided. To adjust the results, edit the form and click
                "Calculate Repayments" again.
              </div>
              <div class="results d-flex flex-column px-7 py-7 mt-8">
                <div class="heading12 my-0">Your monthly repayments</div>
                <div class="final-result1 d-flex">
                  {{ monthlyRepayment }}
                </div>
                <v-divider class="mt-6 mb-7"></v-divider>
                <div class="para12">Total you'll repay over the term</div>
                <div class="final-result2">{{ totalRepayment }}</div>
              </div>
            </v-col>
          </v-col>

          <v-col v-else cols="12" class="d-flex py-7 pa-16">
            <v-col class="d-flex flex-column align-center justify-center">
              <div class="img1 d-flex justify-center">
                <img src="/src/assets/illustration-empty.svg" alt="" />
              </div>
              <div class="text1 text-center">Results shown here</div>
              <div class="text2 text-center">
                Complete the form and click "Calculate Repayments" to see what
                your monthly repayments would be.
              </div>
            </v-col>
          </v-col>
        </v-col>
      </v-col>
    </v-row>
  </v-container>
</template>
  
  <script setup>
import { computed, onMounted, onUnmounted, ref } from "vue";

const isScreenLarge = ref(window.innerWidth >= 595.5);
const handleResize = () => {
  isScreenLarge.value = window.innerWidth >= 595.5;
};

// Add event listener on mounted and clean up on unmounted
onMounted(() => {
  window.addEventListener("resize", handleResize);
});

onUnmounted(() => {
  window.removeEventListener("resize", handleResize);
});

const showError1 = ref(false);
const showError2 = ref(false);
const showError3 = ref(false);
const showError4 = ref(false);
const mortgageAmount = ref();
const mortgageTerm = ref();
const interestRate = ref();
const mortgageType = ref();
const monthlyRepayment = ref("");
const totalRepayment = ref("");

const isMonthlyRepaymentValid = computed(() => {
  // Check if monthlyRepayment is a number and greater than 0
  const repaymentValue = parseFloat(
    monthlyRepayment.value.replace(/[^\d.-]/g, "")
  );
  return !isNaN(repaymentValue) && repaymentValue > 0;
});

const calculateRepayments = () => {
  if (!mortgageAmount.value) showError1.value = true;
  else showError1.value = false;
  if (!mortgageTerm.value) showError2.value = true;
  else showError2.value = false;
  if (!interestRate.value) showError3.value = true;
  else showError3.value = false;
  if (
    mortgageType.value !== "repayment" &&
    mortgageType.value !== "interestOnly"
  )
    showError4.value = true;
  else showError4.value = false;

  console.log(mortgageType);
  const principal = parseFloat(mortgageAmount.value);
  const rate = parseFloat(interestRate.value) / 100 / 12;
  const termInMonths = parseInt(mortgageTerm.value) * 12;

  if (mortgageType.value === "repayment") {
    // Repayment mortgage formula
    const repayment =
      (principal * rate) / (1 - Math.pow(1 + rate, -termInMonths));
    monthlyRepayment.value = `₹${repayment.toFixed(2)}`;
    totalRepayment.value = `₹${(repayment * termInMonths).toFixed(2)}`;
  } else if (mortgageType.value === "interestOnly") {
    // Interest-only mortgage formula
    const interestOnlyRepayment = principal * rate;
    monthlyRepayment.value = `₹${interestOnlyRepayment.toFixed(2)}`;
    totalRepayment.value = `₹${(interestOnlyRepayment * termInMonths).toFixed(
      2
    )}`;
  }
};

const clearAll = () => {
  mortgageAmount.value = null;
  mortgageTerm.value = null;
  interestRate.value = null;
  mortgageType.value = null;
  monthlyRepayment.value = "";
  totalRepayment.value = "";
};
</script>
  
  <style scoped>
@font-face {
  font-family: f1;
  src: url(/src/assets/PlusJakartaSans-Bold.ttf);
}
@font-face {
  font-family: f2;
  src: url(/src/assets/PlusJakartaSans-Medium.ttf);
}
.body {
  background-color: #ffffff;
  border-radius: 20px;
}
.body2 {
  background-color: #ffffff;
}
.right {
  background-color: hsl(201, 54%, 16%);
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  border-bottom-left-radius: 60px;
}
.final-result1,
.final-result2 {
  font-weight: bold;
  margin-top: 3px;
}

.heading {
  color: hsl(202, 55%, 16%);
  font-family: f1;
  font-size: 22px;
}
.clear-all {
  color: hsl(200, 26%, 54%);
  font-family: f2;
  font-size: 14px;
}
.heading2 {
  color: hsl(200, 24%, 40%);
  font-family: f2;
  font-size: 14px;
  font-weight: lighter;
}
.input1 {
  border: 1.5px solid hsl(196, 10%, 61%);
  border-radius: 5px;
}
.input1:hover {
  border: 1.5px solid hsl(61, 70%, 52%);
}
.input1:hover .symbol {
  color: hsl(79, 100%, 10%);
  background-color: hsl(61, 70%, 52%);
  font-family: f1;
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}
.input1-field,
.input2-field,
.input3-field {
  color: black;
  font-family: f1;
  width: 100%;
}
.symbol {
  color: hsl(202, 17%, 42%);
  background-color: hsl(199, 81%, 94%);
  font-family: f1;
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}
.symbol-error {
  color: hsl(0, 0%, 100%); /* Error color */
  background-color: hsl(3, 69%, 50%); /* Error background */
}

.input-error {
  border: 1.5px solid hsl(3, 69%, 50%); /* Error border color */
}

.heading3 {
  color: hsl(198, 19%, 37%);
  font-family: f2;
  font-size: 14px;
  font-weight: lighter;
}
.input2 {
  border: 1.5px solid hsl(196, 10%, 61%);
  border-radius: 5px;
}
.input2:hover {
  border: 1.5px solid hsl(61, 70%, 52%);
}
.input2:hover .years {
  color: hsl(79, 100%, 10%);
  background-color: hsl(61, 70%, 52%);
  font-family: f1;
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
}
.years {
  color: hsl(198, 19%, 37%);
  background-color: hsl(199, 81%, 94%);
  font-family: f1;
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
}
.years2 {
  color: hsl(198, 19%, 37%);
  background-color: hsl(199, 81%, 94%);
  font-family: f1;
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
  font-weight: bolder;
}
.input-error {
  border: 1.5px solid hsl(3, 69%, 50%);
}

.years-error {
  color: hsl(0, 0%, 100%);
  background-color: hsl(3, 69%, 50%);
}

.years2-error {
  color: hsl(0, 0%, 100%);
  background-color: hsl(3, 69%, 50%);
  font-family: f1;
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
  font-weight: bolder;
}

.heading4 {
  color: hsl(198, 19%, 37%);
  font-family: f2;
  font-size: 14px;
  font-weight: lighter;
}
.input3 {
  border: 1.5px solid hsl(196, 10%, 61%);
  border-radius: 5px;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}
.input3:hover {
  border: 1.5px solid hsl(61, 70%, 52%);
}
.input3:hover .years2 {
  color: hsl(79, 100%, 10%);
  background-color: hsl(61, 70%, 52%);
  font-family: f1;
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
}
/* Remove increment/decrement arrows for number inputs */
input[type="number"] {
  -moz-appearance: textfield; /* Firefox */
  appearance: textfield; /* Other browsers */
}

input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none; /* Chrome, Safari, Edge */
  margin: 0;
}
input:focus {
  outline: none;
  border-color: transparent;
}
.heading5 {
  color: hsl(198, 19%, 37%);
  font-family: f2;
  font-size: 14px;
  font-weight: lighter;
}
.calculate-btn {
  background-color: hsl(62, 61%, 57%);
  color: hsl(172, 25%, 13%);
  border-radius: 50px;
  padding-left: 20px;
  padding-right: 20px;
  font-family: f2;
  letter-spacing: -0.2px;
  font-weight: bolder;
}
.option1 {
  border: 1.5px solid hsl(196, 10%, 61%);
  background-color: transparent;
  border-radius: 5px;
}
.radio-text {
  color: hsl(202, 55%, 16%);
  font-family: f1;
  font-size: 16px;
  margin-left: 10px;
}
.option2 {
  border: 1.5px solid hsl(196, 10%, 61%);
  background-color: transparent;
  border-radius: 5px;
}
.heading11 {
  color: hsl(0, 0%, 100%);
  font-family: f1;
  font-size: 22px;
}
.para11 {
  color: hsl(203, 41%, 72%);
  font-family: f2;
  font-size: 14px;
}
.results {
  background-color: hsl(202, 56%, 12%);
  border-radius: 5px;
  border-top: 3.5px solid hsl(62, 61%, 57%);
}
.heading12 {
  color: hsl(203, 41%, 72%);
  font-family: f2;
  font-size: 13px;
}
.final-result1 {
  color: hsl(61, 72%, 52%);
  font-family: f1;
  /* font-size: 50px; */
  display: inline-flex; /* Allows wrapping and flexible resizing */
  flex-wrap: wrap; /* Wrap the text to the next line if it overflows */
  font-size: clamp(
    3.5rem,
    2.5vw,
    1.5rem
  ); /* Dynamically adjust the font size */
  width: 100%; /* Make sure it takes the full width of the container */
  word-break: break-word; /* Break long numbers/words */
}
.para12 {
  color: hsl(203, 41%, 72%);
  font-family: f2;
  font-size: 13px;
}
.final-result2 {
  color: hsl(0, 0%, 100%);
  font-family: f1;
  /* font-size: 22px; */
  display: inline-flex; /* Allows wrapping and flexible resizing */
  flex-wrap: wrap; /* Wrap the text to the next line if it overflows */
  font-size: clamp(
    0.8rem,
    2.5vw,
    1.5rem
  ); /* Dynamically adjust the font size */
  width: 100%; /* Make sure it takes the full width of the container */
  word-break: break-word; /* Break long numbers/words */
}
.text1 {
  color: white;
  font-family: f1;
  font-size: 20px;
}
.text2 {
  color: hsl(203, 41%, 72%);
  font-family: f2;
  font-size: 14px;
}

.custom-radio {
  display: none; /* Hide the default radio button */
}

.radio-text {
  position: relative;
  cursor: pointer;
  padding-left: 30px; /* Space for custom radio button */
}

/* Outer Circle (unselected state) */
.radio-text::before {
  content: "";
  display: inline-block;
  width: 20px; /* Outer circle size */
  height: 20px;
  border: 2px solid hsl(189, 6%, 49%); /* Initial border color */
  border-radius: 50%;
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  transition: border-color 0.3s, background-color 0.3s;
}

/* Inner Circle (visible only when selected) */
.radio-text::after {
  content: "";
  display: none; /* Hidden initially */
  width: 10px; /* Smaller inner circle */
  height: 10px;
  background-color: hsl(61, 70%, 52%); /* Color for inner circle */
  border-radius: 50%;
  position: absolute;
  left: 5px; /* Center inside the outer circle */
  top: 50%;
  transform: translateY(-50%);
}

/* When the radio is checked, show the inner circle and change the outer circle's border color */
.custom-radio:checked + .radio-text::before {
  border-color: hsl(61, 70%, 52%); /* Outer circle color when checked */
}

.custom-radio:checked + .radio-text::after {
  display: inline-block; /* Show inner circle */
}
.selected {
  background-color: hsl(60, 72%, 93%);
  border: 1.5px solid hsl(61, 70%, 52%);
}
.notSelected {
  border: 1.5px solid transparent;
  border: 1.5px solid hsl(198, 7%, 54%);
}

.error-msg1 {
  color: hsl(3, 69%, 50%);
  font-size: 12px;
  font-family: f2;
}
.right20 {
  background-color: hsl(201, 54%, 16%);
}
</style>
  