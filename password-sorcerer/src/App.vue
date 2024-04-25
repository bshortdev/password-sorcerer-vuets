<script setup lang="ts">
import { ref, computed } from 'vue';
const minPasswordLength = 4;
const maxPasswordLength = 128;
const passwordLength = ref(minPasswordLength);
const hasLowercaseLetters = ref(true);
const hasUppercaseLetters = ref(false);
const hasNumbers = ref(false);
const hasSpecialCharacters = ref(false);
const generatedPassword = ref('');

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

const generatePassword = () =>{
  // Add randomization
  generatedPassword.value = 'GeneratedAPassword'
};
</script>

<template>
  <div class="container d-flex flex-column">
    <div class="row justify-content-center align-items-center vh-100">
      <div class="col-4">
        <div class="row justify-content-center py-3">
          <div class="col text-center">
            <h1>Password Sorcerer</h1>
          </div>
        </div>
        <div class="row justify-content-center pb-3">
          <div class="col-8">
            <!-- Add an icon for copying -->
            <input type="text" class="form-control" placeholder="Generated Password" disabled v-model="generatedPassword"/>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col-8">
            <div class="accordion" id="accordionExample">
              <div class="accordion-item">
                <h2 class="accordion-header">
                  <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse"
                    data-bs-target="#collapseOne" aria-expanded="false" aria-controls="collapseOne">
                    Settings
                  </button>
                </h2>
                <div id="collapseOne" class="accordion-collapse collapse" data-bs-parent="#accordionExample">
                  <div class="accordion-body">
                    <div class="row">
                      <div class="col-12">
                        <label for="passwordLengthRange" class="form-label">Password Length</label>
                      </div>
                      <div class="col-auto">
                        <!-- Need to have the range update as slider is sliding -->
                        <input type="range" class="form-range" :min="minPasswordLength" :max="maxPasswordLength"
                          id="passwordLengthRange" :valueAsNumber="passwordLength" @change="setPasswordLengthFromRange"
                          @input="setPasswordLengthFromRange" />                        
                      </div>
                      <div class="col">
                        <div>{{ passwordLength }}</div>
                      </div>
                    </div>
                    <div class="row justify-content-center pt-2">
                      <div class="col">
                        <div class="form-check">
                          <input type="checkbox" class="form-check-input" id="lowercaseCheckbox"
                            v-model="hasLowercaseLetters" />
                          <label for="lowercaseCheckbox" class="form-check-label">Lowercase Letters?</label>
                        </div>
                      </div>
                    </div>
                    <div class="row justify-content-center">
                      <div class="col">
                        <div class="form-check">
                          <input type="checkbox" class="form-check-input" id="uppercaseCheckbox"
                            v-model="hasUppercaseLetters" />
                          <label for="uppercaseCheckbox" class="form-check-label">Uppercase Letters?</label>
                        </div>
                      </div>
                    </div>
                    <div class="row justify-content-center">
                      <div class="col">
                        <div class="form-check">
                          <input type="checkbox" class="form-check-input" id="numbersCheckbox" v-model="hasNumbers" />
                          <label for="numbersCheckbox" class="form-check-label">Numbers?</label>
                        </div>
                      </div>
                    </div>
                    <div class="row justify-content-center">
                      <div class="col">
                        <div class="form-check">
                          <input type="checkbox" class="form-check-input" id="specialCharacatersCheckbox"
                            v-model="hasSpecialCharacters" />
                          <label for="specialCharacatersCheckbox" class="form-check-label">Special Characters?</label>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="row justify-content-center pt-3">
          <div class="col-8">
            <button type="button" class="btn btn-primary w-100" :disabled="!isReadyToGeneratePassword" @click="generatePassword">Generate
              Password</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
