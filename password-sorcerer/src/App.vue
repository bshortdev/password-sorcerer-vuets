<script setup lang="ts">
import { ref } from 'vue';
const minPasswordLength = 4;
const maxPasswordLength = 128;
const passwordLength = ref(minPasswordLength);

const setPasswordLengthFromRange = (event:Event) => {
  const eventTarget = event.target as HTMLInputElement
  passwordLength.value = eventTarget.valueAsNumber
}
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <h1>Password Sorcerer</h1>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <label for="passwordLengthRange" class="form-label">Password Length</label>
      </div>
      <div class="col d-flex gap-4">
        <!-- Need to have the range update as slider is sliding -->
        <input type="range" class="form-range" :min="minPasswordLength" :max="maxPasswordLength" id="passwordLengthRange" :valueAsNumber="passwordLength" @change="setPasswordLengthFromRange">
        <div>{{passwordLength}}</div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
