<script setup>
import * as pdfjsLib from 'pdfjs-dist'
import * as pdfjsWorker from 'pdfjs-dist/build/pdf.worker.entry'
pdfjsLib.GlobalWorkerOptions.workerSrc = pdfjsWorker

const canvasDiv = ref()
const pages = ref([])
const pdf = ref()
// const canItem = ref()
// const scale = 1.6
// const cHeight = ref(0)
// const cWidth = ref(0)
// let pages: PDFPageProxy[] | { render: (arg0: { canvasContext: any; viewport: any }) => void }[] = []

async function renderPdf() {
  const pageList = []
  // await pdfjsLib.getDocument('https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf')
  //   .promise.then((pdf) => {
  //     const numberPages = pdf.numPages
  //     for (let index = 1; index <= numberPages; index++)
  //       pdf.getPage(index).then(page => pageList.push(page))
  //     // pages.value = pageList
  //   })
  const pdf = await pdfjsLib.getDocument('/test-l.pdf').promise

  const numberPages = pdf.numPages
  for (let index = 1; index <= numberPages; index++)
    pageList.push(await pdf.getPage(index))

  pages.value = pageList
  pdf.value = pdf

  // const viewport = pageList[0].getViewport({ scale })

  // cHeight.value = viewport.height
  // cWidth.value = viewport.width

  // const canvasC = canItem.value.getContext('2d')

  // const renderContext = {
  //   canvasContext: canvasC,
  //   viewport,
  // }
  // pageList[0].render(renderContext)
}
onMounted(() => {
  // renderPdf()
})
</script>

<template>
  <div>
    <!-- <div v-for="(page, index) in pages" :key="index">
      <Page :page="page" :pdf="pdf" />
    </div> -->
    <!-- <Page :page="pages" :pdf="pdf" /> -->
    <!-- <canvas ref="canItem" :width="cWidth" :height="cHeight" /> -->
    <div ref="canvasDiv" class="flex justify-center ">
      <Page2 pdf-url="test-l.pdf" />
      <!-- <canvas ref="canItem" :width="cWidth" :height="cHeight" /> -->
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: default
</route>
