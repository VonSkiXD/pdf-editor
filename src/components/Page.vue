<script setup>
const props = defineProps(['page', 'pdf'])

const scale = 1.6

const cHeight = ref(0)
const cWidth = ref(0)

const canItem = ref()
function renderPdf() {
  console.log(props.pdf)
  console.log(props.page)
  const viewport = props.page.getViewport({ scale })

  cHeight.value = viewport.height
  cWidth.value = viewport.width

  const canvasC = canItem.value.getContext('2d')

  const renderContext = {
    canvasContext: canvasC,
    viewport,
  }
  props.page.render(renderContext)
}

onMounted(() => {
  renderPdf()
})
</script>

<template>
  <div>
    <div class="flex justify-center ">
      <canvas ref="canItem" :width="cWidth" :height="cHeight" />
    </div>
  </div>
</template>
