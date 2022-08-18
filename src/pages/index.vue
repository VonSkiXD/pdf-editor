<script setup lang="ts">
import * as pdfjsLib from 'pdfjs-dist'
import * as pdfjsWorker from 'pdfjs-dist/build/pdf.worker.entry'
pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker

const canItem = ref()

async function renderPdf() {
  const loadingTask = pdfjsLib.getDocument('https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf').promise
  const pdf = await loadingTask
  const firstPageNumber = 1
  const page = await pdf.getPage(firstPageNumber)
  const scale = 1.5
  const viewport = page.getViewport({ scale })

  const canvasC = canItem.value.getContext('2d')

  const renderContext = {
    canvasContext: canvasC,
    viewport,
  }

  const renderTask = page.render(renderContext)
  await renderTask.promise
}
onMounted(() => {
  renderPdf()
})
// await renderTask
</script>

<template>
  <div>
    <!-- <button @click="createPdf()">
      下载
    </button> -->
    <canvas ref="canItem" width="1920" height="1080" />
  </div>
</template>

<!-- <route lang="yaml">
meta:
  layout: default
</route> -->
