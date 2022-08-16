<script setup lang="ts">
import { PDFDocument, StandardFonts, rgb } from 'pdf-lib'
import download from 'downloadjs'

async function createPdf() {
  const pdfDoc = await PDFDocument.create()
  const timesRomanFont = await pdfDoc.embedFont(StandardFonts.TimesRoman)

  const page = pdfDoc.addPage()
  const { width, height } = page.getSize()
  const fontSize = 30
  page.drawText('VEE JIA IS **', {
    x: 50,
    y: height - 4 * fontSize,
    size: fontSize,
    font: timesRomanFont,
    color: rgb(0, 0.53, 0.71),
  })

  const pdfBytes = await pdfDoc.save()
  download(pdfBytes, 'created-in-js.pdf', 'application/pdf')
}
</script>

<template>
  <div>
    <button @click="createPdf()">
      下载
    </button>
  </div>
</template>

<route lang="yaml">
meta:
  layout: default
</route>
