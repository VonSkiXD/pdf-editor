<script setup lang="ts">
import * as pdfjsLib from 'pdfjs-dist'
import * as pdfjsWorker from 'pdfjs-dist/build/pdf.worker.entry'
pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker
const p = ref()
console.log(p)
const loadingTask = pdfjsLib.getDocument('https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf').promise
const pdf = await loadingTask

const firstPageNumber = 1
console.log(pdf)
const page = await pdf.getPage(firstPageNumber)
console.log(page)
const scale = 1.5
const viewport = page.getViewport({ scale })
console.log(viewport)
// const p = document.getElementById('p1111')

const renderContext = {
  canvasContext: p,
  viewport,
}

const renderTask = page.render(renderContext)

await renderTask
</script>

<template>
  <div>
    <!-- <button @click="createPdf()">
      下载
    </button> -->
    <canvas
      id="p1111"
      width="200"
      height="100"
    />
    <img src="/favicon.svg" alt="1">
  </div>
</template>

<route lang="yaml">
meta:
  layout: default
</route>
