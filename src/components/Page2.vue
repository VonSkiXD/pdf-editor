<script setup>
import * as pdfjsLib from 'pdfjs-dist'
import * as pdfjsWorker from 'pdfjs-dist/build/pdf.worker.entry'
const props = defineProps(['pdfUrl'])
pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker

const divRef = ref()
const numberPages = ref()
let pdf

const scale = 1.6

const canItems = ref([])
async function renderPdf() {
  pdf = await pdfjsLib.getDocument(props.pdfUrl).promise
}
async function renderCanvas() {
  numberPages.value = pdf.numPages
}
async function renderPage() {
  for (let i = 0; i < pdf.numPages; i++) {
    const page = await pdf.getPage(i + 1)
    const viewport = page.getViewport({ scale })
    const canvas = divRef.value[i]
    canvas.height = viewport.height
    canvas.width = viewport.width
    const canvasC = canvas.getContext('2d')

    const renderContext = {
      canvasContext: canvasC,
      viewport,
    }
    page.render(renderContext)
  }
}
onMounted(async () => {
  await renderPdf()
  await renderCanvas()
  await renderPage()
})
</script>

<template>
  <!-- <canvasDiv /> -->
  <div>
    <canvas v-for="(index) in numberPages" ref="divRef" :key="index" />
  </div>
</template>
