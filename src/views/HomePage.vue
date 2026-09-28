<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Encryption App</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">
      <div class="container">

        <h1>Encryption & Decryption</h1>

        <p class="description">
          Enter your plaintext or ciphertext below.
        </p>

        <ion-item>
          <ion-textarea
            v-model="text"
            label="Plaintext / Ciphertext"
            label-placement="stacked"
            placeholder="Enter your text here..."
            :rows="6"
          ></ion-textarea>
        </ion-item>

        <ion-item>
          <ion-input
            v-model.number="shift"
            type="number"
            label="Shift"
            label-placement="stacked"
            placeholder="Enter shift number"
          ></ion-input>
        </ion-item>

        <div class="buttons">
          <ion-button expand="block" @click="encrypt">
            ENCRYPT
          </ion-button>

          <ion-button expand="block" color="secondary" @click="decrypt">
            DECRYPT
          </ion-button>
        </div>

        <ion-card>
          <ion-card-header>
            <ion-card-title>Result</ion-card-title>
          </ion-card-header>

          <ion-card-content>
            {{ result }}
          </ion-card-content>
        </ion-card>

        <ion-button
          expand="block"
          fill="outline"
          color="danger"
          @click="clearAll"
        >
          CLEAR
        </ion-button>

      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'

import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonItem,
  IonTextarea,
  IonInput,
  IonButton,
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent
} from '@ionic/vue'

const text = ref('')
const result = ref('')
const shift = ref(3)

function caesarCipher(input: string, amount: number): string {
  const normalizedShift = ((amount % 26) + 26) % 26

  return input
    .split('')
    .map((char) => {
      const code = char.charCodeAt(0)

      // Uppercase letters A-Z
      if (code >= 65 && code <= 90) {
        return String.fromCharCode(
          ((code - 65 + normalizedShift) % 26) + 65
        )
      }

      // Lowercase letters a-z
      if (code >= 97 && code <= 122) {
        return String.fromCharCode(
          ((code - 97 + normalizedShift) % 26) + 97
        )
      }

      // Keep numbers, spaces, and symbols unchanged
      return char
    })
    .join('')
}

function encrypt() {
  result.value = caesarCipher(text.value, shift.value)
}

function decrypt() {
  result.value = caesarCipher(text.value, -shift.value)
}

function clearAll() {
  text.value = ''
  result.value = ''
  shift.value = 3
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  margin-top: 20px;
  font-size: 28px;
}

.description {
  text-align: center;
  color: #666;
  margin-bottom: 25px;
}

ion-item {
  margin-bottom: 15px;
}

.buttons {
  margin-top: 20px;
}

ion-button {
  margin-bottom: 10px;
}

ion-card {
  margin-top: 25px;
}

ion-card-content {
  min-height: 80px;
  word-break: break-word;
  font-size: 18px;
}
</style>