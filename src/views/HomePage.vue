<template>
  <ion-page>
    <ion-header class="app-header">
      <ion-toolbar class="header-toolbar">
        <ion-title>
          <span class="brand-mark">//</span> Encryption APP
        </ion-title>
        <div slot="end" class="header-status">
          <span class="status-dot"></span>
          ONLINE
        </div>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding page-content">
      <div class="background-art" aria-hidden="true">
        <span class="orbit orbit-one"></span>
        <span class="orbit orbit-two"></span>
        <span class="signal signal-one"></span>
        <span class="signal signal-two"></span>
      </div>

      <div class="container">
        <div class="intro-row">
          <div>
            <p class="eyebrow">CAESAR SHIFT</p>
            <h1>Make the message<br /><span>unreadable.</span></h1>
            <p class="description">A private channel for transforming text, one rotation at a time.</p>
          </div>
          <div class="cipher-badge" aria-hidden="true">
            <span>↗</span>
            <small>ROTATE<br />ALPHABET</small>
          </div>
        </div>

        <div class="workspace-label">
          <span>MESSAGE WORKSPACE</span>
          <span class="workspace-line"></span>
          <span class="workspace-id">SECURE / LOCAL</span>
        </div>

        <section class="workspace">
          <div class="input-panel">
            <div class="panel-heading">
             
              <span>INPUT TEXT</span>
              <span class="panel-corner">A—Z</span>
            </div>
            <ion-item>
              <ion-textarea
                v-model="text"
                label="Plaintext / Ciphertext"
                label-placement="stacked"
                placeholder="Type a message to begin..."
                :rows="6"
              ></ion-textarea>
            </ion-item>
            <div class="shift-row">
              <span class="shift-label">ROTATION KEY</span>
              <ion-item class="shift-input">
                <ion-input
                  v-model.number="shift"
                  type="number"
                  aria-label="Rotation key"
                ></ion-input>
              </ion-item>
              <span class="shift-unit">STEPS</span>
            </div>
          </div>

          <div class="action-panel">
            <div class="action-arrow" aria-hidden="true"></div>
            <div class="buttons">
              <ion-button expand="block" @click="encrypt">
                <span class="button-icon">+</span> ENCRYPT
              </ion-button>

              <ion-button expand="block" color="secondary" @click="decrypt">
                <span class="button-icon">−</span> DECRYPT
              </ion-button>
            </div>
          </div>

          <ion-card class="result-panel">
            <ion-card-header>
              <div class="panel-heading">
        
                <span>OUTPUT TEXT</span>
                <span class="panel-corner">READY</span>
              </div>
              <ion-card-title>{{ result ? 'Message transformed' : 'Awaiting transmission' }}</ion-card-title>
            </ion-card-header>

            <ion-card-content>
              <span v-if="result">{{ result }}</span>
              <span v-else class="empty-result">Your encrypted message will appear here.</span>
            </ion-card-content>
          </ion-card>
        </section>

        <ion-button
          class="clear-button"
          expand="block"
          fill="outline"
          color="danger"
          @click="clearAll"
        >
          CLEAR WORKSPACE
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
:global(body) {
  background: #07151c;
}

.page-content {
  --background: #07151c;
  --color: #ecf8f5;
  position: relative;
  overflow: hidden;
  background:
    radial-gradient(circle at 82% 12%, rgba(39, 212, 184, 0.16), transparent 25%),
    radial-gradient(circle at 8% 88%, rgba(255, 190, 92, 0.1), transparent 24%),
    repeating-linear-gradient(0deg, rgba(155, 225, 214, 0.035) 0 1px, transparent 1px 46px),
    repeating-linear-gradient(90deg, rgba(155, 225, 214, 0.035) 0 1px, transparent 1px 46px),
    #07151c;
}

.page-content::before {
  content: '';
  position: absolute;
  inset: 0;
  pointer-events: none;
  background: linear-gradient(115deg, transparent 0 48%, rgba(128, 255, 221, 0.05) 48.1% 48.3%, transparent 48.4%);
  mask-image: linear-gradient(to bottom, black, transparent 78%);
}

.background-art {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.orbit {
  position: absolute;
  border: 1px solid rgba(100, 236, 207, 0.18);
  border-radius: 50%;
  transform: rotate(-24deg);
}

.orbit-one {
  width: min(65vw, 620px);
  height: min(22vw, 210px);
  top: 7%;
  right: -10%;
}

.orbit-two {
  width: min(48vw, 450px);
  height: min(16vw, 150px);
  top: 12%;
  right: -3%;
  border-color: rgba(255, 198, 109, 0.2);
}

.signal {
  position: absolute;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #8affd5;
  box-shadow: 0 0 0 5px rgba(138, 255, 213, 0.08), 0 0 22px #4de6bd;
}

.signal-one {
  top: 18%;
  right: 17%;
}

.signal-two {
  bottom: 14%;
  left: 12%;
  background: #ffc66d;
  box-shadow: 0 0 0 5px rgba(255, 198, 109, 0.08), 0 0 22px #ffc66d;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  position: relative;
  z-index: 1;
}

h1 {
  text-align: center;
  margin-top: 20px;
  font-size: 28px;
  color: #f1fff9;
}

.description {
  text-align: center;
  color: #9dc0ba;
  margin-bottom: 25px;
}

ion-item {
  margin-bottom: 15px;
  --background: rgba(14, 39, 45, 0.86);
  --border-color: rgba(128, 214, 195, 0.25);
  --color: #ecf8f5;
  --highlight-color-focused: #71efc4;
  border-radius: 8px;
  backdrop-filter: blur(8px);
}

.buttons {
  margin-top: 20px;
}

ion-button {
  margin-bottom: 10px;
}

ion-card {
  margin-top: 25px;
  --background: rgba(16, 46, 51, 0.9);
  --color: #ecf8f5;
  border: 1px solid rgba(128, 214, 195, 0.2);
  border-radius: 8px;
  box-shadow: 0 18px 45px rgba(0, 0, 0, 0.2);
}

ion-card-content {
  min-height: 80px;
  word-break: break-word;
  font-size: 18px;
  color: #c9eee2;
}

@media (max-width: 600px) {
  .orbit-one {
    top: 4%;
    right: -52%;
    width: 130vw;
    height: 42vw;
  }

  .orbit-two {
    top: 8%;
    right: -35%;
    width: 100vw;
    height: 32vw;
  }

  .signal-one {
    right: 12%;
  }
}

.app-header {
  --background: #07151c;
  --color: #ecf8f5;
}

.header-toolbar {
  --background: rgba(7, 21, 28, 0.94);
  --border-color: rgba(128, 214, 195, 0.16);
  --min-height: 58px;
  border-bottom: 1px solid rgba(128, 214, 195, 0.16);
}

.header-toolbar ion-title {
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.2em;
}

.brand-mark {
  color: #71efc4;
  margin-right: 8px;
}

.header-status,
.workspace-id,
.panel-corner,
.eyebrow,
.workspace-label,
.shift-label,
.shift-unit {
  font-size: 10px;
  letter-spacing: 0.16em;
  font-weight: 700;
}

.header-status {
  color: #9dc0ba;
  margin-right: 16px;
}

.status-dot {
  display: inline-block;
  width: 6px;
  height: 6px;
  margin: 0 7px 1px 0;
  border-radius: 50%;
  background: #71efc4;
  box-shadow: 0 0 12px #71efc4;
}

.intro-row {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 24px;
  padding: 40px 0 34px;
}

.eyebrow {
  color: #ffc66d;
  margin: 0 0 12px;
}

h1 {
  text-align: left;
  margin: 0;
  color: #f1fff9;
  font-size: clamp(34px, 6vw, 58px);
  line-height: 0.98;
  letter-spacing: -0.02em;
}

h1 span {
  color: #71efc4;
}

.description {
  max-width: 350px;
  text-align: left;
  color: #9dc0ba;
  line-height: 1.5;
  margin: 18px 0 0;
}

.cipher-badge {
  display: grid;
  place-items: center;
  width: 86px;
  height: 86px;
  flex: 0 0 86px;
  border: 1px solid rgba(255, 198, 109, 0.5);
  color: #ffc66d;
  transform: rotate(6deg);
}

.cipher-badge span {
  font-size: 30px;
  line-height: 20px;
}

.cipher-badge small {
  font-size: 8px;
  line-height: 1.35;
  letter-spacing: 0.16em;
}

.workspace-label {
  display: flex;
  align-items: center;
  gap: 12px;
  color: #71efc4;
  margin-bottom: 12px;
}

.workspace-line {
  height: 1px;
  flex: 1;
  background: rgba(128, 214, 195, 0.24);
}

.workspace-id,
.panel-corner {
  color: #668b88;
}

.workspace {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 128px;
  gap: 14px;
  align-items: stretch;
}

.input-panel,
.result-panel {
  padding: 18px;
  background: rgba(10, 32, 39, 0.82);
  border: 1px solid rgba(128, 214, 195, 0.22);
  border-radius: 8px;
  backdrop-filter: blur(10px);
}

.input-panel {
  grid-column: 1 / -1;
}

.panel-heading {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #c9eee2;
  font-size: 11px;
  letter-spacing: 0.14em;
  font-weight: 700;
  margin-bottom: 16px;
}

.panel-number {
  color: #ffc66d;
  font-variant-numeric: tabular-nums;
}

.panel-corner {
  margin-left: auto;
  font-size: 9px;
}

.input-panel ion-item {
  margin: 0;
  --background: rgba(7, 21, 28, 0.7);
}

.shift-row {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 14px;
}

.shift-label {
  color: #9dc0ba;
}

.shift-input {
  width: 88px;
  min-height: 40px;
  margin: 0;
  --background: rgba(113, 239, 196, 0.09);
  --border-color: rgba(113, 239, 196, 0.35);
}

.shift-unit {
  color: #668b88;
}

.action-panel {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 16px;
}

.action-arrow {
  align-self: center;
  color: #ffc66d;
  font-size: 28px;
  line-height: 1;
}

.buttons {
  margin: 0;
}

ion-button {
  --border-radius: 5px;
  --box-shadow: none;
  margin-bottom: 10px;
  font-size: 11px;
  letter-spacing: 0.12em;
  font-weight: 700;
}

.button-icon {
  margin-right: 7px;
  font-size: 17px;
}

.result-panel {
  grid-column: 1 / -1;
  margin: 0;
  --background: rgba(16, 46, 51, 0.88);
  --color: #ecf8f5;
  box-shadow: 0 18px 45px rgba(0, 0, 0, 0.2);
}

.result-panel ion-card-header {
  padding: 0;
}

.result-panel ion-card-title {
  color: #f1fff9;
  font-size: 16px;
  margin-top: 18px;
}

.result-panel ion-card-content {
  min-height: 80px;
  padding: 16px 0 0;
  color: #c9eee2;
}

.empty-result {
  color: #668b88;
  font-size: 14px;
}

.clear-button {
  --color: #d99f68;
  --border-color: rgba(217, 159, 104, 0.45);
  margin-top: 18px;
}

@media (max-width: 600px) {
  .container {
    max-width: 100%;
  }

  .intro-row {
    padding-top: 28px;
  }

  .cipher-badge {
    width: 64px;
    height: 64px;
    flex-basis: 64px;
  }

  .cipher-badge span {
    font-size: 23px;
  }

  .workspace {
    display: block;
  }

  .action-panel {
    display: grid;
    grid-template-columns: 24px 1fr;
    align-items: center;
    gap: 10px;
    margin: 14px 0;
  }

  .action-arrow {
    transform: rotate(-90deg);
  }
}

/* Cipher Studio paper layout */
:global(body) {
  background: #f2f5ed;
}

.page-content {
  --background: #f2f5ed;
  --color: #344239;
  background-color: #f2f5ed;
  background-image: radial-gradient(#cdd5c8 0.8px, transparent 0.8px);
  background-size: 22px 22px;
}

.page-content::before,
.background-art {
  display: none;
}

.app-header {
  --background: rgba(242, 245, 237, 0.96);
  --color: #344239;
}

.header-toolbar {
  --background: transparent;
  --border-color: transparent;
  --min-height: 92px;
  max-width: 760px;
  margin: 0 auto;
  padding: 10px 22px;
}

.brand-icon {
  display: grid;
  place-items: center;
  width: 66px;
  height: 66px;
  border-radius: 50%;
  background: #cce66e;
  color: #25392b;
  font-family: Georgia, serif;
  font-size: 38px;
}

.brand-copy {
  display: flex;
  flex-direction: column;
  margin-left: 18px;
  color: #657269;
}

.brand-copy span {
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.12em;
}

.brand-copy strong {
  color: #304033;
  font-family: Georgia, serif;
  font-size: 29px;
  font-weight: 500;
  line-height: 1.1;
}

.header-tag {
  color: #69756d;
  font-size: 13px;
  line-height: 1.25;
  text-align: right;
}

.container {
  max-width: 716px;
}

.page-heading {
  display: none;
}

.workspace {
  display: block;
  overflow: hidden;
  margin: 0 0 34px;
  background: rgba(253, 254, 249, 0.9);
  border: 1px solid #d5ddd2;
  border-radius: 9px;
  box-shadow: 0 2px 7px rgba(79, 93, 78, 0.05);
}

.input-panel {
  padding: 36px 34px 32px;
  background: transparent;
  border: 0;
  border-bottom: 1px solid #dfe5dc;
  border-radius: 0;
  backdrop-filter: none;
}

.control-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 42px;
}

.control-group,
.shift-control {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.control-label {
  color: #68756b;
  font-size: 14px;
  font-weight: 700;
  letter-spacing: 0.1em;
}

.segmented-control {
  display: flex;
  min-height: 70px;
  padding: 5px;
  background: #edf0e9;
  border: 1px solid #dbe2d8;
  border-radius: 9px;
}

.segment {
  flex: 1;
  border: 0;
  border-radius: 6px;
  background: transparent;
  color: #6f7b73;
  font: inherit;
  font-size: 18px;
  cursor: pointer;
}

.segment.active {
  background: #fffefa;
  color: #304033;
  font-weight: 700;
  box-shadow: 0 2px 7px rgba(79, 93, 78, 0.12);
}

.segment:disabled {
  cursor: not-allowed;
  opacity: 0.72;
}

.shift-control {
  flex-direction: row;
  align-items: center;
  gap: 16px;
  margin-top: 36px;
}

.shift-control .control-label {
  align-self: flex-start;
  margin-top: 13px;
  margin-right: 1px;
}

.shift-value {
  width: 120px;
  height: 68px;
  padding: 0 16px;
  border: 1px solid #d2dcd0;
  border-radius: 8px;
  background: #fffefa;
}

.shift-value ion-input {
  --color: #344239;
  font-size: 20px;
}

.shift-unit {
  color: #7d887f;
  font-size: 18px;
}

.text-flow {
  padding: 37px 30px 34px;
}

.text-block {
  display: block;
}

.text-heading {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  margin-bottom: 20px;
}

.text-heading strong {
  color: #344239;
  font-size: 21px;
}

.text-heading span {
  color: #9aa59c;
  font-size: 16px;
}

.text-block ion-textarea {
  --background: #fbfcf8;
  --color: #3d4c41;
  --placeholder-color: #a9b1aa;
  --placeholder-opacity: 1;
  display: block;
  min-height: 334px;
  padding: 21px 24px;
  border: 1px solid #d9e0d6;
  border-radius: 8px;
  font-family: 'Courier New', monospace;
  font-size: 20px;
  line-height: 1.4;
}

.transform-button {
  display: block;
  margin: 22px auto 25px;
  border: 0;
  background: transparent;
  color: #899a7d;
  font-size: 28px;
  cursor: pointer;
}

.result-panel {
  display: none;
}

.result-box {
  min-height: 334px;
  padding: 21px 24px;
  overflow-wrap: anywhere;
  border: 1px solid #d9e0d6;
  border-radius: 8px;
  background: #fbfcf8;
  color: #3d4c41;
  font-family: 'Courier New', monospace;
  font-size: 20px;
  line-height: 1.4;
}

.empty-result {
  color: #a9b1aa;
}

.workspace-footer {
  padding: 26px 30px 25px;
  border-top: 1px solid #dfe5dc;
}

.workspace-footer p {
  margin: 0 0 30px;
  color: #7d887f;
  font-size: 16px;
}

.footer-actions {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 18px;
}

.text-action,
.copy-button {
  border: 0;
  background: transparent;
  color: #a3ada5;
  font: inherit;
  font-size: 17px;
  cursor: pointer;
}

.copy-button {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 18px 25px;
  border-radius: 7px;
  background: #aab7ae;
  color: #fffefa;
  font-weight: 700;
}

.copy-icon {
  width: 18px;
  height: 18px;
  border: 2px solid currentColor;
  border-radius: 3px;
  box-shadow: -5px -5px 0 -2px #aab7ae, -5px -5px 0 0 currentColor;
}

@media (max-width: 600px) {
  .header-toolbar {
    --min-height: 78px;
    padding: 8px 12px;
  }

  .brand-icon {
    width: 50px;
    height: 50px;
    font-size: 28px;
  }

  .brand-copy {
    margin-left: 11px;
  }

  .brand-copy span {
    font-size: 8px;
  }

  .brand-copy strong {
    font-size: 22px;
  }

  .header-tag {
    display: none;
  }

  .input-panel,
  .text-flow,
  .workspace-footer {
    padding-left: 18px;
    padding-right: 18px;
  }

  .control-grid {
    gap: 14px;
  }

  .segmented-control {
    min-height: 58px;
  }

  .segment {
    font-size: 15px;
  }

  .text-block ion-textarea,
  .result-box {
    min-height: 230px;
    padding: 16px;
    font-size: 16px;
  }

  .footer-actions {
    align-items: stretch;
    flex-wrap: wrap;
  }

  .copy-button {
    margin-left: auto;
  }
}
/* Restore the Signal Lab visual system after the reference layout. */
:global(body) {
  background: #07151c;
}

.page-content {
  --background: #07151c;
  --color: #ecf8f5;
  background: #07151c;
  background-image:
    radial-gradient(circle at 82% 12%, rgba(39, 212, 184, 0.16), transparent 25%),
    radial-gradient(circle at 8% 88%, rgba(255, 190, 92, 0.1), transparent 24%),
    repeating-linear-gradient(0deg, rgba(155, 225, 214, 0.035) 0 1px, transparent 1px 46px),
    repeating-linear-gradient(90deg, rgba(155, 225, 214, 0.035) 0 1px, transparent 1px 46px);
}

.page-content::before,
.background-art {
  display: block;
}

.app-header {
  --background: #07151c;
  --color: #ecf8f5;
}

.header-toolbar {
  --background: rgba(7, 21, 28, 0.94);
  --border-color: rgba(128, 214, 195, 0.16);
  --min-height: 58px;
  max-width: none;
  margin: 0;
  padding: 0;
}

.header-toolbar ion-title {
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.2em;
}

.brand-mark {
  color: #71efc4;
  margin-right: 8px;
}

.header-status,
.workspace-id,
.panel-corner,
.eyebrow,
.workspace-label,
.shift-label,
.shift-unit {
  font-size: 10px;
  letter-spacing: 0.16em;
  font-weight: 700;
}

.header-status {
  color: #9dc0ba;
  margin-right: 16px;
}

.status-dot {
  display: inline-block;
  width: 6px;
  height: 6px;
  margin: 0 7px 1px 0;
  border-radius: 50%;
  background: #71efc4;
  box-shadow: 0 0 12px #71efc4;
}

.container {
  max-width: 600px;
}

.intro-row {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 24px;
  padding: 40px 0 34px;
}

.eyebrow {
  color: #ffc66d;
  margin: 0 0 12px;
}

h1 {
  text-align: left;
  margin: 0;
  color: #f1fff9;
  font-size: clamp(34px, 6vw, 58px);
  line-height: 0.98;
}

h1 span {
  color: #71efc4;
}

.description {
  max-width: 350px;
  text-align: left;
  color: #9dc0ba;
  line-height: 1.5;
  margin: 18px 0 0;
}

.cipher-badge {
  display: grid;
  place-items: center;
  width: 86px;
  height: 86px;
  flex: 0 0 86px;
  border: 1px solid rgba(255, 198, 109, 0.5);
  color: #ffc66d;
  transform: rotate(6deg);
}

.cipher-badge span {
  font-size: 30px;
  line-height: 20px;
}

.cipher-badge small {
  font-size: 8px;
  line-height: 1.35;
  letter-spacing: 0.16em;
}

.workspace-label {
  display: flex;
  align-items: center;
  gap: 12px;
  color: #71efc4;
  margin-bottom: 12px;
}

.workspace-line {
  height: 1px;
  flex: 1;
  background: rgba(128, 214, 195, 0.24);
}

.workspace-id,
.panel-corner {
  color: #668b88;
}

.workspace {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 128px;
  gap: 14px;
  align-items: stretch;
  overflow: visible;
  margin: 0;
  background: transparent;
  border: 0;
  border-radius: 0;
  box-shadow: none;
}

.input-panel,
.result-panel {
  padding: 18px;
  background: rgba(10, 32, 39, 0.82);
  border: 1px solid rgba(128, 214, 195, 0.22);
  border-radius: 8px;
  backdrop-filter: blur(10px);
}

.input-panel {
  grid-column: 1 / -1;
  border-bottom: 1px solid rgba(128, 214, 195, 0.22);
}

.panel-heading {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #c9eee2;
  font-size: 11px;
  letter-spacing: 0.14em;
  font-weight: 700;
  margin-bottom: 16px;
}

.panel-number {
  color: #ffc66d;
  font-variant-numeric: tabular-nums;
}

.panel-corner {
  margin-left: auto;
  font-size: 9px;
}

.input-panel ion-item {
  margin: 0;
  --background: rgba(7, 21, 28, 0.7);
}

.shift-row {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-top: 14px;
}

.shift-label {
  color: #9dc0ba;
}

.shift-input {
  width: 88px;
  min-height: 40px;
  margin: 0;
  --background: rgba(113, 239, 196, 0.09);
  --border-color: rgba(113, 239, 196, 0.35);
}

.shift-unit {
  color: #668b88;
}

.action-panel {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 16px;
}

.action-arrow {
  align-self: center;
  color: #ffc66d;
  font-size: 28px;
  line-height: 1;
}

.buttons {
  margin: 0;
}

ion-button {
  --border-radius: 5px;
  --box-shadow: none;
  margin-bottom: 10px;
  font-size: 11px;
  letter-spacing: 0.12em;
  font-weight: 700;
}

.button-icon {
  margin-right: 7px;
  font-size: 17px;
}

.result-panel {
  display: block;
  grid-column: 1 / -1;
  margin: 0;
  --background: rgba(16, 46, 51, 0.88);
  --color: #ecf8f5;
  box-shadow: 0 18px 45px rgba(0, 0, 0, 0.2);
}

.result-panel ion-card-header {
  padding: 0;
}

.result-panel ion-card-title {
  color: #f1fff9;
  font-size: 16px;
  margin-top: 18px;
}

.result-panel ion-card-content {
  min-height: 80px;
  padding: 16px 0 0;
  color: #c9eee2;
}

.empty-result {
  color: #668b88;
  font-size: 14px;
}

.clear-button {
  --color: #d99f68;
  --border-color: rgba(217, 159, 104, 0.45);
  margin-top: 18px;
}

@media (max-width: 600px) {
  .container {
    max-width: 100%;
  }

  .intro-row {
    padding-top: 28px;
  }

  .cipher-badge {
    width: 64px;
    height: 64px;
    flex-basis: 64px;
  }

  .cipher-badge span {
    font-size: 23px;
  }

  .workspace {
    display: block;
  }

  .action-panel {
    display: grid;
    grid-template-columns: 24px 1fr;
    align-items: center;
    gap: 10px;
    margin: 14px 0;
  }

  .action-arrow {
    transform: rotate(-90deg);
  }
}
</style>