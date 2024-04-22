<script setup lang="ts">
import { ref } from 'vue';
const passwordLength = ref(0);
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
      <div class="col">
        <input type="range" class="form-range" min="0" max="5" id="passwordLengthRange" v-model="passwordLength">
        <div>{{passwordLength}}</div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
