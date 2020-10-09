<template>
  <input
    type="file"
    accept="image/*"
    @change="readFile"
  >
  <p>
    <img :src="src" style="width:100%" />
  </p>
  <p>
    {{ imageText }}
  </p>
</template>

<script setup>
  import { ref, onMounted, onBeforeUnmount } from 'vue'
  import { createWorker } from 'tesseract.js'

  export const src = ref('')
  export const imageText = ref('')

  const reader = new FileReader()
  const image = document.createElement('img')
  const worker = createWorker({
    logger: m => console.log(m)
  })

  onMounted(async () => {
    await worker.load()
    await worker.loadLanguage('eng')
    await worker.initialize('eng')
  })

  onBeforeUnmount(async () => {
    await worker.terminate()
  })

  reader.onload = async event => {
    const imageSrc = event.target.result
    image.src = imageSrc
    src.value = imageSrc

    const { data: { text } } = await worker.recognize(imageSrc)
    imageText.value = text
  }

  export function readFile (event) {
    reader.readAsDataURL(event.target.files[0])
  }
</script>
