<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';
const minPasswordLength = 4;
const maxPasswordLength = 128;
const passwordLength = ref(minPasswordLength);
const hasLowercaseLetters = ref(true);
const hasUppercaseLetters = ref(true);
const hasNumbers = ref(true);
const hasSpecialCharacters = ref(true);
const generatedPassword = ref('');
const isPasswordGenerated = ref(false);
const isPasswordCopied = ref(false);

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

const getRandomValue = (value: number) => {
  return Math.floor(Math.random() * value);
}

const getRandomLowercaseLetter = () => {
  const lowercaseAsciiStart = 97;
  const letterIndex = getRandomValue(26);
  return String.fromCharCode(lowercaseAsciiStart + letterIndex);
}

const getRandomUppercaseLetter = () => {
  const uppercaseAsciiStart = 65;
  const letterIndex = getRandomValue(26);
  return String.fromCharCode(uppercaseAsciiStart + letterIndex);
}

const getRandomNumber = () => {
  const numbersAsciiStart = 48;
  const letterIndex = getRandomValue(10);
  return String.fromCharCode(numbersAsciiStart + letterIndex);
}

const getRandomSpecialCharacter = () => {
  const specialCharacterAsciiStart = 34;
  const letterIndex = getRandomValue(13);
  return String.fromCharCode(specialCharacterAsciiStart + letterIndex);
}

const shuffle = (array: string[]) => {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
};

const generatePassword = () => {
  let generatedPasswordHolder = ''
  const randomMethodsToUse = [];

  if (hasLowercaseLetters.value) {
    generatedPasswordHolder += getRandomLowercaseLetter();
    randomMethodsToUse.push(getRandomLowercaseLetter)
  }

  if (hasUppercaseLetters.value) {
    generatedPasswordHolder += getRandomUppercaseLetter();
    randomMethodsToUse.push(getRandomUppercaseLetter)
  }

  if (hasNumbers.value) {
    generatedPasswordHolder += getRandomNumber();
    randomMethodsToUse.push(getRandomNumber)
  }

  if (hasSpecialCharacters.value) {
    generatedPasswordHolder += getRandomSpecialCharacter();
    randomMethodsToUse.push(getRandomSpecialCharacter)
  }

  while (generatedPasswordHolder.length < passwordLength.value) {
    generatedPasswordHolder += randomMethodsToUse[getRandomValue(randomMethodsToUse.length - 1)]()
  }

  generatedPassword.value = shuffle(generatedPasswordHolder.split("")).join('')
  isPasswordGenerated.value = true;
  isPasswordCopied.value = false;
};

const copyTextToClipboard = () => {
  navigator.clipboard.writeText(generatedPassword.value);
  isPasswordCopied.value = true;
}

onMounted(() => {
  generatePassword();
})
</script>

<template>
  <div class="container d-flex flex-column justify-content-center align-items-center ">
    <div class="col-8 vh-100 mx-auto align-content-center">
      <div class="row justify-content-center py-3">
        <div class="col text-center">
          <h1>Password Sorcerer</h1>
        </div>
      </div>
      <div class="row justify-content-center pb-1">
        <div class="col-4">
          <div class="input-group">
            <!-- Replace this with an expandable text field -->
            <input type="text" class="form-control" placeholder="Generated Password" disabled
              v-model="generatedPassword" />
            <button class="btn" :class="isPasswordCopied ? 'btn-outline-success' : 'btn-outline-primary'"
              @click="copyTextToClipboard" :disabled="!isPasswordGenerated">
              <i :class="isPasswordCopied ? 'bi-clipboard-check' : 'bi-copy'"></i>
            </button>
          </div>
        </div>
      </div>
      <!-- Change this into an icon next to copy? -->
      <div class="row justify-content-center pt-1 pb-3">
        <div class="col-4">
          <button type="button" class="btn btn-primary w-100" :disabled="!isReadyToGeneratePassword"
            @click="generatePassword">Generate
            Password</button>
        </div>
      </div>
      <div class="row justify-content-center">
        <div class="col-4">
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

    </div>
  </div>
</template>

<style scoped></style>
