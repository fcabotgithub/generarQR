<script setup>
import { ref } from 'vue'
import QRCode from 'qrcode'

const texto = ref('')
const qrDataUrl = ref('')
const error = ref('')

async function generarQR() {
  error.value = ''
  qrDataUrl.value = ''

  const contenido = texto.value.trim()
  if (!contenido) {
    error.value = 'Introduce un texto para generar el código QR.'
    return
  }

  try {
    qrDataUrl.value = await QRCode.toDataURL(contenido, {
      width: 256,
      margin: 2,
    })
  } catch {
    error.value = 'No se pudo generar el código QR.'
  }
}
</script>

<template>
  <main class="app">
    <h1>Generador QR</h1>

    <label class="field">
      <span>Texto</span>
      <textarea
        v-model="texto"
        rows="4"
        placeholder="Escribe aquí el contenido del QR..."
      />
    </label>

    <button type="button" class="btn" @click="generarQR">
      generarQR
    </button>

    <p v-if="error" class="error">{{ error }}</p>

    <section v-if="qrDataUrl" class="qr-result">
      <img :src="qrDataUrl" alt="Código QR generado" />
    </section>
  </main>
</template>
