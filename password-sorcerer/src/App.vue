<script setup lang="ts">
import { ref, computed } from 'vue';
const minPasswordLength = 4;
const maxPasswordLength = 128;
const passwordLength = ref(minPasswordLength);
const hasLowercaseLetters = ref(true);
const hasUppercaseLetters = ref(false);
const hasNumbers = ref(false);
const hasSpecialCharacters = ref(false);

const isReadyToGeneratePassword = computed(() => {
  return hasLowercaseLetters.value ||
    hasUppercaseLetters.value ||
    hasNumbers.value ||
    hasSpecialCharacters.value
})

const setPasswordLengthFromRange = (event: Event) => {
  const eventTarget = event.target as HTMLInputElement
  passwordLength.value = eventTarget.valueAsNumber
}

// Add method to generate password
</script>

<template>
  <div class="container d-flex flex-column">
    <div class="row justify-content-center align-items-center vh-100">
      <div class="col-4">
        <div class="row justify-content-center">
          <div class="col text-center">
            <h1>Password Sorcerer</h1>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <label for="passwordLengthRange" class="form-label">Password Length</label>
          </div>
          <div class="col d-flex gap-4">
            <!-- Need to have the range update as slider is sliding -->
            <input type="range" class="form-range" :min="minPasswordLength" :max="maxPasswordLength"
              id="passwordLengthRange" :valueAsNumber="passwordLength" @change="setPasswordLengthFromRange">
            <div>{{ passwordLength }}</div>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col-6">
            <div class="form-check">
              <input type="checkbox" class="form-check-input" id="lowercaseCheckbox" v-model="hasLowercaseLetters">
              <label for="lowercaseCheckbox" class="form-check-label">Lowercase Letters?</label>
            </div>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col-6">
            <div class="form-check">
              <input type="checkbox" class="form-check-input" id="uppercaseCheckbox" v-model="hasUppercaseLetters">
              <label for="uppercaseCheckbox" class="form-check-label">Uppercase Letters?</label>
            </div>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col-6">
            <div class="form-check">
              <input type="checkbox" class="form-check-input" id="numbersCheckbox" v-model="hasNumbers">
              <label for="numbersCheckbox" class="form-check-label">Numbers?</label>
            </div>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col-6">
            <div class="form-check">
              <input type="checkbox" class="form-check-input" id="specialCharacatersCheckbox"
                v-model="hasSpecialCharacters">
              <label for="specialCharacatersCheckbox" class="form-check-label">Special Characters?</label>
            </div>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col-6">
            <button type="button" class="btn btn-primary" :disabled="!isReadyToGeneratePassword">Generate
              Password</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
