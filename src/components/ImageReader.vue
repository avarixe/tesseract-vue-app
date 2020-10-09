<template>
  <input
    type="file"
    accept="image/*"
    @change="readFile"
  >
  <p>
    <img :src="src" />
  </p>
</template>

<script setup>
  import { ref } from 'vue'

  export const src = ref('')
  const reader = new FileReader()
  const image = document.createElement('img')

  reader.onload = async event => {
    const imageSrc = event.target.result
    image.src = imageSrc
    src.value = imageSrc
  }

  export function readFile (event) {
    reader.readAsDataURL(event.target.files[0])
  }
</script>
