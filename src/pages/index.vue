<script setup lang="ts">
// import { PDFDocument, StandardFonts, rgb } from 'pdf-lib'
// import download from 'downloadjs'

// async function createPdf() {
//   const pdfDoc = await PDFDocument.create()
//   const timesRomanFont = await pdfDoc.embedFont(StandardFonts.TimesRoman)

//   const page = pdfDoc.addPage()
//   const { width, height } = page.getSize()
//   const fontSize = 30
//   page.drawText('VEE JIA IS **', {
//     x: 50,
//     y: height - 4 * fontSize,
//     size: fontSize,
//     font: timesRomanFont,
//     color: rgb(0, 0.53, 0.71),
//   })

//   const pdfBytes = await pdfDoc.save()
//   download(pdfBytes, 'created-in-js.pdf', 'application/pdf')
// }

// import { getDocument } from 'pdfjs-dist'
import * as pdfjsLib from 'pdfjs-dist'
import * as pdfjsWorker from 'pdfjs-dist/build/pdf.worker.entry'
pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker
const p = ref()
function pdfRead() {
  const loadingTask = pdfjsLib.getDocument('https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf').promise
  const pdf = await loadingTask.promise

  const firstPageNumber = 1

  const page = await pdf.getPage(firstPageNumber)

  const scale = 1.5
  const viewport = page.getViewport({ scale })
  const renderContext = {
    canvasContext: p,
    viewport,
  }
  const renderTask = page.render(renderContext)

  await renderTask.promise
}
pdfRead()
</script>

<template>
  <div>
    <!-- <button @click="createPdf()">
      下载
    </button> -->
    <canvas
      ref="p"
      width="{{window.innerWidth}}"
      height="{{window.innerHeight}}"
    />
    <img src="/favicon.svg" alt="1">
  </div>
</template>

<route lang="yaml">
meta:
  layout: default
</route>
